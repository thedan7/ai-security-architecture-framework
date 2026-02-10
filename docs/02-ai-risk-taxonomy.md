# AI Risk Taxonomy for Financial Services

> **Author's note:** This taxonomy is my attempt to systematically catalogue AI-specific risks in financial services, building on NIST AI RMF, ISO 42001, and OWASP LLM Top 10 while extending them for banking-specific concerns. I've tried to be rigorous in mapping to established frameworks, but I want to be honest: some of these risk assessments reflect my best current understanding from research rather than years of direct experience managing AI risk in banking. I've grounded everything in real-world scenarios and published standards, but I expect to refine these assessments as I learn more. Corrections and challenges are welcome.

---

## 1. Purpose and Scope

This document defines a structured classification of AI-specific risks for financial services organisations. It is designed to serve three functions:

1. **Risk identification.** Provide CISOs, security architects, and risk teams with a comprehensive catalogue of risks that AI systems introduce - beyond what traditional information security and operational risk frameworks cover.

2. **Risk assessment.** Offer a consistent methodology for evaluating the severity of each risk in the context of specific AI use cases, so that risk treatment can be prioritised.

3. **Control traceability.** Create the foundation for the [Control Mapping](05-control-mapping.md) document by establishing the risk statements that controls must address, with explicit mapping to ISO 42001, NIST AI RMF, and OWASP LLM Top 10.

**Scope:** This taxonomy covers risks specific to or significantly amplified by AI/ML systems in financial services. It does not attempt to catalogue general information security risks (covered by ISO 27001, CPS 234, etc.) or general operational risks (covered by existing bank risk frameworks). Where AI risks overlap with existing risk categories, this document identifies the overlap and explains what is net-new.

**Visual reference:** The [AI Risk Taxonomy Diagram Structure](../diagrams/01-ai-risk-taxonomy-structure.md) provides detailed descriptions, banking examples, and mitigation approaches for each risk. This document provides the formal risk framework; the diagram document provides the narrative detail.

---

## 2. Why Financial Services Needs an AI-Specific Risk Taxonomy

Banks already have mature risk taxonomies. They manage credit risk, market risk, operational risk, compliance risk, and technology risk through frameworks that have been refined over decades of regulatory scrutiny. The question is whether these existing taxonomies adequately cover AI.

**My assessment: they don't, but not for the reasons most people think.**

The gap isn't that existing frameworks ignore AI entirely. SR 11-7 covers model risk. CPS 234 covers information security. Privacy legislation covers data protection. The gap is that AI introduces *interactions* between these risk categories that existing frameworks treat separately, and it introduces *novel risk mechanisms* that don't map cleanly to existing control frameworks.

Three examples of what I mean:

**Novel risk mechanisms.** Prompt injection is not a variant of SQL injection. It exploits the fundamental architecture of how LLMs process instructions and data in the same channel. Existing input validation controls don't address it. Existing application security testing doesn't detect it. It requires new controls designed specifically for LLM-based systems.

**Risk amplification.** Bias has always been a risk in credit decisioning. But a biased human loan officer affects hundreds of decisions per year. A biased AI model affects millions. The risk mechanism isn't new, but the scale, speed, and apparent objectivity of AI-driven bias fundamentally changes the risk profile.

**Cross-category interactions.** A training data quality issue (data risk) can cause a model to produce biased outputs (model risk), which creates discriminatory customer outcomes (compliance risk), which triggers regulatory action (compliance risk) and reputational damage (operational risk). These cascading effects cross the boundaries of traditional risk categories and require integrated risk assessment.

This taxonomy is designed to capture these AI-specific characteristics while mapping back to existing banking risk frameworks so that AI risk can be integrated into - not bolted onto - established risk management processes.

---

## 3. Methodology

### 3.1 Taxonomy Construction

This taxonomy was constructed through the following process:

1. **Framework analysis.** Systematic review of NIST AI RMF 1.0 risk categories, ISO/IEC 42001:2023 Annex A controls, OWASP LLM Top 10 2025 vulnerability categories, and MITRE ATLAS adversarial threat landscape.

