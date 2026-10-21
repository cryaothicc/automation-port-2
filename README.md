# Automated Support Ticket Classifier & RAG Engine

![n8n](https://img.shields.io/badge/n8n-FF6600?style=flat-square&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)

An automated support ticket classification, routing, and auto-response pipeline built using **n8n**, **Google Gemini**, and **Supabase**.

## Core Concepts Learned & Practiced
- **Zero-Shot LLM Classification:** Enforcing strict JSON schemas on Gemini (`gemini-1.5-flash`) to extract `category`, `urgency`, and `summary` metadata from unstructured ticket text.
- **Custom Payload Normalization:** Using an ES6 JavaScript Code node to strip markdown code blocks from LLM responses, handle parse errors with fallbacks, and merge original trigger inputs with AI metadata.
- **Multi-Path Routing & Safeguards:** Routing incoming traffic through a Switch node—halting automated replies for high-risk critical tickets (`Needs Human Review`) while automatically processing standard inquiries.
- **Lightweight RAG (Retrieval-Augmented Generation):** Dynamically querying a Supabase knowledge base table based on ticket category to ground AI email draft generation in domain knowledge.
- **Audit Persistence:** Storing complete execution histories, classification tags, ticket statuses, and generated replies in a Supabase PostgreSQL database.

## Workflow Architecture & Data Flow

```mermaid
graph LR
    A[Form Trigger] --> B{Gemini Classifier}
    B --> C[JS Code Node]
    C --> D{Switch Router}
    D -- Critical Path --> E[(Supabase: Escalated)]
    D -- Standard Path --> F[(Supabase: KB Query)]
    F --> G{Gemini Drafter}
    G --> H[(Supabase: Auto-Replied)]
    
    style A fill:#FF6600,stroke:#fff,stroke-width:2px,color:#fff
    style B fill:#8E75B2,stroke:#fff,stroke-width:2px,color:#fff
    style C fill:#f9a826,stroke:#fff,stroke-width:2px,color:#fff
    style D fill:#00bfff,stroke:#fff,stroke-width:2px,color:#fff
    style E fill:#3ECF8E,stroke:#fff,stroke-width:2px,color:#fff
    style F fill:#3ECF8E,stroke:#fff,stroke-width:2px,color:#fff
    style G fill:#8E75B2,stroke:#fff,stroke-width:2px,color:#fff
    style H fill:#3ECF8E,stroke:#fff,stroke-width:2px,color:#fff
```

## Tools & Tech Stack
- **Automation Engine:** n8n Workflow Automation
- **AI Model:** Google Gemini API (`gemini-1.5-flash`)
- **Database:** Supabase / PostgreSQL
- **Scripting:** JavaScript (ES6)
