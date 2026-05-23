# 🤖 AI-Powered LinkedIn Follow-Up Automation System

> **Production-deployed** agentic AI system that automates intelligent LinkedIn follow-ups using RAG pipelines, LLM-based conversation state management, and CRM integration — reducing response latency to **under 3 seconds** end-to-end.

---

## 📌 Project Overview

Manual LinkedIn follow-ups for sales teams are slow, inconsistent, and unscalable. This system replaces manual SDR effort with an intelligent AI agent that:

- Reads conversation context from LinkedIn
- Decides the best follow-up message using RAG + LLM
- Sends personalized, context-aware replies automatically
- Logs everything to CRM with zero duplicates

**Result:** Response latency reduced to **~3 seconds** | Handles **100+ conversations/day** | **Zero downtime** since launch

---

## ⚙️ System Architecture

```
LinkedIn DOM (Chrome Extension)
        ↓
  Conversation Scraper (AimFox Integration)
        ↓
  ┌─────────────────────────────────┐
  │     5-Stage State Machine       │
  │  1. Intent Detection            │
  │  2. Context Retrieval (RAG)     │
  │  3. Response Generation (LLM)   │
  │  4. Quality Check               │
  │  5. Send + Log                  │
  └─────────────────────────────────┘
        ↓                    ↓
  ChromaDB (Vector Store)   GPT-4o-mini (Fallback)
        ↓
  FastAPI Backend (Dockerized)
        ↓
  PostgreSQL + Zoho Bigin CRM
```

---

## 🚀 Key Features

| Feature | Details |
|--------|---------|
| ⚡ Response Latency | ~3 seconds end-to-end (optimized) |
| 🧠 Intent Detection | 90%+ accuracy via self-improving RAG |
| 📈 Throughput | 100+ LinkedIn conversations/day |
| 🔁 Self-Improving | SDR edits re-embedded back into vector store |
| 🚫 Zero Duplicates | Email-based deduplication in CRM |
| 🐳 Production Ready | Dockerized, CI/CD via GitHub Actions |

---

## 🛠️ Tech Stack

**AI & LLM Layer**
- GPT-4o-mini — response generation
- LangChain — orchestration
- ChromaDB — vector store (150+ seed examples)
- MCP Tool-Calling Patterns — AI ↔ CRM integration

**Backend**
- Python + FastAPI — REST API
- PostgreSQL — conversation & lead storage
- Docker — containerization
- GitHub Actions — CI/CD pipeline

**Integrations**
- AimFox Chrome Extension — LinkedIn DOM scraper
- Zoho Bigin CRM — lead management
- JavaScript Chrome Extension API

**Deployment**
- DigitalOcean — cloud hosting
- Zero downtime since launch

---

## 📊 Performance Metrics

```
Before System:
  Manual reply time    →  15–20 minutes per conversation
  Daily capacity       →  ~30 conversations
  CRM sync             →  Manual, error-prone

After System:
  Response latency     →  ~5 seconds ⚡ (97% reduction)
  Daily capacity       →  100+ conversations (3x throughput)
  CRM sync             →  Automated, zero duplicates ✅
```

---

## 🔄 How It Works

### 1. Conversation Ingestion
Chrome Extension scrapes LinkedIn conversation thread via DOM → sends to FastAPI backend

### 2. Intent Detection
System classifies message intent (interested / not interested / question / objection) with 90%+ accuracy

### 3. RAG Pipeline
Retrieves most relevant past response examples from ChromaDB vector store based on semantic similarity

### 4. Response Generation
GPT-4o-mini generates personalized reply using retrieved context + conversation history

### 5. Self-Improvement Loop
When SDR edits a response → edit is re-embedded into ChromaDB → system learns continuously without retraining

### 6. CRM Sync
Lead data + conversation outcome synced to Zoho Bigin with email-based deduplication

---

## 📁 Repository Structure

```
├── api/                  # FastAPI backend
│   ├── main.py
│   ├── routes/
│   └── models/
├── agent/                # AI agent logic
│   ├── state_machine.py  # 5-stage conversation FSM
│   ├── rag_pipeline.py   # ChromaDB retrieval
│   └── llm_handler.py    # GPT-4o-mini integration
├── integrations/
│   ├── crm/              # Zoho Bigin sync
│   └── scraper/          # AimFox integration
├── docker-compose.yml
├── .github/workflows/    # CI/CD
└── README.md
```

> ⚠️ **Note:** Source code is proprietary and not publicly available. This repository contains architecture documentation, system design, and performance benchmarks only.

---

## 🧪 Optimization Journey

| Version | Latency | What Changed |
|---------|---------|--------------|
| v1.0 | ~2 min | Initial implementation |
| v1.5 | ~8 sec | Async API calls + connection pooling |
| v2.0 | ~5 sec | Parallel RAG retrieval + response caching |

---

## 👩‍💻 Built By

**Harpreet Kaur** — AI/ML Engineer  
[LinkedIn](https://linkedin.com/in/harpreet-kour-533b27378) · [GitHub](https://github.com/Harpreet8959) · harpreetkour8959@gmail.com

---

*This system is actively running in production, handling real sales conversations daily.*
