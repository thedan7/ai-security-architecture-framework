# AI Risk Taxonomy for Financial Services - Diagram Structure

> **Purpose:** Reference document for building the visual AI risk taxonomy diagram in draw.io. Defines the four top-level risk categories, twenty individual risks, and the metadata needed for each node in the diagram. This document is the authoritative source for risk definitions - the diagram is the visual representation.

> **Standards key:** References use the format `[Standard] Section/ID`. ISO = ISO/IEC 42001:2023, NIST = NIST AI RMF 1.0, OWASP = OWASP LLM Top 10 2025.

---

## Diagram Layout

```
                    ┌──────────────────────────┐
                    │   AI RISK TAXONOMY FOR    │
                    │   FINANCIAL SERVICES      │
                    └─────────┬────────────────┘
                              │
          ┌───────────┬───────┴───────┬───────────┐
          ▼           ▼               ▼           ▼
    ┌──────────┐ ┌──────────┐ ┌────────────┐ ┌────────────┐
    │  MODEL   │ │   DATA   │ │ OPERATIONAL│ │ COMPLIANCE │
    │   RISK   │ │   RISK   │ │    RISK    │ │    RISK    │
    │ (5 risks)│ │ (5 risks)│ │  (5 risks) │ │  (5 risks) │
    └──────────┘ └──────────┘ └────────────┘ └────────────┘
```

**Colour coding for diagram:**
- Model Risk: Red (`#E74C3C`)
- Data Risk: Orange (`#E67E22`)
- Operational Risk: Blue (`#3498DB`)
- Compliance Risk: Purple (`#9B59B6`)

**Severity indicators per risk:**
- High: Filled node background
- Medium: Hatched/striped background
- Low: Outline only

---

## 1. MODEL RISK

Risks arising from the AI model itself - its architecture, training process, learned behaviours, and limitations. Financial services regulators already have a foundation for model risk management through SR 11-7, but generative AI and LLMs introduce model behaviours that traditional model validation approaches were not designed to assess.

---

### 1.1 Adversarial Attacks (Evasion, Poisoning, Backdoors)

**Description:** Attackers deliberately manipulate model inputs or training data to cause incorrect outputs. Evasion attacks craft inputs that bypass the model at inference time. Poisoning attacks corrupt training data to shift model behaviour. Backdoor attacks embed hidden triggers that activate under specific conditions.

**Banking example:** A fraud detection ML model is trained on transaction data that an insider has subtly modified, introducing a backdoor where transactions from specific merchant category codes are consistently classified as legitimate. The model passes standard validation testing but fails to flag fraudulent transactions matching the backdoor pattern.

**Primary mitigation:** Adversarial testing and red-teaming during model validation, input validation and anomaly detection at inference time, training data integrity verification through cryptographic hashing and provenance tracking.

**Standards mapping:**
- NIST AI RMF: MANAGE 2.2 (mechanisms to detect and respond to AI risks)
- ISO 42001: A.6.2.6 (AI system verification and validation)
- OWASP LLM Top 10: LLM04 (Data and Model Poisoning)

---

### 1.2 Model Drift and Performance Degradation

**Description:** Model accuracy degrades over time as the statistical properties of real-world data diverge from the data the model was trained on. In financial services, this can happen rapidly due to changing customer behaviour, market conditions, economic cycles, or shifts in fraud patterns. Drift is insidious because the model continues to produce outputs with high confidence even as those outputs become less reliable.

**Banking example:** A credit decisioning model trained on pre-pandemic economic data begins systematically misclassifying risk profiles as employment patterns and income distributions shift. The model's approval rates and default predictions gradually diverge from reality, but because the drift is gradual, it isn't caught until quarterly model review reveals a significant increase in early-stage delinquencies.

**Primary mitigation:** Continuous model monitoring with statistical drift detection (PSI, KS tests, KL divergence), automated alerting on performance threshold breaches, scheduled retraining with updated data, and canary deployment patterns for model updates.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.6 (AI system performance monitored on an ongoing basis)
- ISO 42001: A.6.2.7 (AI system operation and monitoring)
- SR 11-7: Section 5 (ongoing monitoring)

---

### 1.3 Bias and Fairness Issues

**Description:** AI models can encode, amplify, and systematise biases present in training data, feature selection, or model architecture. In financial services, bias in AI systems can create discriminatory outcomes in lending, insurance, and customer treatment that violate fair lending laws and anti-discrimination requirements. Unlike human bias, algorithmic bias operates at scale and with apparent objectivity, making it both more impactful and harder to challenge.

