# System Card: Enterprise Decisioning Platform — Natural Language Risk & RAG Engine

**System Owner:** Infrastructure Product Manager — Risk & Credit Platforms  
**Platform Architecture:** Enterprise Microservices & Decision Engine  
**Deployment Target:** Underwriting Strategy Portal & Developer Portal  
**Regulatory Risk Classification:** High-Risk Auxiliary (EU AI Act Annex III Point 5b) | SR 11-7 Model Risk Inventory  
**Status:** In Production (GA)

---

## 1. System Overview & Strategic Intent

This system enables credit risk strategists, underwriters, and platform engineers to query complex credit scoring logic, decision trees, and regulatory policy rules using natural language. 

It extracts context from underlying decisioning knowledge graphs and schema registries, passes the context to a private VPC-hosted LLM endpoint, and generates answers with explicit citations back to source policy documentation.

### Permitted Use
* Natural language querying of credit decisioning rules, underwriting policy specs, and API parameters.
* Automated search across internal risk strategy documentation, schema definitions, and ETL pipelines.

### Prohibited Use
* Autonomous deployment or alteration of live credit scoring algorithms without human sign-off.
* Processing unmasked consumer PII, credit report primitives, or bank credentials through LLM inference endpoints.

---

## 2. Technical Stack & Pipeline Architecture

```text
[User Query] ➔ [ECOA/FCRA Prompt Guardrail] ➔ [Metadata Extraction]
                                                       │
                                                       ▼
[LLM Response] ◄── [Context Ingestion Pipeline] ◄── [Vector / Knowledge Graph Search]
       │
       ▼
[Output Guardrail] ➔ [Rule ID & Code Verification] ➔ [Audited Answer + Citations]

```

# System Card: Enterprise Decisioning Platform — Natural Language Risk & RAG Engine

**System Owner:** Infrastructure Product Manager — Risk & Credit Platforms  
**Platform Architecture:** Enterprise Microservices & Decision Engine  
**Deployment Target:** Underwriting Strategy Portal & Developer Portal  
**Regulatory Risk Classification:** High-Risk Auxiliary (EU AI Act Annex III Point 5b) | SR 11-7 Model Risk Inventory  
**Status:** In Production (GA)

---

## 1. System Overview & Strategic Intent

This system enables credit risk strategists, underwriters, and platform engineers to query complex credit scoring logic, decision trees, and regulatory policy rules using natural language. 

It extracts context from underlying decisioning knowledge graphs and schema registries, passes the context to a private VPC-hosted LLM endpoint, and generates answers with explicit citations back to source policy documentation.

### Permitted Use
* Natural language querying of credit decisioning rules, underwriting policy specs, and API parameters.
* Automated search across internal risk strategy documentation, schema definitions, and ETL pipelines.

### Prohibited Use
* Autonomous deployment or alteration of live credit scoring algorithms without human sign-off.
* Processing unmasked consumer PII, credit report primitives, or bank credentials through LLM inference endpoints.

---

## 2. Technical Stack & Pipeline Architecture

```text
[User Query] ➔ [ECOA/FCRA Prompt Guardrail] ➔ [Metadata Extraction]
                                                       │
                                                       ▼
[LLM Response] ◄── [Context Ingestion Pipeline] ◄── [Vector / Knowledge Graph Search]
       │
       ▼
[Output Guardrail] ➔ [Rule ID & Code Verification] ➔ [Audited Answer + Citations]
Foundation Models: Azure OpenAI GPT-4o / Claude 3.5 Sonnet (Deployed in private VPC with Zero Data Retention).

```

Embeddings: text-embedding-3-large (3072 dimensions).

Storage Layer: BigQuery Vector Search paired with Graph DB for decisioning logic and schema relationships.

Chunking Strategy: Hierarchical 512-token chunks with 15% overlap, tagged with policy domain and version metadata.

3. Data Privacy, Governance & Regulatory Controls
PII & GLBA Safeguards: Automated regex and Named Entity Recognition (NER) filters strip consumer PII (SSNs, account numbers, names) at the ingestion gateway before vector generation.

Multi-Tenant Isolation: Vector indices are strictly segregated using enterprise tenant keys at the metadata layer.

Data Limits: The model processes platform documentation and schema metadata only. Consumer queries are never retained for base model retraining.

4. Guardrails & Safety Controls
Input Filtering: Prompt-injection guardrails filter out adversarial payloads attempting to extract underlying system prompts or bypass regulatory parameters.

Groundedness Benchmark: System outputs must meet a minimum 95% Groundedness Index score against retrieved context before rendering.

Code-to-Rule Verification Agent: A CI/CD agent verifies generated rule identifiers and policy labels against actual source code references to prevent documentation drift.

Citation Enforcement: Responses lacking explicit source citations are blocked automatically.

5. Performance & Operational Impact
Engineering Speed: Reduced documentation lookup time for risk engineering teams, raising sprint throughput by 25%.

Retrieval Precision: Precision@K = 0.94 on standardized underwriting query benchmarks.

Latency SLA: Median (p50) is 850ms; 95th percentile (p95) is 1.8s.

Automated Testing: Build pipelines run 500 ground-truth compliance queries before staging deployment.

6. Fallbacks & Auditability
Low-Confidence Routing: If context retrieval confidence falls below 0.80, the system bypasses LLM generation and routes the user to standard document search or manual underwriting support.

Incident Alerting: Guardrail bypasses trigger immediate alerts to Platform Reliability Engineering.

Immutable Audit Trail: All prompt-response pairs, vector IDs, and confidence scores are logged to encrypted, append-only storage to satisfy SR 11-7 and EU AI Act audit requirements.
