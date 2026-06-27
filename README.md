<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0a0a0f,50:0d1b2a,100:1a2744&height=200&section=header&text=Hitesh%20Gottapu&fontSize=52&fontColor=e2e8f0&animation=fadeIn&fontAlignY=38&desc=AI%20Engineer%20%E2%80%94%20LLM%20Applications%20%7C%20Data%20Engineering%20%7C%20Production%20AI&descSize=16&descAlignY=62&descColor=94a3b8)

[![Portfolio](https://img.shields.io/badge/Portfolio-0d1b2a?style=for-the-badge&logo=vercel&logoColor=38bdf8)](https://hiteshgottapuprotfolio.netlify.app/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0d1b2a?style=for-the-badge&logo=linkedin&logoColor=38bdf8)](https://www.linkedin.com/in/hitesh-data-scientist)
[![Email](https://img.shields.io/badge/Email-0d1b2a?style=for-the-badge&logo=gmail&logoColor=38bdf8)](mailto:hiteshgottapu@gmail.com)

</div>

---

## 🧠 What I Build

I design and ship **end-to-end AI systems** — from raw data ingestion to production-grade LLM inference — that solve real enterprise problems. My work sits at the intersection of machine learning, backend engineering, and applied AI safety.

```
Data Engineering  →  Model Development  →  Production Deployment  →  Safety Validation
```

**Core focus areas:**
- **RAG Systems** — Semantic retrieval pipelines with vector databases
- **LLM Fine-Tuning** — QLoRA / PEFT for domain adaptation
- **AI Safety** — Hallucination mitigation, citation validation, output verification
- **Enterprise Data Pipelines** — Medallion ETL, multi-tenant SaaS integrations
- **FastAPI AI Services** — Production inference APIs with auth and observability

---

## 🚀 Featured Projects

### FluxAI — Enterprise Marketing Intelligence Platform

> *Fragmented SaaS marketing data unified into AI-ready infrastructure*

```
Google Ads  ╮
Meta Ads    ├──▶  OAuth 2.0  ──▶  Medallion ETL (Bronze → Silver → Gold)  ──▶  FastAPI  ──▶  AI Workloads
HubSpot     ╯                          Snowflake + AWS S3
```

**What makes it real:**
- Multi-tenant architecture with per-client data isolation
- OAuth 2.0 ingestion pipelines for 3 SaaS platforms
- Medallion ETL: raw ingestion → cleaned → business-ready gold layer
- FastAPI inference layer decoupled from the data pipeline

`Python` `FastAPI` `PostgreSQL` `Snowflake` `AWS S3` `Docker` `OAuth 2.0`

---

### MedScript — AI Prescription Intelligence Platform

> *Turning handwritten prescriptions into structured, queryable medical records*

```
Prescription Image  ──▶  OpenCV OCR  ──▶  Clinical NER  ──▶  pgvector RAG  ──▶  LLM  ──▶  Patient Explanation
```

**What makes it real:**
- Custom OCR pipeline handling noisy handwritten inputs
- Clinical entity extraction (drug names, dosages, frequencies)
- RAG-based medical Q&A grounded in the patient's own records
- FastAPI service with modular inference stages

`Python` `OpenCV` `PyTorch` `FastAPI` `PostgreSQL` `Gemini API`

---

### Brahmo — Legal AI Safety Engine

> *LLMs hallucinate legal citations. This system catches them before they cause harm.*

```
User Query  ──▶  LLM Generation  ──▶  Citation Extraction  ──▶  Deterministic Verification  ──▶  Safe Response
                                                                         ↕
                                                              Rule-Based Legal Validator
```

**What makes it real:**
- Deterministic citation verification — no model guessing
- Thread-safe LRU cache for repeated legal lookups
- Hallucination rate tracking across response batches
- Production-grade FastAPI + React frontend

`FastAPI` `React` `PostgreSQL` `pgvector` `Gemini API`

---

### QLoRA Fine-Tuning — Domain-Adaptive Coding Assistant

> *Specializing a 1.5B parameter model for coding Q&A with minimal compute*

```
CodeAlpaca Dataset  ──▶  4-bit NF4 Quantization  ──▶  QLoRA + PEFT  ──▶  Qwen2.5-1.5B  ──▶  Eval  ──▶  Inference
```

**What makes it real:**
- 4-bit NF4 quantization — runs on consumer hardware
- PEFT adapter training (not full fine-tune — surgical and efficient)
- Automated evaluation pipeline with held-out coding benchmarks
- Drop-in inference, no framework lock-in

`Python` `PyTorch` `Hugging Face` `PEFT` `BitsAndBytes` `Transformers`

---

## 🛠️ Tech Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-0d1b2a?style=for-the-badge&logo=python&logoColor=38bdf8)
![FastAPI](https://img.shields.io/badge/FastAPI-0d1b2a?style=for-the-badge&logo=fastapi&logoColor=38bdf8)
![PyTorch](https://img.shields.io/badge/PyTorch-0d1b2a?style=for-the-badge&logo=pytorch&logoColor=38bdf8)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-0d1b2a?style=for-the-badge&logo=postgresql&logoColor=38bdf8)
![Docker](https://img.shields.io/badge/Docker-0d1b2a?style=for-the-badge&logo=docker&logoColor=38bdf8)
![AWS](https://img.shields.io/badge/AWS-0d1b2a?style=for-the-badge&logo=amazonaws&logoColor=38bdf8)
![Snowflake](https://img.shields.io/badge/Snowflake-0d1b2a?style=for-the-badge&logo=snowflake&logoColor=38bdf8)
![Hugging Face](https://img.shields.io/badge/HuggingFace-0d1b2a?style=for-the-badge&logo=huggingface&logoColor=38bdf8)

</div>

| Layer | Tools |
|---|---|
| **AI / ML** | PyTorch · Hugging Face · PEFT · BitsAndBytes · QLoRA · OCR |
| **LLM APIs** | Gemini · OpenAI · LangChain |
| **Vector & RAG** | pgvector · FAISS · Semantic Search |
| **Backend** | FastAPI · PostgreSQL · OAuth 2.0 |
| **Data Engineering** | Snowflake · AWS S3 · Medallion ETL |
| **DevOps** | Docker · GitHub Actions |

---

## 📐 Engineering Principles

- **Production-first** — every system is designed to run in prod, not just notebooks
- **Modular AI pipelines** — each stage is testable, replaceable, and observable
- **Safety by design** — validation layers are not optional; they're part of the architecture
- **API-first backends** — clean separation of inference from data and UI layers
- **Efficient fine-tuning** — domain adaptation without cluster-scale compute

---

## 📊 GitHub Stats

<div align="center">

![Hitesh's GitHub Stats](https://github-readme-stats.vercel.app/api?username=HiteshGottapu&show_icons=true&theme=transparent&hide_border=true&title_color=38bdf8&icon_color=38bdf8&text_color=94a3b8&rank_icon=github)
&nbsp;
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=HiteshGottapu&layout=compact&theme=transparent&hide_border=true&title_color=38bdf8&text_color=94a3b8)

</div>

---

<div align="center">

*Open to AI/ML engineering roles, research collaborations, and production AI consulting.*

[![Portfolio](https://img.shields.io/badge/See%20Full%20Portfolio-0d1b2a?style=for-the-badge&logo=vercel&logoColor=38bdf8)](https://hiteshgottapuprotfolio.netlify.app/)

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:1a2744,50:0d1b2a,100:0a0a0f&height=100&section=footer)

</div>