**Banking example:** A customer service chatbot routing system learns from historical interaction data that certain postcodes correlate with simpler queries, and begins routing customers from those areas to lower-skilled support tiers. The postcodes correlate with demographic characteristics, creating a systematic difference in service quality that constitutes indirect discrimination - even though no protected attributes are used as explicit features.

**Primary mitigation:** Bias testing across protected attributes before deployment (disparate impact analysis, equalised odds testing), ongoing fairness monitoring in production, diverse and representative training datasets, and human review gates for high-impact decisions.

**Standards mapping:**
- NIST AI RMF: MAP 2.3 (scientifically valid approaches to identify AI risks), MEASURE 2.11 (fairness assessments)
- ISO 42001: A.6.2.4 (assessing AI system impacts on interested parties)
- OWASP LLM Top 10: LLM09 (Misinformation) - in the context of systematically biased outputs

---

### 1.4 Lack of Explainability and Interpretability

**Description:** Complex AI models, particularly deep learning and large language models, operate as effective black boxes where the relationship between inputs and outputs cannot be easily explained. In financial services, this creates tension with regulatory expectations for model transparency, customer rights to explanations of adverse decisions, and internal requirements for model validation and audit.

**Banking example:** A credit decisioning support system using an LLM to synthesise applicant data recommends declining a mortgage application. The relationship manager cannot explain to the customer *why* the recommendation was made, the model risk team cannot validate the decision logic against credit policy, and the compliance team cannot demonstrate to regulators that the decision was free from prohibited discrimination.

**Primary mitigation:** Use inherently interpretable models where regulatory requirements demand explainability, implement post-hoc explanation methods (SHAP, LIME) for complex models, maintain decision audit trails that capture model inputs and outputs alongside the reasoning chain, and establish clear policies on which use cases require what level of explainability.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.2 (transparency requirements), MEASURE 2.7 (AI system outputs are interpretable)
- ISO 42001: A.6.2.3 (AI system transparency)
- SR 11-7: Section 4 (model validation - conceptual soundness)

---

### 1.5 Overfitting and Generalisation Failures

**Description:** Models that perform exceptionally well on training and test data but fail to generalise to real-world conditions or edge cases. Overfitting means the model has memorised patterns specific to the training dataset rather than learning generalisable relationships. In production, this manifests as unexpected failures when the model encounters scenarios that differ even slightly from its training distribution.

**Banking example:** A document processing model trained to extract key terms from loan agreements performs with 99% accuracy on the standardised document templates used in training. When deployed to process agreements from a newly acquired business unit using different document formats, accuracy drops to below 70%, causing incorrect data extraction that flows into downstream credit risk calculations.

**Primary mitigation:** Rigorous train/validation/test splits with holdout data representative of production diversity, cross-validation during development, ongoing monitoring of production accuracy versus validation accuracy, and staged rollout with representative pilot populations.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.5 (AI system robustness and resilience)
- ISO 42001: A.6.2.6 (AI system verification and validation)
- SR 11-7: Section 4 (outcomes analysis)

---

## 2. DATA RISK

Risks arising from the data used to train, fine-tune, and operate AI systems. In financial services, data risks are amplified by the sensitivity of the data involved (customer PII, financial records, transaction histories) and the regulatory obligations governing its use. Data is the foundation of every AI system - compromised data means compromised decisions.

---

### 2.1 Training Data Poisoning

**Description:** Malicious actors deliberately introduce corrupted, misleading, or manipulated data into training datasets to influence model behaviour. Unlike adversarial attacks at inference time, data poisoning attacks happen during training and can be extremely difficult to detect because the model learns the poisoned patterns as if they were legitimate. The attack persists through model retraining cycles unless the poisoned data is identified and removed.

**Banking example:** An attacker with access to a transaction labelling pipeline systematically mislabels a small percentage of known fraudulent transactions as legitimate. The proportion is small enough to pass data quality checks, but sufficient to reduce the fraud detection model's sensitivity to specific fraud patterns - effectively creating a blind spot that can be exploited at scale.

**Primary mitigation:** Training data provenance and integrity verification, data pipeline access controls with segregation of duties, statistical analysis of label distributions before training, anomaly detection on training data contributions, and data quality gates in the ML pipeline.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.6 (data quality and integrity)
- ISO 42001: A.7.4 (data for AI systems), A.7.5 (data quality)
- OWASP LLM Top 10: LLM04 (Data and Model Poisoning)

