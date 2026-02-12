# Securing RAG Systems

**AI Security Framework — Reference Architecture**
**Author:** Daniel | **Version:** 1.0 | **Last Updated:** February 2026

> **Standards Alignment:** OWASP Top 10 for LLM Applications 2025 (LLM01, LLM02, LLM08) · SANS Critical AI Security Guidelines v1.1 · NIST AI RMF · ISO 42001
>
> **Key Research:** BlackHat USA 2024 — Bargury & Sharbat, "Living off Microsoft Copilot" (RAG poisoning, indirect prompt injection) · Liu et al., "Prompt Injection Attack against LLM-integrated Applications" (2023) · Zeng et al., "Privacy Issues in RAG" (ACL 2024)

---

## 1. What is RAG?

### The One-Paragraph Explanation

Retrieval-Augmented Generation (RAG) is a method where an AI system searches your company's documents before it answers a question. Instead of relying only on what the AI model learned during training, it pulls in real information from your files, databases, or knowledge bases. This makes the answers more accurate and more relevant to your business. RAG is how most companies connect AI tools to their own data.

### Why Companies Use RAG

RAG solves a simple problem: AI models do not know your company's information. They were trained on public data from the internet, not your internal policies, customer records, or product documentation.

With RAG, companies can build:

- **Customer support chatbots** that answer questions using real product documentation, not guesses.
- **Employee knowledge bases** where staff ask questions and get answers drawn from internal policies, HR handbooks, or technical guides.
- **Document search tools** that find and summarise information buried in thousands of files — contracts, reports, compliance records.
- **Code assistants** that understand your company's codebase and internal standards.

RAG is popular because it is faster and cheaper than retraining an AI model. You point the system at your documents, and it starts working within days, not months.

### Why Security Matters

RAG creates a new type of risk that traditional security tools were not designed to handle. Here is the core problem:

**RAG connects your most sensitive internal data directly to an AI model that was designed to be helpful — not cautious.**

Think of it this way. A traditional database has strict rules. You write a query, and it returns exactly what you asked for. An AI model is different. It interprets, summarises, and sometimes makes assumptions. When you connect your sensitive documents to a system that interprets loosely, you create risks that did not exist before.

At BlackHat USA 2024, security researcher Michael Bargury demonstrated this problem clearly. He showed how an attacker could send a single email to a company using Microsoft Copilot — which uses RAG to search internal documents and emails — and manipulate the AI into changing bank account numbers, stealing credentials, and directing users to phishing sites. No malware was needed. The attack worked because the RAG system treated the malicious email as trusted information, just like any other document in the knowledge base.

Traditional application security focuses on input validation, authentication, and access control. These still matter for RAG systems. But RAG adds three new challenges:

1. **Documents become attack vectors.** A poisoned document in your knowledge base can change every answer the AI gives about that topic. This is not a theoretical risk — it was demonstrated at BlackHat 2024.

2. **Access control gets harder.** When an AI system searches across thousands of documents to build an answer, how do you ensure the user is authorised to see every piece of information the AI pulls together? A single answer might combine fragments from documents with different classification levels.

3. **Outputs are unpredictable.** Unlike a database query that returns the same result every time, RAG answers vary. The same question asked twice might pull different documents and produce different responses. This makes testing and auditing much harder.

The OWASP Top 10 for LLM Applications 2025 includes **LLM08: Vector and Embedding Weaknesses** as a dedicated entry, recognising that RAG systems represent a distinct and growing attack surface. The SANS Critical AI Security Guidelines v1.1 similarly identifies data protection for augmentation systems — including vector databases — as a critical control category.

This document provides the security architecture for addressing these risks.

---

## 2. RAG Security Principles

Good RAG security starts with clear thinking, not specific products. The principles below are drawn from the OWASP GenAI Security Project, the SANS Critical AI Security Guidelines v1.1, and practical lessons from published security research. They apply regardless of which cloud provider, AI model, or vector database you use.

### Traditional Security Still Applies

RAG systems are software systems first. Every security control you already use still matters. Do not abandon what works.

**Identify trust boundaries.** Map where data moves from trusted to untrusted zones in your RAG pipeline. The most important boundary is between your curated knowledge base and the AI model. Documents in your knowledge base are *application inputs* — treat them with the same scrutiny you would apply to any external data source entering your system. The SANS Guidelines emphasise that augmentation data "should be treated as sensitive, especially if it influences LLM responses."

For example, a customer support RAG system has at least four trust boundaries: the user's question enters the system (untrusted), the retrieval engine searches the vector database (semi-trusted — the data was curated but could be poisoned), the retrieved documents are assembled into a prompt (critical boundary — this is where injection attacks happen), and the model's response goes back to the user (untrusted output — must be validated before display).

**Trace data flows end to end.** Document how data enters your RAG system, how it is chunked and embedded, where embeddings are stored, how retrieval works, how prompts are assembled, and how responses are delivered. You cannot secure what you cannot map. Most RAG vulnerabilities exploit gaps between these stages — the document parser, the text splitter, and the prompt template are all exploitable components, as demonstrated in retrieval poisoning research (Liu et al., FSE 2024).

