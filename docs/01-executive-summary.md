# Executive Summary: AI Governance & Security Architecture for Financial Services

> **Author's note:** I'm a security architect with 20 years of experience, currently working in Azure security architecture at Microsoft. AI security is my newest domain. I built this framework to learn systematically and to demonstrate how I approach complex architectural problems. This document reflects genuine research and architectural thinking, but I want to be upfront: I'm applying proven security architecture methods to a domain I'm actively learning. Where I'm confident, I'll say so. Where I'm still forming my views, I'll say that too.

---

## 1. The Challenge: AI Adoption Is Outpacing AI Governance

Financial institutions are deploying AI at a pace that their governance frameworks were never designed to handle.

Australian major banks are rolling out generative AI across customer service, fraud detection, and internal productivity. Globally, the pattern is the same: the largest financial institutions have moved beyond pilots into production LLM deployments - internal knowledge assistants, customer-facing advisory tools, AI-driven coding platforms, and autonomous transaction monitoring systems. The financial services sector is not experimenting with AI. It is operationalising it.

This isn't experimentation any more. These are production systems handling regulated data, making or influencing decisions that affect customers, and operating at a scale where failures carry material consequences.

**The problem is that AI introduces categories of risk that traditional security and risk frameworks don't adequately address:**

- **Prompt injection and manipulation.** Attackers can manipulate LLM behaviour through crafted inputs, potentially bypassing access controls, exfiltrating data, or causing the model to take unauthorised actions. This is fundamentally different from traditional input validation - the attack surface is the natural language interface itself.

- **Hallucination and confabulation.** Models generate plausible but fabricated information with high confidence. In financial services, this means incorrect regulatory guidance, fabricated transaction histories, or invented compliance requirements - delivered with the authority of an institutional system.

- **Training data poisoning and leakage.** Models can memorise and regurgitate sensitive training data, including customer PII, proprietary trading strategies, or confidential deal information. Data poisoning attacks can subtly shift model behaviour in ways that are difficult to detect through traditional testing.

- **Algorithmic bias and fairness failures.** AI systems used in credit decisioning, fraud detection, or customer segmentation can encode and amplify biases, creating fair lending violations, discriminatory outcomes, and regulatory exposure that may not surface until significant harm has occurred.

- **Model supply chain risks.** Financial institutions are consuming models from third-party providers (OpenAI, Anthropic, open-source), creating dependencies on external model behaviour, training data provenance, and update cycles that don't align with traditional vendor risk management.

**The regulatory landscape is still catching up.** APRA is actively monitoring AI adoption in Australian financial services but has not yet issued AI-specific prudential guidance. The existing frameworks - CPS 234 for information security, CPS 220 for risk management, SPS 232 for outsourcing - provide a foundation, but they were written before generative AI entered the picture. Globally, the EU AI Act is creating compliance obligations for high-risk AI systems, and the US regulatory agencies (OCC, Fed, FDIC) have issued joint guidance on AI/ML in financial services, but comprehensive prudential standards are still in development.

Banks that wait for prescriptive regulation before building AI governance will find themselves retrofitting controls onto production systems - a pattern that cloud migration taught us is far more expensive and risky than building governance in from the start.

## 2. The Solution: A Practical AI Security Architecture Framework

This framework takes the emerging AI governance standards - ISO/IEC 42001, NIST AI Risk Management Framework, OWASP LLM Top 10 - and translates them into practical security architecture patterns for financial services, with a focus on Azure AI services.

**What makes this approach different from reading the standards directly:**

- **Implementation specificity.** ISO 42001 tells you to "manage AI risks." This framework shows you *how* - with Azure resource configurations, network architectures, identity patterns, and monitoring strategies.

- **Financial services context.** Generic AI governance frameworks don't account for the specific regulatory, operational, and reputational constraints of banking. This framework maps controls to FFIEC guidance, SR 11-7 model risk management expectations, and APRA prudential standards.