---

### 2.2 Data Leakage (PII, Confidential Information in Training Data)

**Description:** AI models can memorise and reproduce sensitive information from their training data, including customer PII, confidential business information, and proprietary data. Large language models are particularly susceptible because they are trained on vast corpora and can regurgitate training data verbatim when prompted in specific ways. This creates data breach risk even when the model itself is considered the product rather than the data.

**Banking example:** A customer service chatbot fine-tuned on historical support transcripts memorises specific customer account details, complaint narratives, and resolution outcomes from the training data. A user discovers that carefully crafted prompts can cause the chatbot to reveal other customers' names, account numbers, and complaint details - constituting a data breach under the Privacy Act without any traditional database being compromised.

**Primary mitigation:** Data sanitisation and PII scrubbing before training, differential privacy techniques during fine-tuning, membership inference testing to detect memorisation, output filtering for sensitive data patterns (account numbers, PII formats), and strict data classification controls on training data selection.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.7 (privacy norms and values), MANAGE 2.2 (risk response)
- ISO 42001: A.7.4 (data for AI systems), A.8.4 (privacy)
- OWASP LLM Top 10: LLM06 (Sensitive Information Disclosure)

---

### 2.3 Insufficient Data Governance

**Description:** AI systems are built on data from multiple sources with varying quality, provenance, and usage rights. Without rigorous data governance, organisations lose visibility into what data was used to train which models, whether that data was authorised for that purpose, and whether downstream regulatory obligations (consent, retention, deletion rights) can be satisfied. This is a governance gap rather than a technical vulnerability, but it creates systemic risk.

**Banking example:** An AI team builds a customer churn prediction model using data from the CRM, transaction history, and call centre logs. Six months later, a regulatory inquiry requires the bank to demonstrate that all data used in the model was collected with appropriate consent and that customers who exercised deletion rights under privacy legislation have had their data removed - including from model training sets. Without data lineage records, the bank cannot provide this assurance.

**Primary mitigation:** Data catalogue and lineage tracking for all AI training data, consent and purpose mapping before data is used in training, integration with data subject rights processes (deletion, correction), and mandatory data impact assessments for new AI data sources.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.7 (privacy values), MAP 3.4 (data provenance)
- ISO 42001: A.7.4 (data for AI systems), A.7.2 (data management process)
- CPS 234: Para 25-28 (information asset identification and classification)

---

### 2.4 Data Quality and Lineage Issues

**Description:** AI model quality is fundamentally bounded by the quality of its training data. Incomplete, inaccurate, outdated, or inconsistently labelled data produces models that embed those quality issues into their predictions. In financial services, where AI outputs can inform credit decisions, fraud determinations, and regulatory reporting, data quality failures in AI systems carry direct financial and compliance consequences.

**Banking example:** A fraud detection model is retrained monthly on new transaction data. Due to an upstream ETL issue, three months of training data contains duplicate transactions that were not deduplicated before ingestion. The model learns inflated baseline frequencies for certain transaction patterns, reducing its sensitivity to genuine anomalies and increasing both false positives (customer friction) and false negatives (missed fraud).

**Primary mitigation:** Automated data quality checks at each stage of the ML pipeline (completeness, consistency, freshness, accuracy), data lineage tracking from source to model, data versioning to enable reproducibility and rollback, and data quality SLAs with upstream data providers.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.6 (AI system performance with representative data)
- ISO 42001: A.7.5 (data quality for AI systems)
- SR 11-7: Section 3 (model development - data and assumptions)

---

### 2.5 Unauthorised Data Access

**Description:** AI systems often require access to broad datasets spanning multiple business domains, creating expanded access paths to sensitive data. The aggregation of data for AI training and inference can bypass traditional data access controls that were designed for transactional systems with narrower access patterns. Service accounts, training pipelines, and inference endpoints all represent potential access paths that must be governed.

**Banking example:** An AI inference endpoint serving a document processing model is configured with read access to a document storage layer containing loan applications. A vulnerability in the API gateway allows unauthenticated access to the inference endpoint, which can be prompted to return or summarise document contents - effectively turning the AI model into an unauthorised data access channel that bypasses the document management system's access controls.