2. **Financial services mapping.** Each identified risk was evaluated against financial services regulatory expectations: SR 11-7 (model risk), FFIEC AI/ML guidance, APRA CPS 234 (information security), CPS 220 (risk management), and the EU AI Act's classification of financial services AI as high-risk.

3. **Use case grounding.** Risks were validated against four representative banking AI use cases (detailed in [Banking Use Cases](06-banking-use-cases.md)): customer-facing chatbots, document processing, fraud detection, and credit decisioning. Any risk that couldn't be concretely demonstrated in at least one use case was either reframed or excluded.

4. **Consolidation.** Related risks were grouped into four top-level categories (Model, Data, Operational, Compliance) with five specific risks per category, yielding twenty distinct risk items. This structure is deliberately manageable - comprehensive enough to cover the landscape, concise enough to be practically useful.

### 3.2 What This Taxonomy Does Not Cover

- **General cybersecurity risks** that apply to all systems equally (e.g., DDoS, credential theft, insider threat). These are covered by existing frameworks and are only included here where AI creates a materially different risk profile.
- **Business strategy risks** from AI adoption (e.g., competitive disadvantage, talent acquisition). These are real but outside the scope of a security architecture framework.
- **Ethical and philosophical questions** about AI (e.g., consciousness, existential risk). This framework is concerned with practical, near-term risks in banking AI deployments.

---

## 4. Risk Assessment Methodology

### 4.1 Assessment Dimensions

Each risk in this taxonomy should be assessed across two dimensions for any given AI use case:

**Likelihood** - How probable is it that this risk materialises for this specific use case?

| Rating | Definition | Guidance |
|--------|-----------|----------|
| **High** | Expected to occur during the system's operational lifetime | The risk has been demonstrated in similar systems, or the conditions for it to occur are present by default |
| **Medium** | Plausible and has occurred in comparable contexts | The risk requires specific conditions or attacker capability, but these are realistic |
| **Low** | Theoretically possible but requires unusual circumstances | The risk requires sophisticated attack, unlikely conditions, or has not been demonstrated in comparable systems |

**Impact** - What is the consequence if this risk materialises?

| Rating | Definition | Guidance |
|--------|-----------|----------|
| **Critical** | Material financial loss, regulatory enforcement action, or systemic customer harm | The bank would face immediate regulatory scrutiny, potential fines, or class-action exposure |
| **High** | Significant operational disruption, reputational damage, or individual customer harm | The incident would require executive-level response and potential regulatory notification |
| **Medium** | Contained operational impact, limited customer exposure | The incident is manageable through existing incident response processes |
| **Low** | Minimal operational or customer impact | The incident is addressable through normal operational processes |

### 4.2 Severity Matrix

Combine likelihood and impact to determine overall severity:

|  | **Impact: Low** | **Impact: Medium** | **Impact: High** | **Impact: Critical** |
|--|---|---|---|---|
| **Likelihood: High** | Medium | High | Critical | Critical |
| **Likelihood: Medium** | Low | Medium | High | Critical |
| **Likelihood: Low** | Low | Low | Medium | High |

### 4.3 Assessment Context

Risk severity is **not static** - it varies significantly based on:

- **Use case characteristics.** A hallucination risk is Medium for an internal knowledge assistant with human review, but Critical for an automated customer advisory system.
- **Data sensitivity.** Data leakage risk is higher when the model is trained on customer PII than when trained on publicly available market data.
- **Autonomy level.** Excessive agency risk scales directly with the degree of autonomous action granted to the AI system.
- **Exposure.** Customer-facing systems have higher risk profiles than internal-only tools for most risk categories.