- **Use case grounding.** Rather than abstract control descriptions, this framework builds security architecture around four concrete banking AI use cases: customer-facing chatbots, document processing and extraction, fraud detection systems, and credit decisioning support.

- **Governance beyond technology.** Technical controls are necessary but insufficient. This framework includes a governance operating model - the roles, review gates, approval workflows, and escalation paths that make AI security operational.

The framework is designed for Azure-centric environments because that reflects both my professional experience and the reality that many financial institutions have standardised on Azure (or are evaluating it) for AI workloads. However, the architectural principles, risk taxonomy, and governance patterns apply regardless of the cloud platform.

## 3. Framework Overview

The framework is organised into six components, each addressing a distinct aspect of AI governance and security:

| Component | Purpose | Key Outputs |
|-----------|---------|-------------|
| **AI Risk Taxonomy** | Structured classification of AI-specific risks in financial services | Risk register, risk categories, regulatory mapping |
| **Azure AI Security Architecture** | Reference architectures for secure AI deployment on Azure | Network diagrams, identity patterns, data flow models |
| **Governance Operating Model** | People and process framework for AI lifecycle management | RACI matrices, approval workflows, review gate criteria |
| **Control Mapping** | Cross-reference of controls across multiple standards | Control matrix (ISO 42001, NIST AI RMF, OWASP, FFIEC) |
| **Banking Use Cases** | Concrete AI scenarios with security requirements | Use case profiles, data flow diagrams, security requirements |
| **Threat Models** | STRIDE-based threat analysis for each banking use case | Threat catalogues, attack trees, mitigation strategies |

**Supporting these six components are Architecture Decision Records (ADRs)** that document the key design decisions, the alternatives considered, and the rationale for each choice. These ADRs are where much of the architectural thinking lives - they capture not just *what* was decided, but *why*, which is often more valuable than the decision itself.

**Design principles guiding the framework:**

- **Defence in depth for AI.** No single control addresses AI risk. The framework layers controls across the model lifecycle: data ingestion, training/fine-tuning, deployment, runtime, and monitoring.

- **Incremental adoption.** Not every organisation needs every component on day one. The framework is designed so that teams can adopt the risk taxonomy first, then layer on technical controls, then build out governance processes.

- **Existing control reuse.** Many AI risks are variants of risks that financial institutions already manage. The control mapping explicitly identifies where existing security, data governance, and model risk controls can be extended rather than replaced.

- **Assume breach for AI systems.** Traditional security architecture assumes that perimeter controls may fail. AI security architecture must additionally assume that the model itself may behave unexpectedly - through adversarial manipulation, data quality issues, or inherent model limitations.

## 4. Target Outcomes

This framework is designed to enable four outcomes for financial institutions adopting AI:

**Risk visibility for leadership.** CISOs and boards need to understand AI risk in terms they can act on. The risk taxonomy translates AI-specific technical risks (prompt injection, embedding inversion, model extraction) into business risk categories (financial loss, regulatory action, reputational damage, customer harm) with clear severity and likelihood assessments.

**Secure AI deployment patterns.** Security architects need reusable patterns they can apply to new AI initiatives without starting from scratch each time. The Azure reference architectures and use case threat models provide that foundation - not as rigid templates, but as starting points that encode security decisions and can be adapted to specific contexts.

**Regulatory readiness.** When APRA (or other prudential regulators) issue AI-specific guidance, institutions that have already mapped their AI controls to ISO 42001 and NIST AI RMF will be positioned to demonstrate compliance quickly. The control mapping component is designed specifically for this purpose - it creates a traceable line from regulatory expectations to implemented controls.

**Balanced innovation and risk management.** The goal is not to prevent AI adoption - that ship has sailed, and the competitive implications of falling behind are real. The goal is to enable AI adoption at a pace that the organisation's risk management capability can support. The governance operating model provides the mechanism for this: clear criteria for what AI use cases require what level of review, so that low-risk applications move quickly while high-risk applications get appropriate scrutiny.

