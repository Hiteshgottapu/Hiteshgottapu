<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0a0f1e,50:0d1b2a,100:112240&height=180&section=header&text=Hitesh%20Gottapu&fontSize=48&fontColor=e2e8f0&animation=fadeIn&fontAlignY=40&desc=AI%20Systems%20Engineer&descSize=18&descAlignY=62&descColor=7dd3fc)

[![Portfolio](https://img.shields.io/badge/Portfolio-0d1b2a?style=for-the-badge&logo=vercel&logoColor=7dd3fc)](https://hiteshgottapuprotfolio.netlify.app/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0d1b2a?style=for-the-badge&logo=linkedin&logoColor=7dd3fc)](https://www.linkedin.com/in/hitesh-data-scientist)
[![Email](https://img.shields.io/badge/Email-0d1b2a?style=for-the-badge&logo=gmail&logoColor=7dd3fc)](mailto:hiteshgottapu@gmail.com)

</div>

---

I build production AI systems — RAG pipelines, LLM safety engines, data infrastructure, and fine-tuned models — designed to run reliably in production, not just in notebooks.

**Focus areas:** LLM infrastructure · AI safety · backend engineering · data pipelines · vector search · parameter-efficient fine-tuning

---

## Systems

### Brahmo — Legal AI Safety Engine

LLMs hallucinate legal citations at a 5–15% error rate. In legal workflows, a single unverified citation is a liability.

Brahmo wraps every LLM output in a deterministic, multi-tier safety pipeline before it reaches a lawyer's desk. The system enforces zero false negatives by treating the LLM as an untrusted input source and verifying its output against rule-based and cache-backed validators.

```
Query
  └─▶ SectionNormalizer (IPC → BNS statute mapping)
        └─▶ RAGRetriever (text-embedding-004 · 768-dim · pgvector)
              └─▶ Gemini 2.0 Flash  ←── probabilistic boundary
                    └─▶ CitationExtractor (6 regex patterns)
                          └─▶ CitationRepair
                                └─▶ Tier 1: HallucinationDetector  [0ms · rule-boundary]
                                      └─▶ Tier 2: LRU Cache Gate   [OrderedDict · maxsize=1024]
                                            └─▶ Tier 3: Cache-Miss Resolver
                                                  └─▶ CitationAnnotator → badge-annotated HTML
                                                        └─▶ Supabase async commit → React UI
```

**Engineering decisions**

| Decision | Rationale |
|---|---|
| `OrderedDict` LRU over Redis | Zero-latency local gate; interface contract is Redis-compatible for drop-in swap at scale |
| pgvector at exactly 768 dims | Matches `text-embedding-004` footprint — eliminates dimensionality mismatch at query time |
| Pydantic at every boundary | `ResponseNode` token stream is schema-validated before entering the safety engine; silent failures are unacceptable in legal AI |
| FastAPI ASGI | Concurrent LLM calls and DB writes without thread-blocking; horizontal scale via async |
| Startup cache warm-up | `@app.on_event("startup")` pre-populates the LRU gate — eliminates cold-cache latency drops in production |

**Tier 1 pre-filter catches at 0ms:** future-dated citations · impossible SCC volumes · pre-1900 dates · out-of-range page numbers

`FastAPI` `React` `TypeScript` `Supabase` `PostgreSQL` `pgvector` `Gemini 2.0 Flash` `Pydantic` `Vite` `TailwindCSS`

---

### FluxAI — Enterprise Marketing Intelligence Platform

Enterprise marketing data is fragmented across ad platforms, CRMs, and client-owned warehouses — each with a different auth model, schema, and ingestion cadence. Unified analytics requires solving the infrastructure problem before the AI problem.

FluxAI ingests from five sources into a single project-partitioned data lake, with clean separation between raw storage, processing, and consumption layers.

```
Google Ads ──┐
Meta Ads   ──┤──▶ OAuth 2.0 Auth Service ──▶ AWS S3 Raw Lake (project_id partitioned)
HubSpot    ──┤    (CloudFormation · least-privilege IAM)         │
Client S3  ──┤                                                    ▼
Snowflake  ──┘                                       Preprocessing & Integration
                                                     (date filter · cross-channel join · normalization)
                                                                  │
                                                                  ▼
                                                     Analytics & Reporting Layer
                                                     (Pandas EDA · stakeholder dashboard)
```

**What I owned end-to-end**

- OAuth 2.0 ingestion pipelines for Google Ads, Meta Ads, HubSpot — scoped token lifecycle, refresh handling
- Centralized auth service — per-project credential isolation, no hard-coded secrets
- S3 raw lake — `project_id` partitioning with JSON credential sidecars per connection
- Preprocessing pipeline — date filtering, cross-channel join, CRM combination, normalization
- Backend orchestration API and stakeholder reporting layer

**Design principles**

| Principle | Implementation |
|---|---|
| Security-first | No hard-coded credentials · least-privilege IAM · separation of concerns at every layer |
| Reliability | Failure handling and input validation at ingestion · reproducible data flows |
| Scalability | Tenant-aware pipelines · S3 partitioning designed for horizontal growth |
| Extensibility | Additional sources or agent workflows plug in without structural rework |

`Python` `FastAPI` `AWS S3` `AWS CloudFormation` `Snowflake` `PostgreSQL` `OAuth 2.0` `Plotly` `Docker` `Pyspark`

---

### MedScript — AI Prescription Intelligence Platform

Handwritten medical prescriptions are unstructured and unqueryable at scale. Digitizing them requires solving OCR, clinical entity extraction, and retrieval before an LLM can reason over them reliably.

```
Prescription image
  └─▶ OpenCV OCR pipeline
        └─▶ Clinical NER (drug names · dosages · frequencies)
              └─▶ Structured records (PostgreSQL)
                    └─▶ pgvector semantic index
                          └─▶ RAG retriever
                                └─▶ Gemini API
                                      └─▶ Patient-facing explanation
```

The RAG layer grounds LLM responses in the patient's own structured records — reducing hallucination risk on the domain where it matters most.

`Python` `OpenCV` `PyTorch` `FastAPI` `PostgreSQL` `pgvector` `Gemini API`

---

### QLoRA — Domain-Adaptive Coding Assistant

Full fine-tuning a 1.5B parameter model requires updating every weight. QLoRA injects lightweight adapter layers and freezes the base — achieving domain adaptation at 0.28% of the parameter cost.

```
CodeAlpaca-20k (instruction-response pairs)
  └─▶ 4-bit NF4 quantization (BitsAndBytes)   base weights frozen
        └─▶ LoRA adapter injection (r=16 · alpha=32 · dropout=0.05)
              └─▶ Training: 2 epochs · lr=2e-4 · effective batch=8
                    └─▶ Evaluation on unseen prompts
                          (Binary Search · BFS · SQL · Linked List · Array ops)
```

**Results**

| Metric | Value |
|---|---|
| Total parameters | 1,548,072,448 |
| Trainable parameters | 4,358,144 |
| Trainable % | **0.2815%** |
| Final training loss | 0.1186 |
| Final validation loss | 0.1248 |
| Training time | ~4 hr 20 min (Kaggle GPU) |

Training and validation loss stayed tightly aligned — no overfitting signal at this dataset size. Fine-tuned model improved on implementation tasks; base model remained stronger on conceptual explanations. That asymmetry is the expected signature of domain adaptation, not domain replacement.

**Why these hyperparameters:** Alpha/rank ratio of 2 (alpha=32, r=16) is the standard scaling for stable adapter convergence. NF4 over INT8 — NF4 is information-theoretically optimal for normally distributed weights, which LLM parameters closely approximate. Gradient accumulation steps=4 with batch=2 gives effective batch=8 without exceeding VRAM limits.

`Python` `PyTorch` `Hugging Face Transformers` `PEFT` `BitsAndBytes` `Datasets`

---

## Stack

| Layer | Tools |
|---|---|
| LLM & AI | Gemini 2.0 Flash · OpenAI API · QLoRA · PEFT · BitsAndBytes · LangChain |
| ML | PyTorch · Hugging Face Transformers · OpenCV · Scikit-learn · Pandas |
| Vector / RAG | pgvector · 768-dim embeddings · cosine distance RPC · semantic chunking |
| Backend | FastAPI (ASGI) · Pydantic · Python 3.10+ · Uvicorn |
| Frontend | React 18 · TypeScript · Vite · TailwindCSS |
| Database | PostgreSQL · Supabase · JSONB · pgvector · Row Level Security |
| Data engineering | AWS S3 · Snowflake · OAuth 2.0 · Medallion ETL · Pandas Profiling |
| Infra | Docker · AWS CloudFormation · GitHub Actions |

---

## Engineering principles

**Determinism wraps non-determinism.** LLMs are probabilistic. Every system I build enforces deterministic constraints at the output boundary — schema validation, rule-based pre-filters, cache gates — before results surface to users.

**Modular by contract.** Each pipeline stage exposes a defined input/output interface. Swapping Gemini for another model, or a local LRU cache for Redis, is a one-adapter change — not a system rewrite.

**Production-first.** Async APIs, boundary validation, graceful failure handling, and observability are built in from the start — not added after the fact.

---
