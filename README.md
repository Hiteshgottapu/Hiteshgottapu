<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0a0f1e,50:0d1b2a,100:112240&height=210&section=header&text=Hitesh%20Gottapu&fontSize=54&fontColor=e2e8f0&animation=fadeIn&fontAlignY=38&desc=AI%20Engineer%20%E2%80%94%20Production%20LLM%20Systems%20%7C%20Data%20Engineering%20%7C%20AI%20Safety&descSize=16&descAlignY=62&descColor=7dd3fc)

[![Portfolio](https://img.shields.io/badge/Portfolio-0d1b2a?style=for-the-badge&logo=vercel&logoColor=7dd3fc)](https://hiteshgottapuprotfolio.netlify.app/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0d1b2a?style=for-the-badge&logo=linkedin&logoColor=7dd3fc)](https://www.linkedin.com/in/hitesh-data-scientist)
[![Email](https://img.shields.io/badge/Email-0d1b2a?style=for-the-badge&logo=gmail&logoColor=7dd3fc)](mailto:hiteshgottapu@gmail.com)

</div>

---

## What I Build

I design and ship **end-to-end AI systems** that solve real problems — from multi-source data ingestion pipelines to deterministic LLM safety engines. My work sits at the intersection of production backend engineering, applied AI, and data infrastructure.

Every system I build is production-first: modular, observable, and safe by design — not as an afterthought.

---

## Featured Projects

---

### ⚖️ Brahmo — Legal AI Safety Engine
**Deterministic Citation Verification Pipeline for Indian Legal AI**

> LLMs hallucinate legal citations at a 5–15% error rate. A single unverified citation in a legal brief is a professional liability. Brahmo enforces zero false negatives through a layered deterministic safety pipeline that wraps every LLM output before it reaches a lawyer's desk.

**The core engineering problem:** Generic LLMs default to the repealed Indian Penal Code (IPC) rather than the active Bharatiya Nyaya Sanhita (BNS) enforced since July 1, 2024 — and hallucinate case references that look real but don't exist.

#### Architecture: Dual-Loop Safety Design

```
USER QUERY
    │
    ▼
┌─────────────────────────────────────────────────────────┐
│ PASS 1 — Query Ingestion & Context Assembly             │
│                                                         │
│  SectionNormalizer  ──▶  RAGRetriever                  │
│  (IPC → BNS maps)        (text-embedding-004, 768-dim) │
│                                ▼                        │
│                     PostgreSQL Cosine Distance RPC      │
│                     (match_chunks, threshold: 0.4)      │
│                                ▼                        │
│                     RAG-Augmented Prompt Assembly       │
└────────────────────────────┬────────────────────────────┘
                             ▼
              ╔══════════════════════════╗
              ║  GEMINI API (Non-Det.)   ║  ← probabilistic boundary
              ║  Raw Unprotected Output  ║
              ╚══════════════╤═══════════╝
                             ▼
┌─────────────────────────────────────────────────────────┐
│ PASS 2 — The Citation Safety Engine Loop                │
│                                                         │
│  CitationExtractor        6 compiled regex patterns     │
│       ▼                                                 │
│  CitationRepair           formatting & casing fixes     │
│       ▼                                                 │
│  TIER 1: HallucinationDetector    0ms rule-boundary     │
│  ├── Future year check (blocks 2028+ citations)         │
│  ├── Impossible SCC volume check (Vol > 20 blocked)     │
│  └── Pre-1900 date filter                               │
│       ▼                                                 │
│  TIER 2: LRU Cache Gate           OrderedDict(1024)     │
│  ├── Token normalization (lowercase + strip)            │
│  ├── Duplicate vector collapse                          │
│  └── ₹0.80 savings logged per cache hit                │
│       ▼                                                 │
│  TIER 3: Cache-Miss Resolver      zero network latency  │
│  └── Unmapped citations → UNVERIFIED / REMOVED state    │
└────────────────────────────┬────────────────────────────┘
                             ▼
              CitationAnnotator → Badge-Annotated HTML
              Session Telemetry → Supabase async commit
              React 3-Column Dashboard refresh
```

#### 3-Column Workspace Interface

| Column | Role |
|---|---|
| **Input & Context** | Query ingestion, session history portal, legal matter switching |
| **Generation Split** | Side-by-side: Raw LLM output vs. Brahmo-verified safe output |
| **Audit & Alerts** | Real-time telemetry, IPC→BNS section alerts, precedent audit log |

#### Engineering Decisions Worth Defending

**Why `collections.OrderedDict` over Redis?** The LRU gate is architected to swap to a Redis cluster with zero core refactoring — the interface contract is identical. Local OrderedDict keeps latency at 0ms for the current scale.

**Why FastAPI + ASGI?** Legal queries require concurrent LLM API calls and database writes. ASGI's native asyncio prevents thread-blocking under concurrent load without spinning up worker pools.

**Why pgvector at 768 dimensions?** Standardized on Google `text-embedding-004`'s exact footprint — eliminates dimensionality mismatch errors during cosine distance comparisons and gives deterministic query execution times.

**Why Pydantic at every boundary?** The `ResponseNode` token stream must pass strict schema validation before entering the safety engine. Silent failures in legal AI are unacceptable.

```python
# The Cache Warm-Up strategy
@app.on_event("startup")
async def warm_cache():
    # Pre-populates the LRU gate on server boot
    # Eliminates cold-cache latency drops in production
    await populate_local_cache_from_db()
```

`FastAPI` `React + Vite` `TypeScript` `Supabase` `PostgreSQL` `pgvector` `Gemini 2.0 Flash` `TailwindCSS` `Pydantic`

---

### 📡 FluxAI — Enterprise Marketing Intelligence Platform
**Multi-Source Data Lake with AI-Ready Analytics Infrastructure**

> Enterprise marketing data lives in silos — Google Ads, Meta Ads, HubSpot CRM, client-owned S3 buckets, Snowflake warehouses — each with its own auth model, schema, and cadence. FluxAI unifies all of it into a single, AI-ready data lake with clean separation between ingestion, processing, and consumption layers.

#### Architecture: 6-Layer Medallion Pipeline

```
INGESTION SOURCES
  Google Ads ──╮
  Meta Ads   ──┤──▶  Centralized OAuth 2.0 Auth Service  ──▶  Token lifecycle mgmt
  HubSpot    ──┤      (AWS CloudFormation, least-privilege IAM)     │
  AWS S3     ──┤                                                     │
  Snowflake  ──╯                                                     ▼
                                                       ┌────────────────────────┐
                                                       │  Internal Raw Data Lake │
                                                       │  (AWS S3)               │
                                                       │  project_id partitioned │
                                                       │  credential metadata    │
                                                       │  stored as JSON         │
                                                       └───────────┬────────────┘
                                                                   ▼
                                              ┌────────────────────────────────────┐
                                              │   Preprocessing & Integration      │
                                              │   Date-based filtering             │
                                              │   Cross-channel + CRM combination  │
                                              │   Cleaning & normalization         │
                                              └──────────────┬─────────────────────┘
                                                             ▼
                                    ┌──────────────────────────────────────────┐
                                    │  Analytics & Reporting Layer             │
                                    │  Pandas Profiling (Automated EDA)        │
                                    │  Dashboard / Reporting (Stakeholder UI)  │
                                    └──────────────────────────────────────────┘
```

#### My Ownership (Built End-to-End)

- **OAuth 2.0 ingestion pipelines** for Google Ads, Meta Ads, HubSpot — scoped authorization flows, token refresh lifecycle
- **Centralized auth service** — credential isolation per project, no hard-coded secrets
- **AWS S3 raw data lake** — `project_id` partitioning with connection metadata as JSON sidecars
- **Preprocessing pipeline** — date-based filtering, cross-channel join logic, CRM combination, cleaning & normalization
- **Backend API layer** — orchestration service enabling the analytics interface
- **Dashboard & reporting layer** — insights interface for stakeholder consumption

#### Design Principles Applied

| Principle | Implementation |
|---|---|
| **Security-first** | No hard-coded credentials. Least-privilege IAM. Clear separation of concerns at every layer. |
| **Reliability** | Failure handling and input validation at ingestion. Reproducible data flows. |
| **Scalability** | Modular, tenant-aware ingestion pipelines. S3 partitioning designed for horizontal growth. |
| **Extensibility** | Additional data sources or agent-driven workflows plug in without structural rework. |

`Python` `FastAPI` `AWS S3` `AWS CloudFormation` `Snowflake` `PostgreSQL` `OAuth 2.0` `Pandas` `Docker`

---

### 🏥 MedScript — AI Prescription Intelligence Platform

> Handwritten medical prescriptions are unstructured, inconsistent, and nearly impossible to query at scale. MedScript digitizes them through a multi-stage OCR and NER pipeline, then wraps the structured output in a RAG-based Q&A system so patients can query their own medical history in plain language.

```
Prescription Image
    ▼
OpenCV OCR Pipeline  ──▶  Clinical NER (drug names, dosages, frequencies)
    ▼
Structured Medical Records (PostgreSQL)
    ▼
pgvector Semantic Index  ──▶  RAG Retriever  ──▶  Gemini API  ──▶  Patient-Friendly Explanation
```

`Python` `OpenCV` `PyTorch` `FastAPI` `PostgreSQL` `pgvector` `Gemini API`

---

### 🤖 QLoRA Fine-Tuning — Domain-Adaptive Coding Assistant
**Domain-Adapting Qwen2.5-1.5B-Instruct for Coding Q&A Using QLoRA**

> General-purpose instruction-tuned models answer programming questions reasonably well — but aren't optimized for a specific coding style or domain. This project adapts Qwen2.5-1.5B-Instruct to coding tasks by training only 4.36 million of its 1.55 billion parameters. Full fine-tuning would require updating every weight; QLoRA updates 0.28%.

#### Architecture & Training Pipeline

```
CodeAlpaca-20k (instruction-response pairs)
    ▼
Data Preparation
  Format: ### Instruction: <text> / ### Response: <output>
  Train: 4,500 samples  |  Validation: 500 samples
    ▼
4-bit NF4 Quantization (BitsAndBytes)
  Base weights compressed → frozen, loaded in 4-bit
    ▼
LoRA Adapter Injection (PEFT)
  Rank r=16  |  Alpha=32  |  Dropout=0.05
  Lightweight adapter matrices injected into attention layers
  Only adapters are updated — base weights stay frozen
    ▼
Training (Kaggle GPU, ~4hr 20min)
  Epochs: 2  |  LR: 2e-4  |  Batch: 2  |  Grad Accum: 4
    ▼
Evaluation on unseen coding prompts
  Binary Search · BFS · Linked List Cycle Detection
  SQL Query Generation · Array Manipulation
```

#### Parameter Efficiency — The Core Result

| Metric | Value |
|---|---|
| Total Parameters | 1,548,072,448 |
| Trainable Parameters | 4,358,144 |
| Trainable % | **0.2815%** |
| Training Time | ~4 hours 20 minutes |

Only **4.36M parameters updated** out of 1.55B. The base model weights were never touched.

#### Training Results

| Epoch | Training Loss | Validation Loss |
|---|---|---|
| 1 | 0.1270 | 0.1253 |
| 2 | 0.1186 | 0.1248 |

Training and validation loss stayed tightly aligned across both epochs — no overfitting signal despite the small dataset size. The fine-tuned model produced more concise, implementation-focused answers on coding tasks while the base model stayed stronger on conceptual explanations — the expected behaviour of domain adaptation, not domain replacement.

#### Why These Hyperparameters

**LoRA rank r=16, alpha=32** — alpha/rank ratio of 2 is the standard scaling for stable adapter training. Lower rank (r=8) under-fits domain-specific patterns; higher rank starts approaching full fine-tune cost with diminishing returns at this model size.

**4-bit NF4 over INT8** — NF4 (Normal Float 4) is information-theoretically optimal for normally distributed weights, which LLM parameters approximate closely. INT8 would require 2× the memory for marginal quality gain on a 1.5B model.

**Gradient accumulation steps=4 with batch size=2** — effective batch size of 8 without exceeding GPU VRAM limits. Larger effective batch stabilizes gradient estimates during LoRA training.

`Python` `PyTorch` `Hugging Face Transformers` `PEFT` `BitsAndBytes` `Datasets` `Kaggle GPU`

---

## Tech Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-0d1b2a?style=for-the-badge&logo=python&logoColor=7dd3fc)
![FastAPI](https://img.shields.io/badge/FastAPI-0d1b2a?style=for-the-badge&logo=fastapi&logoColor=7dd3fc)
![PyTorch](https://img.shields.io/badge/PyTorch-0d1b2a?style=for-the-badge&logo=pytorch&logoColor=7dd3fc)
![React](https://img.shields.io/badge/React-0d1b2a?style=for-the-badge&logo=react&logoColor=7dd3fc)
![TypeScript](https://img.shields.io/badge/TypeScript-0d1b2a?style=for-the-badge&logo=typescript&logoColor=7dd3fc)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-0d1b2a?style=for-the-badge&logo=postgresql&logoColor=7dd3fc)
![Supabase](https://img.shields.io/badge/Supabase-0d1b2a?style=for-the-badge&logo=supabase&logoColor=7dd3fc)
![AWS](https://img.shields.io/badge/AWS-0d1b2a?style=for-the-badge&logo=amazonaws&logoColor=7dd3fc)
![Snowflake](https://img.shields.io/badge/Snowflake-0d1b2a?style=for-the-badge&logo=snowflake&logoColor=7dd3fc)
![Docker](https://img.shields.io/badge/Docker-0d1b2a?style=for-the-badge&logo=docker&logoColor=7dd3fc)
![Hugging Face](https://img.shields.io/badge/HuggingFace-0d1b2a?style=for-the-badge&logo=huggingface&logoColor=7dd3fc)
![Vite](https://img.shields.io/badge/Vite-0d1b2a?style=for-the-badge&logo=vite&logoColor=7dd3fc)

</div>

| Layer | Tools |
|---|---|
| **AI / LLM** | Gemini 2.0 Flash · OpenAI API · LangChain · QLoRA · PEFT · BitsAndBytes |
| **ML / Data Science** | PyTorch · Hugging Face Transformers · Pandas · Scikit-learn · OpenCV |
| **Vector / RAG** | pgvector · 768-dim embeddings · cosine distance RPC · semantic chunking |
| **Backend** | FastAPI (ASGI) · Pydantic · Python 3.10+ · Uvicorn |
| **Frontend** | React 18 · TypeScript · Vite · TailwindCSS |
| **Database** | PostgreSQL · Supabase · JSONB · pgvector · RLS enforcement |
| **Data Engineering** | AWS S3 · Snowflake · OAuth 2.0 · Medallion ETL · Pandas Profiling |
| **DevOps** | Docker · AWS CloudFormation · GitHub Actions |

---

## Engineering Principles

**Production-first, not notebook-first.** Every system is designed for deployment — async APIs, schema validation at boundaries, graceful failure handling.

**Determinism wraps non-determinism.** LLMs are probabilistic engines. My safety architecture treats them as untrusted inputs and enforces deterministic constraints at every output boundary.

**Modular by contract.** Each pipeline stage has a defined input/output contract. Swapping Gemini for another LLM, or OrderedDict for Redis, requires changing one adapter — not rewriting the system.

**Secure by design.** No hard-coded credentials. Least-privilege IAM. Per-tenant data isolation. RLS enforcement at the database layer.

---

## GitHub Stats

<div align="center">

![Hitesh's GitHub Stats](https://github-readme-stats.vercel.app/api?username=HiteshGottapu&show_icons=true&theme=transparent&hide_border=true&title_color=7dd3fc&icon_color=7dd3fc&text_color=94a3b8&rank_icon=github)
&nbsp;
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=HiteshGottapu&layout=compact&theme=transparent&hide_border=true&title_color=7dd3fc&text_color=94a3b8)

</div>

---

<div align="center">

*Open to AI/ML engineering roles, production AI consulting, and applied research collaborations.*

[![Portfolio](https://img.shields.io/badge/See%20Full%20Portfolio-0d1b2a?style=for-the-badge&logo=vercel&logoColor=7dd3fc)](https://hiteshgottapuprotfolio.netlify.app/)

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:112240,50:0d1b2a,100:0a0f1e&height=100&section=footer)

</div>