## 5. Who Should Use This Framework

**Security architects designing AI systems for financial institutions.** This is the primary audience. If you're being asked to secure an Azure OpenAI deployment, design guardrails for a customer-facing chatbot, or architect a RAG system that handles regulated data, this framework provides reference patterns and threat models to accelerate your work.

**CISOs and security leaders evaluating AI risk.** The executive summary and risk taxonomy are designed to be board-ready. They translate technical AI risks into business language and provide a structured way to assess and communicate AI risk posture.

**Risk and compliance teams.** The control mapping component is specifically designed for teams that need to assess AI initiatives against regulatory expectations. It maps controls across ISO 42001, NIST AI RMF, OWASP LLM Top 10, and financial services frameworks, making it easier to identify gaps and demonstrate coverage.

**AI and ML engineering teams.** Engineers building AI systems in financial services often lack security architecture guidance specific to their domain. The use case profiles and threat models provide concrete security requirements that can be incorporated into design and development processes.

**Security architects learning AI security.** This is where I am. If you're an experienced security professional applying your skills to AI for the first time, this framework represents one approach to structuring that learning. Take what's useful, challenge what doesn't look right, and share what you learn.

## 6. What This Framework Is and Is Not

It's important to set expectations clearly.

**This framework IS:**

- Rigorous architectural thinking applied to AI security. I'm applying methods that have worked across two decades of enterprise security architecture - threat modelling, control mapping, defence in depth, architecture decision records - to AI-specific risks.

- Practical Azure implementation guidance. Not just conceptual frameworks, but specific patterns for Azure OpenAI Service, Azure AI Studio, Azure API Management, and supporting infrastructure.

- A learning exercise that demonstrates my approach. This is "learning in public" in its most literal form. I'm researching, synthesising, architecting, and publishing - and the quality of the output is the evidence of the quality of the thinking.

- Grounded in established standards. Every recommendation traces back to ISO 42001, NIST AI RMF, OWASP LLM Top 10, or established financial services regulatory expectations. I'm not inventing a new framework - I'm making existing frameworks actionable.

**This framework IS NOT:**

- Expert guidance from a seasoned AI security specialist. I have deep security architecture experience, but AI security is a domain I'm actively learning. I've done extensive research, but I haven't yet accumulated years of production AI security experience. I believe the architectural thinking is sound, but I want readers to evaluate it critically rather than accept it on authority.

- A replacement for qualified AI risk consultants. For institutions making significant AI investments, this framework is a useful starting point and reference, but it should complement - not replace - engagement with professionals who have deep domain expertise in AI risk management.

- Purely theoretical. Despite being a learning exercise, this framework includes concrete implementation patterns, real Azure configurations, and actionable threat models. The goal is to be useful, not just educational.

- An official Microsoft position or product. This is a personal project. It reflects my own research and architectural thinking, not the views of my employer.

## 7. Where to Start

Your entry point depends on your role and what you need most immediately:

| If you are... | Start with... | Then move to... |
|---------------|---------------|-----------------|
| A CISO or board member | This executive summary, then the [AI Risk Taxonomy](02-ai-risk-taxonomy.md) | [Governance Operating Model](04-governance-operating-model.md) |
| A security architect | [Azure AI Security Architecture](03-azure-ai-security-architecture.md) | [Threat Models](07-threat-models.md) and [ADRs](../adrs/) |
| A risk/compliance professional | [Control Mapping](05-control-mapping.md) | [AI Risk Taxonomy](02-ai-risk-taxonomy.md) |
| An AI/ML engineer | [Banking Use Cases](06-banking-use-cases.md) | [Azure AI Security Architecture](03-azure-ai-security-architecture.md) |
| A security architect learning AI | Read everything in order | The sequence is designed to build understanding progressively |

---

**Next document:** [AI Risk Taxonomy for Financial Services](02-ai-risk-taxonomy.md) - A structured classification of AI-specific risks mapped to banking regulatory expectations.