**Apply least privilege.** AI agents and retrieval tools should have the minimum permissions needed to function. If your RAG system only needs to read from a knowledge base, it should not have write access. If it only serves one department, it should not have access to company-wide data. The BlackHat 2024 Copilot research showed how overly broad permissions let attackers use the AI to access data across the entire Microsoft 365 environment — emails, files, Teams messages — far beyond what any single query required.

### New AI-Specific Risks

RAG systems introduce risks that do not exist in traditional applications. Your security architecture must account for these explicitly.

**LLMs are unreliable components. Design around failure.** An AI model will sometimes produce wrong answers, follow injected instructions, or combine information in unexpected ways. Your architecture must assume the model will fail and include controls that limit the damage when it does. Never make the AI model your only line of defence for anything — not for access control, not for content filtering, not for data classification. The SANS Guidelines state this directly: "if there is information or actions that your AI should never disclose or take, the wiser course is to ensure your model does not have access to that information."

For example, do not rely on a system prompt instruction like "never reveal salary data" to protect sensitive information. Instead, remove salary data from the knowledge base that the model can access.

**Treat LLM output as untrusted.** Every response from an AI model should be treated the same way you treat user input in a web application — as potentially malicious. The model's output could contain injected commands, cross-site scripting payloads, or misleading information. Validate and sanitise outputs before they reach downstream systems or users. This aligns with OWASP LLM05: Improper Output Handling.

**External data is an attack surface.** Every document, email, webpage, or tool output that feeds into your RAG system is a potential attack vector. An attacker who can place a document in your knowledge base — or even in a source that gets indexed — can influence every AI response related to that topic. This is exactly how the BlackHat 2024 Copilot attack worked: a single malicious email, never opened by the victim, was indexed by the RAG system and used to poison the AI's responses.

**Isolate secrets from the AI context.** API keys, passwords, database connection strings, and other secrets must never appear in prompts, retrieved documents, or system instructions that the model can access. If a secret is in the model's context window, it can be extracted through prompt injection. The OWASP Top 10 2025 added LLM07: System Prompt Leakage as a new entry specifically because this risk is being exploited in production systems.

### RAG-Specific Principles

Beyond general AI security, RAG systems have their own distinct security requirements.

**Retrieved documents are application inputs — validate them like API inputs.** When your retrieval engine pulls documents from the vector database, those documents become part of the prompt sent to the AI model. They directly influence the response. Apply the same validation you would apply to any external data entering a critical system: check for hidden content (invisible text, embedded instructions), validate document format and structure, and scan for known injection patterns. Research has demonstrated that attackers can embed invisible instructions in documents — such as white text on a white background in a resume — that the RAG system processes but humans cannot see (OWASP LLM08 example scenario).

**Embeddings contain sensitive data — isolate by security domain.** When you convert documents into vector embeddings for your database, those embeddings encode the meaning and content of the original documents. While you cannot read the original text directly from an embedding, research has shown that embedding inversion attacks can reconstruct source text from vectors. Store embeddings for different security classifications in separate vector stores or collections. If your HR policies and your marketing materials have different access requirements, their embeddings should not share a database. The OWASP Top 10 2025 notes that "in a multi-tenant environment where different groups or classes of users share the same vector database, embeddings from one group might be inadvertently retrieved in response to queries from another."

**Default to NOT logging prompts and responses.** Prompts and AI responses frequently contain personally identifiable information (PII), sensitive business data, or other protected information. A user asking "what is John Smith's performance review rating?" creates a log entry that contains PII regardless of whether the system answers the question. Design your logging strategy to capture security-relevant metadata (who asked, when, which documents were retrieved, was the request flagged) without storing the full prompt and response text by default. Where full logging is required for compliance or debugging, apply the same data protection controls you would apply to any PII store — encryption at rest, access controls, and retention policies.

---

*Next section: [3. RAG Threat Model →](./04-securing-rag-systems.md#3-rag-threat-model) (mapping OWASP LLM Top 10 risks to specific RAG components)*

---

> **Framework Navigation**
>
> ← [03 — Identity Security for AI](./03-identity-security-for-ai.md) · **04 — Securing RAG Systems** · [05 — LLM Application Security Patterns →](./05-llm-application-security-patterns.md)
>
> **Standards Cross-Reference:** This document maps to OWASP LLM01 (Prompt Injection), LLM02 (Sensitive Information Disclosure), LLM05 (Improper Output Handling), LLM07 (System Prompt Leakage), LLM08 (Vector and Embedding Weaknesses) · SANS Critical AI Security Guidelines: Access Controls, Data Protection, Inference Security · NIST AI RMF: GOVERN, MAP, MEASURE, MANAGE functions

---

> **Note:** This framework is a portfolio piece demonstrating architectural thinking. It shows *what* good RAG security looks like. For implementation guidance specific to your environment — including vendor-specific configurations, deployment scripts, and testing procedures — refer to the vendor reference architectures linked in Section 5 (Azure AI Services, AWS Bedrock, Google Vertex AI) or [contact the author](https://github.com/[your-github]) for advisory engagements.
