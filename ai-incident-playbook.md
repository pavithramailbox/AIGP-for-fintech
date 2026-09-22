# Production AI Incident Response & Fallback Playbook

**Owner:** Infrastructure PM & Platform Reliability Engineering (PRE)  
**Target Services:** Credit Decisioning Natural Language Engine & RAG Microservices  
**Severity Classification:** Sev-1 (Critical Regulatory/Guardrail Breach), Sev-2 (Performance Degradation)

---

## 1. Trigger Conditions & Circuit Breakers

```text
[Production Alert] ➔ [Automated Circuit Breaker Triggered?]
                              │
               ┌──────────────┴──────────────┐
               ▼                             ▼
        YES (Sev-1 Breach)            NO (Sev-2 Degradation)
               │                             │
               ▼                             ▼
   [Switch to Static Fallback]    [Throttle Traffic / Route to Human Queue]
Sev-1 (Critical):

Hallucinated credit policy rule or score threshold rendered to an active underwriting session.

PII or credit metric exposure detected in outgoing prompt logs.

System hallucination rate exceeding 2% across a rolling 30-minute window.

Sev-2 (Degradation):

Context retrieval confidence score dropping below 0.80 for >5% of active sessions.

p95 inference latency exceeding 2.5s over a 15-minute window.

2. Immediate Incident Response Protocol
Step 1: Automated Circuit Breaker Activation (0–2 Minutes)
Action: API Gateway automatically routes query requests away from the LLM microservice to the static documentation search engine.

User Experience: Displays verified, static policy documentation with the banner: "Assistant undergoing maintenance. Displaying pre-verified policy references."

Step 2: Isolation & Triage (2–15 Minutes)
On-call PRE and Infrastructure PM paged via PagerDuty.

Freeze vector store update pipelines to prevent potential index corruption.

Isolate session logs and prompt-response pairs associated with the alert timestamp.

Step 3: Root-Cause Analysis & Remediation (15–60 Minutes)
If Data Issue: Roll back vector indices to the previous daily snapshot.

If Model/Prompt Issue: Increase output guardrail strictness or revert to the secondary backup model endpoint.

If Guardrail Bypass: Update input sanitization regex patterns and push updated system prompt constraints to staging.

3. Post-Incident & Regulatory Reporting Requirements
Root Cause Analysis (RCA): Completed within 24 hours and submitted to Model Risk Management (MRM).

Regulatory Compliance Check: If PII exposure or adverse impact occurred, notify the Data Protection Officer within 24 hours per GDPR/GLBA protocols.

CI/CD Suite Update: Convert the incident query payload into a permanent ground-truth test case in the regression testing pipeline.
