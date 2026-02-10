# AI Governance & Security Architecture for Financial Services

A practical, opinionated framework for securing AI/ML workloads in regulated financial services environments, with a focus on Azure AI services, large language models, and the emerging standards that govern them.

---

## What This Framework Is (and Isn't)

**This is** a structured collection of security architecture artifacts - threat models, control mappings, architecture decision records, and reference patterns - built by a security architect who is actively learning AI security and applying two decades of enterprise security experience to this new domain.

**This is not** a finished product, an official Microsoft position, or a claim of AI expertise. It's a working repository where I think through hard problems in public, make mistakes, correct them, and hopefully produce something useful for others navigating the same territory.

Think of it as an architect's notebook, not a textbook.

## Why I'm Building This

I've spent 20 years in security architecture. I've seen what happens when organizations adopt transformative technology without security architecture that keeps pace - cloud taught us that lesson, and AI is teaching it again, faster.

Financial services CISOs are fielding board-level questions about AI risk today, and the industry's answers are still fragmented across overlapping frameworks (ISO 42001, NIST AI RMF, EU AI Act) with limited practical guidance for Azure-centric environments.

I'm building this framework to:

- **Learn systematically** - Writing forces clarity. Publishing forces rigor.
- **Bridge the gap** between abstract AI governance standards and concrete Azure implementation patterns.
- **Demonstrate architectural thinking** - Not just *what* to secure, but *why* these trade-offs, *why* this control placement, *why* this architecture over the alternatives.
- **Contribute to the community** - The more security architects share their thinking on AI governance, the faster we all get better at it.

## What's Inside

The framework is organized into six areas that mirror how I think about AI security architecture in financial services:

### 1. Executive Summary
A board-ready overview of AI risk in financial services, why existing security frameworks are necessary but insufficient, and what an AI governance program needs to address.

### 2. AI Risk Taxonomy
A structured classification of AI-specific risks - model risks, data risks, operational risks, ethical risks - mapped to financial services regulatory expectations. Built on NIST AI RMF categories but extended for banking-specific concerns like fair lending and market manipulation.

### 3. Azure AI Security Architecture
Reference architectures for securing Azure OpenAI Service, Azure AI Studio, and supporting infrastructure. Covers network isolation, identity and access, data protection, and logging patterns. This is where my day-to-day Azure security experience meets AI-specific requirements.

### 4. Governance Operating Model
The people and process side: roles, responsibilities, review gates, and escalation paths for AI workloads. Covers the AI model lifecycle from experimentation through production, including who approves what and when.

### 5. Control Mapping
Cross-referencing exercise mapping controls across ISO 42001, NIST AI RMF, OWASP LLM Top 10, and existing financial services frameworks (FFIEC, SR 11-7). The goal is to show where existing controls already address AI risks and where net-new controls are needed.

### 6. Banking Use Cases & Threat Models
Concrete threat models for common financial services AI use cases: customer-facing chatbots, document processing, fraud detection, credit decisioning. Each use case includes STRIDE-based threat analysis, attack trees, and recommended mitigations.

## Architecture Decision Records (ADRs)

Key architectural decisions are documented as ADRs, capturing the context, options considered, and rationale. These are the "why" behind the framework:

| ADR | Decision |
|-----|----------|
| [ADR-001](adrs/adr-001-azure-openai-vs-self-hosted.md) | Azure OpenAI vs. self-hosted models |
| [ADR-002](adrs/adr-002-guardrail-architecture.md) | Guardrail architecture patterns |
| [ADR-003](adrs/adr-003-training-data-governance.md) | Training data governance approach |
| [ADR-004](adrs/adr-004-model-monitoring-strategy.md) | Model monitoring strategy |
| [ADR-005](adrs/adr-005-prompt-injection-mitigation.md) | Prompt injection mitigation |
| [ADR-006](adrs/adr-006-rag-security-pattern.md) | RAG security patterns |

## Status

This is an active work in progress. Here's where things stand:

