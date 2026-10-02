# Lead-enrichment-and-AI-cost-engine
Production-grade n8n automation pipeline for real-time B2B lead enrichment, LLM personalization, dynamic token cost tracking, and CRM sync.

<div align="center">

# ⚡ AURA

### Autonomous B2B outreach intelligence and unit-economic cost engine

*Real-time ICP qualification, live search grounding via Serper API, structured Gemini 1.5 JSON synthesis, and sub-cent unit-economic cost tracking.*

[![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-1.5%20Flash-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini/)
[![Serper API](https://img.shields.io/badge/Serper-Google%20Search%20API-34A853?style=for-the-badge&logo=googlechrome&logoColor=white)](https://serper.dev)
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-CRM%20Layer-0F9D58?style=for-the-badge&logo=googlesheets&logoColor=white)](https://sheets.google.com)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

</div>

---

## 📄 Overview

Traditional B2B outbound workflows either rely on static templates with low reply rates or blind LLM batch generation that exhausts API rate limits and hallucinates facts. **AURA** addresses this by serving as an event-driven outbound personalization infrastructure with built-in cost tracking.

Incoming webhook events trigger a multi-stage validation, web enrichment, and AI reasoning loop. Every prospect is deduplicated against real-time operational windows, scored against rule-based ICP thresholds, grounded via live Google Search results, and synthesized into custom cold outreach hooks via **Gemini 1.5 Flash** with strict JSON schemas.

1. **Evaluates ICP fit** - Filters out non-target companies based on headcount, industry criteria, and data completeness.
2. **Executes live web grounding** - Pulls live search snippets (news, announcements, press releases) via Serper API to prevent LLM hallucinations.
3. **Structured synthesis** - Forces Gemini 1.5 Flash through strict output parsers into a validated JSON schema (`subject_line`, `icebreaker`, `pain_point`).
4. **Calculates real-time unit Economics** - Measures prompt/completion tokens and logs exact infrastructure costs (sub-$0.0002/lead).
5. **Human-in-the-Loop Delivery** - Creates staged drafts in Gmail and broadcasts instant rich Telegram alerts with CRM direct links.

---

## 📸 Visual Showcase

| Lead Ingestion and web enrichment engine | Guardrails, unit economics and execution |
| :---: | :---: |
| <img src="assets/01_ingest_and_enrichment.png" width="100%" alt="Ingestion & Grounding Canvas"/> | <img src="assets/02_cost_guardrails_dispatch.png" width="100%" alt="Guardrails & Dispatch Canvas"/> |
| *Webhook Ingestion, Temporal Deduplication, Serper Search & Gemini 1.5 Synthesis* | *Token Cost Calculation, Quality Validation Guardrail & Gmail Draft Automation* |

| Live Qualified CRM with Dynamic Unit-Economics | ICP Disqualification Quarantine Ledger |
| :---: | :---: |
| <img src="assets/03_qualified_crm_costs.png" width="100%" alt="Google Sheets Qualified Tracker"/> | <img src="assets/04_disqualified_routing.png" width="100%" alt="Google Sheets Disqualified Quarantine"/> |
| *Structured CRM Sync: Real-Time Token Tracking & Sub-Cent Infrastructure Cost ($/Lead)* | *Zero-Waste Retention: Automatic ICP Disqualification Routing for Cold Retargeting* |

<div align="center">

### Real-Time SDR Notification Delivery

<img src="assets/05_telegram_outreach_card.png" width="60%" alt="Telegram Dispatch Card"/>

*Rich Telegram Alert with Company Meta, Live Context hook & direct deep link to staged gmail draft*

</div>

---

## 🏗 System & Pipeline Architecture

```mermaid
graph TD
    subgraph INGESTION ["1. Ingestion and deduplication"]
        WH[Incoming Webhook POST] --> DEDUP[Remove duplicates node]
        DEDUP --> ICP{ICP Qualified?}
    end

    subgraph DISQUALIFICATION ["Disqualification routing"]
        ICP -->|Score < 60| META_DISQ[Set disqualified metadata]
        META_DISQ --> SHEET_DISQ[(Google Sheets: disqualified)]
    end

    subgraph ENRICHMENT ["2. Enrichment & Grounding"]
        ICP -->|Score >= 60| SCORE[Calculate lead score node]
        SCORE --> SERPER[Serper API: Live Google Search]
    end

    subgraph AI_CORE ["3. Generative Synthesis & Guardrails"]
        SERPER --> LLM_NODE[Generate outreach hook: Gemini 1.5]
        SCHEMA[JSON Schema Parser] -.-> LLM_NODE
        LLM_NODE --> COST[Calculate AI cost and tokens]
        COST --> GUARD{Guardrail: valid hook?}
        GUARD -->|Fail: Empty / short| ERR_TG[Telegram error alert]
        LLM_NODE -.->|API Error| ERR_TG
    end

    subgraph EXECUTION ["4. Execution and notification"]
        GUARD -->|Pass: Length > 20| SHEET_QUAL[(Google Sheets: CRM Sync)]
        SHEET_QUAL --> GMAIL[Gmail API: Create Draft]
        GMAIL --> TG[Sales team telegram broadcast]
    end

    style WH fill:#1e293b,stroke:#38bdf8,stroke-width:2px,color:#fff
    style LLM_NODE fill:#1e293b,stroke:#818cf8,stroke-width:2px,color:#fff
    style SHEET_QUAL fill:#064e3b,stroke:#34d399,stroke-width:2px,color:#fff
    style ERR_TG fill:#450a0a,stroke:#f87171,stroke-width:2px,color:#fff
```

---

## ⚡ 10-Step Pipeline Execution Flow

```text
Step 0   WEBHOOK_INGEST      - Receives structured POST payload from forms, CRM, or lead lists
Step 1   DEDUP_FILTER        - In-memory hash deduplication against (company + contact_email)
Step 2   ICP_EVALUATOR       - Rule-based heuristics evaluating headcount, industry target, and domain validity
Step 2b  DISQ_RECORD         - Sub-threshold leads diverted to Google Sheets 'Disqualified' sheet for retargeting
Step 3   SERPER_SEARCH       - Live SERP search query constructed dynamically: "<Company> business announcement expansion news"
Step 4   SNIPPET_EXTRACTION  - Top organic snippets parsed and sanitized into a contextual string
Step 5   LLM_INFERENCE       - Gemini 1.5 Flash synthesizes custom email components via system prompt
Step 6   SCHEMA_VALIDATION   - Structured JSON Output Parser validates icebreaker, pain point, and subject line
Step 7   COST_ACCOUNTING     - Mathematical calculation of input/output token cost via Gemini pricing formulas
Step 8   STRING_GUARDRAIL    - Null check & string length verification (> 20 chars) to prevent empty drafts
Step 9   CRM_PERSISTENCE     - Qualified row appended to Google Sheets CRM with score, tokens, and USD cost
Step 10  DISPATCH_EXECUTION  - Draft created in Gmail inbox; rich Markdown card dispatched to Telegram SDR bot
```