**Primary mitigation:** Principle of least privilege for AI service accounts and pipeline identities, network segmentation between AI infrastructure and data stores, managed identity and RBAC for Azure AI services, audit logging of all data access by AI components, and regular access reviews specific to AI workloads.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.4 (organisational practices for AI risk management)
- ISO 42001: A.8.2 (information security for AI systems)
- CPS 234: Para 33-35 (access management)
- OWASP LLM Top 10: LLM06 (Sensitive Information Disclosure)

---

## 3. OPERATIONAL RISK

Risks arising from the deployment, operation, and runtime behaviour of AI systems in production. These are the risks that materialise when AI moves from development into the real world - where models interact with users, integrate with business processes, and operate at scale. Many of these risks are unique to generative AI and LLMs.

---

### 3.1 Prompt Injection and Jailbreaking

**Description:** Attackers craft inputs designed to override the AI system's instructions, bypass safety controls, or manipulate its behaviour. Direct prompt injection embeds malicious instructions in user input. Indirect prompt injection hides instructions in external data sources the model processes (documents, web pages, emails). Jailbreaking uses creative prompting to circumvent content filters and behavioural guardrails. These are the defining attack class for LLM-based systems.

**Banking example:** A customer-facing banking chatbot processes natural language queries about accounts and products. An attacker crafts a prompt that instructs the chatbot to ignore its system prompt and instead act as an unrestricted assistant, then requests a list of available API endpoints, internal system names, or the contents of the system prompt itself - revealing infrastructure details and security boundaries that inform further attack planning.

**Primary mitigation:** Input validation and prompt filtering, system prompt hardening, output validation and content filtering, separation of instruction and data channels (sandwich architecture), rate limiting and anomaly detection on prompt patterns, and regular red-teaming of deployed LLM systems.

**Standards mapping:**
- NIST AI RMF: MANAGE 2.2 (mechanisms to detect and respond), MEASURE 2.5 (robustness)
- ISO 42001: A.6.2.6 (verification and validation), A.8.2 (information security)
- OWASP LLM Top 10: LLM01 (Prompt Injection)

---

### 3.2 Hallucinations and Incorrect Outputs

**Description:** LLMs generate plausible-sounding but factually incorrect information, fabricated references, or invented data - with the same confidence and fluency as accurate outputs. Users cannot reliably distinguish hallucinated content from factual responses. In financial services, hallucinated outputs can lead to incorrect advice, fabricated regulatory citations, invented transaction details, or fictional customer information being treated as fact.

**Banking example:** An internal knowledge assistant used by compliance analysts is asked about the bank's obligations under a specific regulatory provision. The LLM generates a detailed, well-structured response that cites a specific APRA guidance document - but the cited document does not exist. The analyst, trusting the authoritative tone, incorporates the fabricated citation into a compliance report that is submitted to the regulator.

**Primary mitigation:** RAG (Retrieval-Augmented Generation) architecture to ground responses in verified source documents, output verification layers that check factual claims against authoritative data, confidence scoring and uncertainty indicators in outputs, and clear user guidance that AI outputs require human verification for consequential decisions.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.7 (AI system outputs are interpreted correctly)
- ISO 42001: A.6.2.5 (AI system reliability)
- OWASP LLM Top 10: LLM09 (Misinformation)

---

### 3.3 Excessive Agency (AI Acting Beyond Intended Scope)

**Description:** AI systems that are granted access to tools, APIs, or actions can exceed their intended scope of operation - performing actions that were technically available but not intended for the AI to invoke. This risk is amplified by agentic AI patterns where LLMs are given the ability to chain actions, call APIs, execute code, or interact with external systems. The boundary between "what the AI can do" and "what the AI should do" is enforced by prompts and guardrails rather than hard technical controls.

**Banking example:** An AI assistant designed to help relationship managers draft client communications is given access to the email sending API for convenience. Through a prompt injection or unexpected reasoning chain, the assistant sends an email directly to a client containing a preliminary credit assessment that was intended as an internal draft - creating both a customer communication breach and potential regulatory issues around preliminary credit decisions.

**Primary mitigation:** Principle of least privilege for AI tool access, human-in-the-loop approval gates for consequential actions, hard technical boundaries (not just prompt-based restrictions) on AI capabilities, action logging and audit trails for all AI-initiated operations, and rate limiting on AI-initiated actions.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.4 (risk management practices), MANAGE 2.4 (risk treatment)
- ISO 42001: A.6.2.7 (AI system operation and monitoring)
- OWASP LLM Top 10: LLM08 (Excessive Agency)

