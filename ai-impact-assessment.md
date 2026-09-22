
# Enterprise AI Impact Assessment (AIA) & Risk Register

**Target System:** Natural Language Risk & RAG Engine  
**Assessment Date:** Q3 2026  
**Governance Standards:** NIST AI RMF 1.0 (MAP / MEASURE), EU AI Act (Annex III Point 5b), SR 11-7 Model Risk Management  
**Lead Evaluator:** Infrastructure Product Manager — Risk & Credit Platforms

---

## 1. System Classification & Regulatory Scope

| Regulation / Standard | Classification | Operational Requirement |
| :--- | :--- | :--- |
| **EU AI Act** | **High-Risk Auxiliary (Annex III Point 5b)** | Mandatory technical documentation, logging, fundamental rights assessment, and human oversight controls. |
| **SR 11-7 (Fed / OCC)** | **Model Risk Inventory Asset** | Formal model validation, conceptual soundness verification, and ongoing monitoring logs. |
| **ECOA / CFPB Circular** | **Explainability Requirement** | Zero black-box outputs; responses referencing credit decisions must state explicit policy factors. |
| **GLBA / PCI-DSS** | **Data Isolation Scope** | Zero raw consumer credit metrics or cardholder data permitted in vector stores or prompts. |

---

## 2. Risk Register & Mitigation Matrix

### Risk 1: Hallucinated Underwriting Parameters or Credit Policy Rules
* **Likelihood:** Medium | **Impact:** High | **Pre-Mitigation Risk:** High
* **Root Cause:** LLM generates plausible-sounding rule thresholds not backed by official credit policy docs.
* **Mitigation Control:** Enforced a **95% Groundedness threshold** on context retrieval. Implemented an automated label verification agent that validates generated rule IDs directly against code repository references before display.
* **Residual Risk:** Low | **Monitoring:** Daily automated regression test runs across 500 ground-truth credit policy query sets.

### Risk 2: Proxy Discrimination & Bias Exposure (ECOA / FCRA)
* **Likelihood:** Low | **Impact:** Critical | **Pre-Mitigation Risk:** High
* **Root Cause:** Vector retrieval surfacing stale policy docs containing correlated demographic proxies (e.g., geographic zip-code bias factors).
* **Mitigation Control:** Vector ingestion pipeline excludes prohibited demographic features and proxy variables. Data lineage tag enforces immediate deprecation of legacy policy documentation upon new release tags.
* **Residual Risk:** Minimal | **Monitoring:** Quarterly fair-lending bias audits on vector chunk metadata.

### Risk 3: Data Poisoning or PII Bleed into LLM Prompts
* **Likelihood:** Low | **Impact:** High | **Pre-Mitigation Risk:** High
* **Root Cause:** Underwriter pastes raw credit file containing consumer SSNs or financial accounts into the query interface.
* **Mitigation Control:** Client-side and gateway-level regex/NER sanitization layers redact financial identifiers before sending payloads to the embedding pipeline.
* **Residual Risk:** Minimal | **Monitoring:** Continuous real-time audit logs for redacted PII patterns.

---

## 3. Human-In-The-Loop (HITL) Sign-Off

* [x] **Infrastructure Product Manager:** Approved system boundaries, latency SLAs, and fallback logic.
* [x] **Model Risk Management (MRM) Lead:** Validated SR 11-7 conceptual soundness and model inventory entry.
* [x] **Chief Compliance Officer:** Confirmed alignment with ECOA, FCRA, and EU AI Act transparency rules.
VENDOR_LLM_EVALUATION_RUBRIC.md
Markdown
# Foundation Model Selection & Vendor Governance Rubric

**Purpose:** Evaluation framework for selecting third-party API models and open-weights LLMs for integration into credit decisioning platforms.  
**Target Scope:** RAG Pipelines, Policy Query Assistants, and Code-Generation Utilities

---

## 1. Evaluation Criteria & Weightings

| Evaluation Dimension | Weight | Core Requirement | Pass Threshold |
| :--- | :--- | :--- | :--- |
| **Data Privacy & Zero Retention** | **30%** | Contractual Zero Data Retention (ZDR); guaranteed zero training on API data; VPC isolation. | **Mandatory Pass** |
| **Explainability & JSON Stability**| **25%** | Deterministic JSON output adherence for rule parsing; verifiable reasoning paths. | Score ≥ 8.5/10 |
| **Regulatory & Security Standards**| **20%** | SOC 2 Type II, ISO 42001, SR 11-7 audit support, and data residency guarantees. | **Mandatory Pass** |
| **Latency & Availability SLA** | **15%** | p95 latency < 1.5s; 99.99% operational uptime guarantee. | Score ≥ 7.5/10 |
| **IP & Copyright Protection** | **10%** | Full uncapped legal indemnification against third-party IP/copyright infringement claims. | **Mandatory Pass** |

---

## 2. Model Evaluation Scorecard

| Criterion | Hosted Vendor API (Commercial) | Self-Hosted Open-Weights (In-VPC) |
| :--- | :--- | :--- |
| **Zero Data Retention (ZDR)** | **PASS** (Enforced via Enterprise Agreement) | **PASS** (Air-gapped deployment) |
| **JSON Schema Adherence** | 9.5 / 10 | 7.5 / 10 (Requires specialized fine-tuning) |
| **p95 Latency (RAG Context)** | 880ms | 1350ms (Requires dedicated GPU cluster) |
| **Cost Efficiency** | Pay-per-token ($0.0025 / 1k tokens) | Fixed infrastructure overhead ($12k/mo GPU) |
| **Copyright Indemnification** | Included in Enterprise SLA | Not Provided |
| **SR 11-7 Auditability** | Vendor provides SOC 2 + ISO 42001 | Full internal weight control & transparency |
| **Final Recommendation** | **APPROVED for Live RAG Operations** | **APPROVED for Internal Offline Testing** |

---

## 3. Governance Protocol for Model Updates

1. **Change Management Window:** Minimum 30-day side-by-side shadow deployment prior to redirecting production traffic to a new model version.
2. **Benchmark Verification:** Updated model versions must pass automated evaluation suites (groundedness, schema adherence, latency) with zero performance regression.
3. **Approval Gate:** Requires sign-off from Infrastructure PM, Model Risk Management (MRM), and InfoSec prior to endpoint updating.