| Document | Status | Notes |
|----------|--------|-------|
| [Executive Summary](docs/01-executive-summary.md) | **Complete** | |
| [AI Risk Taxonomy](docs/02-ai-risk-taxonomy.md) | **Complete** | |
| [Azure AI Security Architecture](docs/03-azure-ai-security-architecture.md) | Planned | Priority - starting here |
| [Governance Operating Model](docs/04-governance-operating-model.md) | Planned | |
| [Control Mapping](docs/05-control-mapping.md) | Planned | Dependent on risk taxonomy |
| [Banking Use Cases & Threat Models](docs/06-banking-use-cases.md) | Planned | |
| [Threat Models](docs/07-threat-models.md) | Planned | STRIDE-based approach |
| ADR-001: Azure OpenAI vs. Self-Hosted | Planned | |
| ADR-002: Guardrail Architecture | Planned | |
| ADR-003: Training Data Governance | Planned | |
| ADR-004: Model Monitoring | Planned | |
| ADR-005: Prompt Injection Mitigation | Planned | |
| ADR-006: RAG Security Pattern | Planned | |

## Quick Start

- **Security architects** - Start with the [Azure AI Security Architecture](docs/03-azure-ai-security-architecture.md) for reference patterns, then review the [ADRs](adrs/) for decision rationale.
- **CISOs and risk leaders** - Start with the [Executive Summary](docs/01-executive-summary.md) and [AI Risk Taxonomy](docs/02-ai-risk-taxonomy.md) for the risk landscape.
- **Governance teams** - Start with the [Control Mapping](docs/05-control-mapping.md) to understand how AI controls map to your existing framework.
- **Everyone** - The [Banking Use Cases](docs/06-banking-use-cases.md) ground everything in concrete scenarios.

## Standards and Frameworks Referenced

- **ISO/IEC 42001:2023** - AI Management System
- **NIST AI Risk Management Framework (AI RMF 1.0)**
- **OWASP LLM Top 10 (2025)**
- **FFIEC Guidance on AI/ML** - Federal Financial Institutions Examination Council
- **SR 11-7** - Federal Reserve Guidance on Model Risk Management
- **EU AI Act** - Where relevant to global financial institutions

## About the Author

I'm a security architect with 20 years of experience across enterprise security, cloud architecture, and infrastructure. CISSP certified. Currently at Microsoft working on Azure security architecture.

AI security is my newest domain, and I'm approaching it the way I approach any new architecture challenge: by reading the standards, studying the threat landscape, building reference architectures, and pressure-testing my thinking by publishing it. I don't have all the answers here - some of what I write will be wrong, and I'll correct it as I learn. That's the point.

If you're a security architect navigating the same transition, I hope this is useful. If you spot something I've gotten wrong, I'd genuinely appreciate the feedback.

## Disclaimer

This repository is a **personal learning project** and does not constitute professional advice, consulting guidance, or an official position of any employer, past or present.

- **Not professional advice.** The content in this framework is provided for educational and informational purposes only. It should not be relied upon as legal, regulatory, compliance, or security advice. Financial institutions should engage qualified professionals for AI governance, risk management, and regulatory compliance decisions.
- **Not an employer position.** All views, opinions, and architectural recommendations expressed here are my own. They do not represent the views, policies, or positions of Microsoft or any other organisation I am or have been affiliated with.
- **No warranty.** This framework is provided "as is" without warranty of any kind. I make no guarantees about the accuracy, completeness, or suitability of any content for any particular purpose. AI security is a rapidly evolving domain and information may become outdated.
- **No assessment of third parties.** Nothing in this framework should be interpreted as an assessment, criticism, or endorsement of the security posture, AI practices, or risk management capabilities of any specific financial institution, technology vendor, or other organisation.
- **Use at your own risk.** Any implementation decisions based on patterns or recommendations in this framework are the sole responsibility of the implementing organisation. Always validate against your own regulatory obligations, risk appetite, and organisational context.

## Contributing

This is primarily a personal learning project, but feedback and contributions are welcome:

- **Open an issue** if you spot an error or disagree with an architectural decision
- **Start a discussion** if you want to explore a topic further
- **Submit a PR** if you'd like to contribute content

## License

This work is shared for educational purposes. See [LICENSE](LICENSE) for details.