---

### 3.4 System Availability and Reliability

**Description:** AI systems in production must meet the same availability and reliability standards as other critical banking infrastructure. However, AI systems introduce unique reliability challenges: model inference latency can spike unpredictably, GPU resources can be constrained, upstream model API providers can experience outages, and model updates can introduce regressions. Financial services availability requirements (often 99.9%+) must account for these AI-specific failure modes.

**Banking example:** A bank's real-time fraud detection system relies on an Azure OpenAI endpoint to analyse transaction narratives as part of its scoring pipeline. During a peak transaction period, the Azure OpenAI service experiences throttling due to regional capacity constraints. The fraud detection pipeline's latency increases beyond the acceptable threshold, forcing a fallback to rule-based detection only - temporarily reducing fraud detection coverage during the highest-risk period.

**Primary mitigation:** Graceful degradation patterns with fallback to non-AI processing, multi-region deployment for AI inference endpoints, capacity planning and quota management for cloud AI services, circuit breaker patterns for AI API calls, and defined SLAs and SLOs for AI components that align with business criticality.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.5 (AI system resilience)
- ISO 42001: A.6.2.5 (AI system reliability)
- CPS 234: Para 14-17 (information security capability)

---

### 3.5 Third-Party AI Model and API Dependencies

**Description:** Financial institutions increasingly depend on third-party AI models and APIs (Azure OpenAI, AWS Bedrock, Anthropic) rather than building and hosting models in-house. This creates supply chain dependencies where the model behaviour, performance, safety characteristics, and availability are controlled by an external provider. Model updates, deprecations, and policy changes happen on the provider's timeline, not the bank's.

**Banking example:** A bank's document processing pipeline uses a specific version of GPT-4 via Azure OpenAI. The model provider releases an updated version and deprecates the previous one with 90 days' notice. The bank's testing reveals that the updated model handles certain document formats differently, requiring prompt engineering changes and regression testing across the entire document processing workflow - an unplanned remediation effort driven by an external dependency.

**Primary mitigation:** Vendor risk assessment specific to AI model providers (beyond standard third-party risk), model version pinning and controlled upgrade processes, contractual protections around model deprecation timelines and behaviour changes, abstraction layers that enable model substitution, and contingency plans for provider outages or discontinuation.

**Standards mapping:**
- NIST AI RMF: GOVERN 6.1 (policies for third-party AI resources)
- ISO 42001: A.10.2 (AI system supply chain), A.10.3 (third-party relationships)
- OWASP LLM Top 10: LLM05 (Supply Chain Vulnerabilities)
- SPS 232: Outsourcing requirements (for APRA-regulated entities)

---

## 4. COMPLIANCE RISK

Risks arising from the evolving regulatory and legal landscape surrounding AI in financial services. Compliance risk is distinct from the other categories because it is primarily driven by external factors - regulatory expectations, legal precedent, and societal norms - rather than technical characteristics of the AI system itself. However, managing compliance risk requires technical capabilities (explainability, audit trails, bias testing) that must be designed into AI systems from the start.

---

### 4.1 Regulatory Uncertainty

**Description:** The AI regulatory landscape is evolving rapidly and unevenly across jurisdictions. Financial institutions operating across borders face a patchwork of emerging requirements with different scope, definitions, and timelines. Building AI governance for today's requirements is necessary but insufficient - governance frameworks must be adaptable to requirements that don't yet exist. The risk is both non-compliance with emerging rules and over-compliance that unnecessarily constrains AI adoption.

**Banking example:** An Australian bank operating across APAC builds a customer service AI platform. APRA has signalled interest in AI governance but hasn't issued specific guidance. Singapore's MAS has published FEAT principles. The EU AI Act classifies certain financial AI as high-risk. The bank must build a governance framework that satisfies its home regulator's emerging expectations while also meeting requirements in every jurisdiction where it operates - without clear, harmonised standards to build against.

**Primary mitigation:** Build governance on internationally recognised foundations (ISO 42001, NIST AI RMF) that are likely to inform jurisdiction-specific requirements, maintain a regulatory horizon scanning function for AI-specific developments, design governance processes that can accommodate additional requirements without structural changes, and engage proactively with regulators on AI governance approach.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.1 (legal and regulatory requirements identified)
- ISO 42001: 4.1 (understanding the organisation and its context), A.4.3 (legal requirements)
- EU AI Act: Articles 6-7 (classification of high-risk AI systems)