The risk register template in [Section 7](#7-risk-register-template) is designed to capture these contextual assessments per use case.

---

## 5. AI Risk Taxonomy

### 5.0 Taxonomy Overview

```
AI Risk Taxonomy for Financial Services
├── MODEL RISK - Risks from the AI model itself
│   ├── MR-01  Adversarial Attacks
│   ├── MR-02  Model Drift and Performance Degradation
│   ├── MR-03  Bias and Fairness Failures
│   ├── MR-04  Lack of Explainability
│   └── MR-05  Overfitting and Generalisation Failures
├── DATA RISK - Risks from AI training and operational data
│   ├── DR-01  Training Data Poisoning
│   ├── DR-02  Data Leakage and Memorisation
│   ├── DR-03  Insufficient Data Governance
│   ├── DR-04  Data Quality and Lineage Failures
│   └── DR-05  Unauthorised Data Access via AI
├── OPERATIONAL RISK - Risks from AI runtime behaviour
│   ├── OR-01  Prompt Injection and Jailbreaking
│   ├── OR-02  Hallucinations and Incorrect Outputs
│   ├── OR-03  Excessive Agency
│   ├── OR-04  Availability and Reliability Failures
│   └── OR-05  Third-Party Model Dependencies
└── COMPLIANCE RISK - Risks from regulatory and legal landscape
    ├── CR-01  Regulatory Uncertainty
    ├── CR-02  Explainability and Auditability Requirements
    ├── CR-03  Anti-Discrimination and Fairness Laws
    ├── CR-04  Data Sovereignty and Cross-Border Transfer
    └── CR-05  Intellectual Property Exposure
```

---

### 5.1 Model Risk

Risks arising from the AI model itself - its architecture, training process, learned behaviours, and inherent limitations. Financial services has an established foundation for model risk management through SR 11-7, but generative AI and LLMs introduce behaviours that traditional model validation was not designed to assess.

**Relationship to existing frameworks:** SR 11-7 defines model risk as "the potential for adverse consequences from decisions based on incorrect or misused model outputs." This definition covers AI models, but SR 11-7's validation methodologies assume models with stable, testable input-output relationships. LLMs are non-deterministic, context-dependent, and resist traditional validation approaches. The risks below extend SR 11-7's scope to address these characteristics.

| ID | Risk | Risk Statement | Default Likelihood | Default Impact | Key Indicators |
|----|------|---------------|-------------------|----------------|----------------|
| MR-01 | Adversarial Attacks | An attacker deliberately manipulates model inputs or training data to cause incorrect outputs, bypassing the model's intended behaviour through evasion, poisoning, or backdoor techniques | Medium | High | Unexpected model behaviour on edge-case inputs; statistical anomalies in training data distributions; model performance discrepancies between validation and production |
| MR-02 | Model Drift | Model accuracy degrades over time as real-world data distributions diverge from training data, producing increasingly unreliable outputs while maintaining high confidence scores | High | High | Declining performance metrics over time; increasing gap between predicted and actual outcomes; shifts in input feature distributions (detected via PSI, KS tests) |
| MR-03 | Bias and Fairness | The model produces systematically different outcomes for different demographic groups, creating discriminatory impact through direct use of protected attributes or proxy variables that correlate with them | High | Critical | Disparate approval/denial rates across demographic groups; differential error rates by population segment; proxy feature importance scores correlating with protected attributes |
| MR-04 | Lack of Explainability | Model decision logic cannot be sufficiently explained to satisfy regulatory requirements, customer rights to explanation, or internal model validation needs | High | High | Inability to produce feature attribution explanations; regulatory requests that cannot be satisfied; model validation failures due to insufficient conceptual soundness evidence |
| MR-05 | Overfitting | Model performs well on training/validation data but fails on production data that differs from the training distribution, particularly on edge cases and novel scenarios | Medium | Medium | Significant performance gap between validation and production metrics; failures concentrated on inputs outside the training distribution; degraded accuracy after deployment to new business units or geographies |

---

### 5.2 Data Risk

Risks arising from the data used to train, fine-tune, ground, and operate AI systems. In financial services, data risks are amplified by the sensitivity of the data involved and the regulatory obligations governing its use. Data is the foundation of every AI system - the quality, integrity, and governance of that data directly determines the trustworthiness of the system's outputs.

**Relationship to existing frameworks:** CPS 234 requires the identification and classification of information assets and mandates controls proportionate to their sensitivity. Privacy legislation governs the collection, use, and disclosure of personal information. These frameworks apply to AI training data, but they don't address AI-specific data risks such as training data memorisation, data poisoning, or the challenge of exercising data subject rights against a trained model. The risks below extend existing data governance to cover these AI-specific concerns.

| ID | Risk | Risk Statement | Default Likelihood | Default Impact | Key Indicators |
|----|------|---------------|-------------------|----------------|----------------|
| DR-01 | Training Data Poisoning | Malicious or corrupted data is introduced into training datasets, causing the model to learn incorrect patterns that persist through retraining cycles and are difficult to detect through standard validation | Low | Critical | Anomalous label distributions in training data; model behaviour discrepancies on specific data subsets; unexplained performance degradation in targeted categories |
| DR-02 | Data Leakage | The model memorises and can be induced to reproduce sensitive information from its training data, including customer PII, confidential business information, or proprietary data | High | Critical | Model outputs containing verbatim training data; successful membership inference attacks in testing; PII patterns detected in model outputs |
| DR-03 | Insufficient Data Governance | Lack of visibility into what data was used to train which models, whether that data was authorised for AI training, and whether data subject rights can be honoured against trained models | High | High | Inability to produce training data lineage on request; data subject deletion requests that cannot be verified against model training sets; training data used without documented purpose authorisation |
| DR-04 | Data Quality Failures | Training or inference data contains errors, inconsistencies, duplications, or gaps that degrade model performance and embed data quality issues into model predictions at scale | High | Medium | Declining model performance correlated with data pipeline changes; data quality metric failures at pipeline gates; discrepancies between training data statistics and known population characteristics |
| DR-05 | Unauthorised Data Access | AI system components (inference endpoints, training pipelines, service accounts) create new access paths to sensitive data that bypass traditional access controls | Medium | High | AI service accounts with broader data access than required; inference endpoints accessible without proper authentication; data access audit logs showing unexpected access patterns from AI components |

---

### 5.3 Operational Risk

Risks arising from the deployment, runtime behaviour, and operational management of AI systems in production. These risks materialise when AI moves from development into the real world, where models interact with users, integrate with business processes, and operate at scale. Many of these risks are unique to or significantly amplified by generative AI and LLMs.

**Relationship to existing frameworks:** Operational risk management in financial services is well-established. The challenge with AI operational risks is that several of them - prompt injection, hallucination, excessive agency - have no direct precedent in traditional systems. A SQL injection and a prompt injection both exploit input handling, but they require fundamentally different detection and mitigation approaches. The risks below identify where AI operational risks require genuinely new controls versus extensions of existing ones.

| ID | Risk | Risk Statement | Default Likelihood | Default Impact | Key Indicators |
|----|------|---------------|-------------------|----------------|----------------|
| OR-01 | Prompt Injection | Attackers craft inputs that override system instructions, bypass safety controls, or manipulate model behaviour through direct injection in user input or indirect injection via external data sources | High | High | Anomalous prompt patterns in logs; model outputs that reference or reveal system instructions; content filter bypasses detected in monitoring; unexpected model behaviours reported by users |
| OR-02 | Hallucinations | The model generates plausible but factually incorrect information, fabricated references, or invented data, with the same confidence and fluency as accurate outputs | High | High | User-reported factual errors in AI outputs; verification failures in automated fact-checking layers; fabricated citations or references in outputs; outputs that contradict authoritative source data |
| OR-03 | Excessive Agency | AI systems perform actions beyond their intended scope by exploiting available tools, APIs, or permissions in ways that were technically possible but not intended by design | Medium | Critical | AI-initiated actions outside expected patterns; tool or API calls not anticipated in system design; actions taken without required human approval; unexpected downstream effects from AI operations |
| OR-04 | Availability Failures | AI system components fail, degrade, or become unavailable due to infrastructure issues, capacity constraints, model serving failures, or upstream provider outages | Medium | High | Inference latency spikes; increased error rates from AI endpoints; upstream provider status page incidents; failover to degraded/non-AI processing paths |
| OR-05 | Third-Party Dependencies | Critical AI capabilities depend on external model providers whose behaviour, availability, pricing, and roadmap are outside the institution's control | High | Medium | Provider deprecation notices; model behaviour changes after provider updates; pricing changes affecting AI operating costs; provider outages affecting bank operations |

---

### 5.4 Compliance Risk

Risks arising from the evolving regulatory and legal landscape surrounding AI in financial services. Compliance risk is driven primarily by external factors - regulatory expectations, legal precedent, and societal norms - but managing it requires technical capabilities that must be designed into AI systems from the start. Retrofitting explainability, audit trails, or bias testing into a production AI system is significantly more costly and disruptive than building them in.

**Relationship to existing frameworks:** Financial services compliance is heavily regulated. The challenge is that AI-specific regulation is still emerging and fragmented across jurisdictions. Institutions face the dual risk of non-compliance with emerging requirements and over-investment in compliance measures that turn out to be unnecessary. The risks below focus on areas where regulatory direction is clearest and where the cost of non-compliance is highest.

| ID | Risk | Risk Statement | Default Likelihood | Default Impact | Key Indicators |
|----|------|---------------|-------------------|----------------|----------------|
| CR-01 | Regulatory Uncertainty | The absence of definitive AI-specific regulation creates ambiguity about compliance requirements, making it difficult to determine appropriate governance investment and creating risk of both under- and over-compliance | High | Medium | New AI-related regulatory consultations or guidance documents; peer institutions receiving regulatory inquiries about AI practices; divergent AI regulations across operating jurisdictions |
| CR-02 | Auditability Requirements | AI systems cannot produce sufficient audit trails, decision explanations, or transparency documentation to satisfy regulatory examinations, customer complaints, or internal model validation | High | High | Regulatory requests that cannot be fulfilled with current logging; customer complaints about unexplained decisions; model validation findings citing insufficient transparency; gaps between required and available decision records |
| CR-03 | Anti-Discrimination | AI systems produce outcomes that disproportionately affect protected groups, creating exposure under anti-discrimination and fair lending legislation - whether through direct use of protected attributes or proxy variables | Medium | Critical | Statistical disparities in AI-influenced outcomes across demographic groups; adverse action rates differing by protected characteristics; customer complaints alleging discriminatory treatment; advocacy group or media scrutiny of AI-driven outcomes |
| CR-04 | Data Sovereignty | AI data processing occurs across geographic boundaries in ways that breach data residency requirements, privacy regulations, or regulatory expectations about where customer data is processed and stored | Medium | High | AI data flows crossing jurisdictional boundaries; cloud provider infrastructure routing data outside expected regions; regulatory inquiries about offshore data processing; gaps between documented and actual data flow architectures |
| CR-05 | Intellectual Property | AI systems generate content that infringes third-party IP, reproduce copyrighted training material, or fail to protect the institution's own proprietary AI assets (prompts, fine-tuning data, system instructions) | Medium | Medium | AI outputs closely resembling third-party published content; successful extraction of system prompts or proprietary instructions; legal claims related to AI-generated content; competitor content patterns appearing in AI outputs |

---

## 6. Risk Interactions and Cascading Effects

AI risks don't exist in isolation. A key insight from building this taxonomy is that the most damaging scenarios involve cascading failures across risk categories. Understanding these interactions is essential for realistic risk assessment.

### 6.1 High-Impact Interaction Patterns

**Data quality to model bias to regulatory action (DR-04 + MR-03 + CR-03).** Poor training data quality (underrepresentation of certain demographic groups) leads to model bias (differential accuracy across populations), which creates discriminatory outcomes caught by fairness monitoring or external scrutiny, triggering regulatory investigation. This is arguably the most consequential cascade in banking AI because it connects technical debt to regulatory enforcement.

**Prompt injection to excessive agency to customer harm (OR-01 + OR-03 + CR-02).** A successful prompt injection attack manipulates an agentic AI system into performing actions outside its intended scope (sending communications, accessing data, initiating transactions). The lack of comprehensive audit trails makes it difficult to determine the full scope of the incident, complicating regulatory notification and customer remediation.

**Third-party dependency to availability failure to model drift (OR-05 + OR-04 + MR-02).** A model provider deprecates a model version, forcing migration to a new version with subtly different behaviour. During migration, service disruptions cause fallback to non-AI processing. The new model version introduces performance drift that isn't caught because monitoring baselines were calibrated to the previous version.

**Insufficient data governance to data leakage to IP exposure (DR-03 + DR-02 + CR-05).** Weak data governance allows proprietary or third-party copyrighted material into training data without proper authorisation tracking. The model memorises and reproduces this content in outputs. Without data lineage records, the institution cannot determine the source or scope of the exposure.

### 6.2 Interaction Matrix

The following matrix identifies which risk pairs have significant interaction effects. An **X** indicates that the materialisation of one risk significantly increases the likelihood or impact of the other.

|  | MR-01 | MR-02 | MR-03 | MR-04 | MR-05 | DR-01 | DR-02 | DR-03 | DR-04 | DR-05 | OR-01 | OR-02 | OR-03 | OR-04 | OR-05 | CR-01 | CR-02 | CR-03 | CR-04 | CR-05 |
|--|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **MR-01** | | | | | | X | | | | | | | | | | | | | | |
| **MR-02** | | | X | | | | | | X | | | X | | | | | X | | | |
| **MR-03** | | X | | X | | | | | X | | | | | | | | | X | | |
| **MR-04** | | | X | | | | | | | | | | | | | | X | X | | |
| **MR-05** | | X | | | | | | | X | | | X | | | | | | | | |
| **DR-01** | X | | X | | | | | | | | | | | | | | | | | |
| **DR-02** | | | | | | | | X | | | | | | | | | | | X | X |
| **DR-03** | | | | | | | X | | X | | | | | | | | X | | X | X |
| **DR-04** | | X | X | | X | | | | | | | X | | | | | | X | | |
| **DR-05** | | | | | | | X | | | | | | | | | | | | | |
| **OR-01** | | | | | | | X | | | X | | | X | | | | | | | |
| **OR-02** | | | | | | | | | | | | | | | | | X | | | |
| **OR-03** | | | | | | | | | | | X | | | | | | X | | | |
| **OR-04** | | | | | | | | | | | | | | | X | | | | | |
| **OR-05** | | X | | | | | | | | | | | | X | | | | | | |
| **CR-01** | | | | | | | | | | | | | | | | | | | | |
| **CR-02** | | | | X | | | | X | | | | | | | | | | | | |
| **CR-03** | | | X | X | | | | | X | | | | | | | | | | | |
| **CR-04** | | | | | | | | | | | | | | | | | | | | |
| **CR-05** | | | | | | | X | X | | | | | | | | | | | | |

**How to read this matrix:** Row risk materialising increases column risk. For example, DR-04 (data quality failures) increases the risk of MR-02 (model drift), MR-03 (bias), MR-05 (overfitting), OR-02 (hallucinations), and CR-03 (anti-discrimination exposure).

---

## 7. Risk Register Template

Use this template to assess each risk against a specific AI use case. A completed risk register should be produced for every AI system that undergoes formal risk assessment.

### Per-Risk Assessment Fields

| Field | Description |
|-------|-------------|
| **Risk ID** | Taxonomy identifier (e.g., MR-01, DR-03) |
| **Risk Name** | Short name from taxonomy |
| **Use Case** | The specific AI use case being assessed |
| **Contextual Likelihood** | High / Medium / Low - assessed for this specific use case |
| **Contextual Impact** | Critical / High / Medium / Low - assessed for this specific use case |
| **Overall Severity** | Derived from severity matrix |
| **Likelihood Rationale** | Why this likelihood rating for this use case |
| **Impact Rationale** | Why this impact rating for this use case |
| **Existing Controls** | Controls already in place that partially address this risk |
| **Control Gaps** | What additional controls are needed |
| **Residual Risk** | Expected severity after planned controls are implemented |
| **Risk Owner** | The individual accountable for managing this risk |
| **Review Frequency** | How often this risk assessment should be revisited |

### Example: Risk Assessment for Customer-Facing Chatbot

| Field | Value |
|-------|-------|
| **Risk ID** | OR-01 |
| **Risk Name** | Prompt Injection and Jailbreaking |
| **Use Case** | Customer-facing banking chatbot (account inquiries, product information) |
| **Contextual Likelihood** | **High** - The system accepts free-text input from unauthenticated or lightly authenticated users. Prompt injection is well-documented and requires no specialised tools or knowledge. |
| **Contextual Impact** | **High** - Successful injection could reveal system prompt contents, bypass content filters to produce harmful outputs under the bank's brand, or manipulate the chatbot into disclosing information from its RAG knowledge base that should be access-controlled. |
| **Overall Severity** | **Critical** |
| **Likelihood Rationale** | Customer-facing LLMs are the highest-risk surface for prompt injection. The attack requires only natural language input and is actively researched and shared in public communities. Indirect injection via documents or URLs the chatbot processes adds a second attack vector. |
| **Impact Rationale** | Outputs are visible to customers under the bank's brand. Reputational risk from manipulated outputs is significant. Data exfiltration via injection could constitute a notifiable data breach. System prompt disclosure reveals security architecture details. |
| **Existing Controls** | Azure AI Content Safety filters; input length limits; rate limiting per session |
| **Control Gaps** | No prompt injection-specific detection; no output validation layer; no system prompt hardening; no red-teaming program for LLM systems |
| **Residual Risk** | High (even with additional controls, prompt injection cannot be fully eliminated for customer-facing LLMs - defence in depth reduces but does not eliminate risk) |
| **Risk Owner** | Head of Digital Security |
| **Review Frequency** | Monthly (active threat landscape evolution) |

---

## 8. Mapping to Banking Risk Frameworks

This section maps the AI risk taxonomy to existing banking risk management frameworks, identifying where AI risks are already partially covered and where net-new risk management is required.

### 8.1 SR 11-7 (Model Risk Management)

SR 11-7 is the most directly relevant existing framework for AI model risk. Published by the Federal Reserve in 2011, it predates modern AI/ML but its principles are broadly applicable.

| SR 11-7 Element | AI Risks Covered | Gaps for AI/LLMs |
|-----------------|-----------------|-------------------|
| **Model development (S3)** | MR-05 (overfitting), DR-04 (data quality) | Does not address foundation model selection, prompt engineering as model development, or fine-tuning governance |
| **Model validation (S4)** | MR-01 (adversarial), MR-03 (bias), MR-04 (explainability) | Validation methodologies assume deterministic models; LLM non-determinism requires new validation approaches |
| **Ongoing monitoring (S5)** | MR-02 (model drift) | Does not address prompt injection monitoring, hallucination detection, or third-party model behaviour changes |
| **Governance (S6)** | General governance | Does not address AI-specific roles (AI ethics, prompt engineering review) or the speed of LLM model iteration cycles |

### 8.2 APRA CPS 234 (Information Security)

| CPS 234 Element | AI Risks Covered | Gaps for AI/LLMs |
|-----------------|-----------------|-------------------|
| **Information asset identification (Para 25-28)** | DR-03 (data governance), DR-05 (unauthorised access) | Does not classify AI models themselves as information assets; training datasets not explicitly addressed |
| **Information security capability (Para 14-17)** | OR-04 (availability) | Does not address AI-specific security capabilities (prompt injection defence, output filtering, model red-teaming) |
| **Access management (Para 33-35)** | DR-05 (unauthorised access) | Does not address AI-specific access patterns (inference endpoint access, training pipeline permissions, RAG knowledge base access) |
| **Third-party management (Para 36-41)** | OR-05 (third-party dependencies), CR-04 (data sovereignty) | Does not address AI model provider-specific risks (model behaviour changes, deprecation, training data provenance) |

### 8.3 NIST AI RMF 1.0

The NIST AI RMF is the most comprehensive AI-specific framework and maps directly to most risks in this taxonomy. See the cross-reference table in the [Diagram Structure](../diagrams/01-ai-risk-taxonomy-structure.md#cross-reference-summary) for detailed mappings.

**Key alignment:** NIST AI RMF's four functions (GOVERN, MAP, MEASURE, MANAGE) provide the process framework within which this taxonomy's risks should be managed. This taxonomy provides the *what* (risk identification); NIST AI RMF provides the *how* (risk management process).

### 8.4 ISO/IEC 42001:2023

ISO 42001 provides the management system framework for AI. Its Annex A controls map to specific risks in this taxonomy as detailed in the [Control Mapping](05-control-mapping.md) document. The key structural alignment:

| ISO 42001 Annex A Area | Taxonomy Risks Addressed |
|------------------------|-------------------------|
| **A.6 - AI system lifecycle** | MR-01 through MR-05 (model risk category) |
| **A.7 - Data for AI systems** | DR-01 through DR-05 (data risk category) |
| **A.8 - Information for interested parties** | CR-02 (auditability), CR-03 (fairness), CR-04 (sovereignty) |
| **A.10 - Third-party and customer** | OR-05 (third-party dependencies) |

---

## 9. Using This Taxonomy

### 9.1 For AI Project Risk Assessments

1. Identify the AI use case and classify it by exposure level (customer-facing, internal-facing, autonomous, human-in-the-loop).
2. Walk through all 20 risks and assess contextual likelihood and impact using the methodology in [Section 4](#4-risk-assessment-methodology).
3. Complete the risk register template in [Section 7](#7-risk-register-template) for each risk rated Medium or above.
4. Identify risk interactions using the matrix in [Section 6](#6-risk-interactions-and-cascading-effects) - cascading risks may require a higher severity rating than the individual assessment suggests.
5. Use the completed risk register to inform control selection from the [Control Mapping](05-control-mapping.md).

### 9.2 For Board and CISO Reporting

Focus on the category-level summary rather than individual risks. Report:
- Number of AI use cases assessed
- Distribution of Critical/High/Medium/Low risks across the portfolio
- Top risk concentrations (which risk categories appear most frequently at Critical/High)
- Trend over time as risk assessments are updated
- Control gap status (number and severity of identified gaps versus remediated gaps)

### 9.3 For Regulatory Engagement

This taxonomy's explicit mapping to ISO 42001, NIST AI RMF, and existing prudential standards (SR 11-7, CPS 234) is designed to support regulatory conversations. When engaging with regulators on AI governance:
- Reference the taxonomy structure to demonstrate systematic risk identification
- Use the banking framework mapping in [Section 8](#8-mapping-to-banking-risk-frameworks) to show how AI risk management extends (rather than replaces) existing risk frameworks
- Present completed risk registers as evidence of use-case-level risk assessment

---

## 10. Taxonomy Maintenance

This taxonomy should be treated as a living document. The AI threat landscape is evolving rapidly, and new risk categories will emerge.

**Triggers for taxonomy review:**
- New OWASP LLM Top 10 release or significant update
- New NIST AI RMF profiles or guidance
- AI-specific prudential guidance from APRA or other relevant regulators
- Significant AI security incident in financial services (industry-wide learning)
- Introduction of new AI capabilities or architectures (e.g., new agentic patterns, multi-modal systems)
- Completion of threat modelling for a new use case that reveals risks not adequately captured

**Review cadence:** Quarterly review of the full taxonomy against emerging threats and regulatory developments. Ad-hoc updates as triggered by the events above.

---

**Previous document:** [Executive Summary](01-executive-summary.md)

**Next document:** [Azure AI Security Architecture](03-azure-ai-security-architecture.md) - Reference architectures for securing AI workloads on Azure.

**Related:** [AI Risk Taxonomy Diagram Structure](../diagrams/01-ai-risk-taxonomy-structure.md) - Detailed descriptions, banking examples, and mitigation approaches for each risk.
