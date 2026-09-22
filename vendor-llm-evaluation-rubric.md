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