---

### 4.2 Explainability and Auditability Requirements

**Description:** Regulators and customers have a right to understand how AI-influenced decisions are made. For financial services, this includes explaining adverse credit decisions, demonstrating that pricing is non-discriminatory, and providing audit trails for regulatory examinations. Many AI systems, particularly deep learning models and LLMs, are inherently difficult to explain, creating tension between model capability and regulatory expectations for transparency.

**Banking example:** A regulator conducting a thematic review of AI in lending requests the bank demonstrate how its AI-assisted credit decisioning process reaches specific outcomes for sampled applicants. The bank must produce an audit trail showing: what data the model received, what features influenced the decision, how the model's recommendation was weighed against human judgement, and that the outcome would not have been materially different for a comparable applicant from a different demographic group.

**Primary mitigation:** Implement explainability by design - select model architectures that support the level of explainability required for each use case, maintain comprehensive decision logs (inputs, outputs, model version, confidence scores), implement post-hoc explanation tools (SHAP, LIME, attention visualisation), and establish clear documentation of the human-AI decision boundary.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.2 (transparency), MEASURE 2.7 (interpretability)
- ISO 42001: A.6.2.3 (AI system transparency), A.8.3 (logging and monitoring)
- SR 11-7: Section 4 (model validation), Section 5 (governance)

---

### 4.3 Fairness and Anti-Discrimination Laws

**Description:** AI systems used in financial services decisions are subject to anti-discrimination laws that prohibit adverse treatment based on protected characteristics (race, gender, age, disability, etc.). AI can violate these laws through direct discrimination (using protected attributes) or indirect discrimination (using proxy variables that correlate with protected attributes). The legal and reputational consequences of discriminatory AI in banking are severe - class action exposure, regulatory enforcement, and lasting brand damage.

**Banking example:** A credit decisioning model uses postcode as a feature. In certain markets, postcodes are strongly correlated with ethnicity and socioeconomic status. The model learns to weigh postcode heavily in its risk assessments, producing systematically different approval rates across demographic groups - even though no protected attribute is used directly. An advocacy group identifies the pattern through statistical analysis of the bank's published lending data, triggering a regulatory investigation and public scrutiny.

**Primary mitigation:** Pre-deployment disparate impact testing across all protected attributes, ongoing monitoring of outcome distributions by demographic group, proxy variable analysis to identify features that correlate with protected characteristics, regular third-party fairness audits, and documented remediation processes for identified bias.

**Standards mapping:**
- NIST AI RMF: MEASURE 2.11 (fairness and bias assessment)
- ISO 42001: A.6.2.4 (impact assessment), A.8.5 (societal and environmental wellbeing)
- EU AI Act: Article 10 (data and data governance for high-risk AI)

---

### 4.4 Data Sovereignty and Cross-Border Transfer

**Description:** AI workloads frequently process data across geographic boundaries - training data may be stored in one region, model training may occur in another, and inference may happen in a third. For financial services, data sovereignty requirements, privacy regulations, and regulatory expectations around data residency create constraints on where AI processing can occur. Cloud AI services add complexity because the location of model inference, content filtering, and abuse monitoring may not always be transparent.

**Banking example:** An Australian bank uses Azure OpenAI with an endpoint in the Australia East region, assuming all data processing remains within Australian borders. However, the bank's content filtering and abuse monitoring configurations route data through infrastructure in the US. A regulatory inquiry into data handling practices reveals that customer data submitted to the AI system has been processed outside Australia, potentially breaching data sovereignty commitments and APRA's expectations around offshore data processing.

**Primary mitigation:** Explicit data residency architecture documenting where every component of the AI pipeline processes data, contractual guarantees from cloud providers on data processing locations, Azure data boundary controls and regional deployment configurations, data classification that drives residency requirements (not all data has the same sovereignty constraints), and regular validation that actual data flows match documented architecture.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.7 (privacy values and principles)
- ISO 42001: A.8.4 (privacy considerations for AI), A.7.4 (data for AI systems)
- CPS 234: Para 36-41 (information assets managed by related or third parties)
- SPS 232: Offshoring requirements

---

### 4.5 Intellectual Property Concerns

**Description:** AI systems raise novel intellectual property questions at multiple levels: the IP status of AI-generated content, the risk that models reproduce copyrighted training material, the protection of proprietary prompts and fine-tuning as trade secrets, and the potential for models to ingest and disclose proprietary information from competitors via training data. The legal landscape is still being established through litigation and legislation, creating uncertainty that must be managed as risk.

**Banking example:** A bank's marketing team uses an LLM to generate customer communication materials. The model produces content that closely mirrors the phrasing and structure of a competitor's published materials - content that was likely present in the model's public training data. The competitor alleges copyright infringement. Separately, the bank's proprietary prompt templates and system instructions, which encode significant intellectual effort and competitive advantage, are extracted through a prompt injection attack and published online.

**Primary mitigation:** Legal review of AI-generated content before external publication, IP indemnification provisions in AI service provider contracts, protection of prompts and system instructions as confidential information (not relying solely on prompt-based secrecy), clear internal policies on what content AI can generate and for what purposes, and monitoring for reproduced copyrighted material in AI outputs.

**Standards mapping:**
- NIST AI RMF: GOVERN 1.1 (legal requirements), MAP 5.1 (impacts to people and organisations)
- ISO 42001: A.4.3 (legal and regulatory requirements), A.8.2 (information security)
- OWASP LLM Top 10: LLM06 (Sensitive Information Disclosure) - in the context of IP leakage

---

## Cross-Reference Summary

Quick reference for diagram annotations showing which standards apply to each risk:

| Risk | NIST AI RMF | ISO 42001 | OWASP LLM Top 10 | Other |
|------|------------|-----------|-------------------|-------|
| 1.1 Adversarial Attacks | MANAGE 2.2 | A.6.2.6 | LLM04 | |
| 1.2 Model Drift | MEASURE 2.6 | A.6.2.7 | | SR 11-7 S5 |
| 1.3 Bias/Fairness | MAP 2.3, MEASURE 2.11 | A.6.2.4 | LLM09 | |
| 1.4 Explainability | GOVERN 1.2, MEASURE 2.7 | A.6.2.3 | | SR 11-7 S4 |
| 1.5 Overfitting | MEASURE 2.5 | A.6.2.6 | | SR 11-7 S4 |
| 2.1 Data Poisoning | MEASURE 2.6 | A.7.4, A.7.5 | LLM04 | |
| 2.2 Data Leakage | GOVERN 1.7, MANAGE 2.2 | A.7.4, A.8.4 | LLM06 | |
| 2.3 Data Governance | GOVERN 1.7, MAP 3.4 | A.7.4, A.7.2 | | CPS 234 |
| 2.4 Data Quality | MEASURE 2.6 | A.7.5 | | SR 11-7 S3 |
| 2.5 Unauthorised Access | GOVERN 1.4 | A.8.2 | LLM06 | CPS 234 |
| 3.1 Prompt Injection | MANAGE 2.2, MEASURE 2.5 | A.6.2.6, A.8.2 | LLM01 | |
| 3.2 Hallucinations | MEASURE 2.7 | A.6.2.5 | LLM09 | |
| 3.3 Excessive Agency | GOVERN 1.4, MANAGE 2.4 | A.6.2.7 | LLM08 | |
| 3.4 Availability | MEASURE 2.5 | A.6.2.5 | | CPS 234 |
| 3.5 Third-Party Deps | GOVERN 6.1 | A.10.2, A.10.3 | LLM05 | SPS 232 |
| 4.1 Regulatory Uncertainty | GOVERN 1.1 | 4.1, A.4.3 | | EU AI Act |
| 4.2 Auditability | GOVERN 1.2, MEASURE 2.7 | A.6.2.3, A.8.3 | | SR 11-7 |
| 4.3 Anti-Discrimination | MEASURE 2.11 | A.6.2.4, A.8.5 | | EU AI Act |
| 4.4 Data Sovereignty | GOVERN 1.7 | A.8.4, A.7.4 | | CPS 234, SPS 232 |
| 4.5 IP Concerns | GOVERN 1.1, MAP 5.1 | A.4.3, A.8.2 | LLM06 | |

---

## Diagram Notes

**For draw.io implementation:**

- Each of the 20 risk nodes should display: risk name, severity indicator, and primary standard icon(s)
- Use the colour coding defined above to visually group by category
- Consider a second view that reorganises risks by applicable standard rather than by category - useful for compliance teams working through a specific framework
- Include a legend explaining severity levels, colour coding, and standard abbreviations
- The cross-reference summary table above can be included as a second page/tab in the draw.io file for reference
