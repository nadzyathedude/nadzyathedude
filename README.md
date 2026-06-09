# Nadezhda Chelyadinova — AI / LLM Engineer

I build production LLM systems: inference pipelines with cost controls, multi-agent orchestration, RAG from scratch, and tooling that ships.

---

## What I work with

**LLM layer** — fine-tuning, confidence-based routing, multi-stage inference, embedding-gate classifiers, evals  
**Agent systems** — LangGraph orchestration, sub-agent patterns, fan-out/fan-in, durable workflows (Temporal)  
**RAG** — custom pipelines without LangChain/LlamaIndex, hybrid retrieval, citation enforcement, incremental sync  
**MCP** — custom MCP servers, Claude Code skills, browser automation  
**Infrastructure** — Redis Streams, Qdrant, FastAPI, Docker, Kubernetes, Prometheus/Grafana  
**Languages** — Python, Go, Swift

---

## Projects

### [sentiment-finetune](https://github.com/nadzyathedude/sentiment-finetune) — Production LLM pipeline on one task
> Full stack of production techniques applied to a single problem (sentiment classification of product reviews)

Five layers that compose into a production-grade inference pyramid:

| Layer | What it does | Result |
|---|---|---|
| **Fine-tuning** | 50-example JSONL dataset → validate → upload → job → poll | Pipeline complete, artifacts ready |
| **Evals** | `confidence_check.py`: self-scoring + redundancy + constraint checks | **15 OK / 2 UNSURE / 0 FAIL, accuracy 79%** |
| **Routing** | `router.py`: gpt-4o-mini → gpt-4o on low confidence | **16 cheap / 1 expensive, 75% cost savings** |
| **Multi-stage** | `multistage.py`: monolithic → 3 stages with strict formats + rule-based aggregator | Unit-tested aggregator |
| **Micro-model first** | `micro_model.py`: embedding centroid classifier as LLM gatekeeper | Cheapest layer, O(1) lookup |

The full pyramid: `embedding-gate → fine-tuned gpt-4o-mini + confidence check → gpt-4o`  


---

### [ci-hub](https://github.com/nadzyathedude/ci-hub) — Multi-agent competitive intelligence system

Production-grade system that monitors competitors via web scraping, news, pricing, and generates weekly digest reports.

**Architecture decisions that matter:**
- **LangGraph** — conditional routing, fan-out/fan-in, subgraph retry loops
- **Sub-agent pattern** — stateless composable units with semaphore-controlled concurrency
- **Two-level parallelism** — top-level fan-out (3 agents) × sub-agent fan-out (N items each)
- **Redis Streams** over Pub/Sub — durability, consumer groups, replay
- **Temporal** — durable scheduling, retries, `continue_as_new` for long-running workflows
- **Qdrant hybrid search** — dense + BM25 sparse with RRF fusion

6 agent groups (WebWatcher, News, Pricing, Analyst, Alerts, Digest), FastAPI + WebSocket for real-time progress, 100+ unit tests with all LLM calls mocked, Kubernetes manifests included.

---

### [notion_mcp_with_rag_memory](https://github.com/nadzyathedude/notion_mcp_with_rag_memory) — MCP server + RAG from scratch

Custom RAG pipeline built without LangChain or LlamaIndex — every component written by hand:

- **Chunker** — fixed-size with overlap, deterministic SHA-256 chunk IDs
- **Retriever** — brute-force cosine similarity, no numpy/FAISS
- **Reranker** — LLM-based rescoring (score 0–1), threshold filtering with fallback
- **Citations** — regex extraction → validation against allowed set → auto-retry on failure
- **Incremental sync** — `last_edited_time` diff, only re-indexes changed Notion pages

3 MCP tools: `sync_notion` / `search_notion_memory` / `ask_notion`  
Tested on 10 Notion pages → 469 chunks indexed. Search "ООП" → top score 0.65, correct page.

---

### [awasameCircuitBreaker](https://github.com/nadzyathedude/awasameCircuitBreaker) — Go Circuit Breaker library

Production-grade Circuit Breaker for protecting calls to unstable services.

- **Zero dependencies** — standard library only
- **97.1% test coverage**, passes `go test -race` cleanly
- **Generics-first** — type-safe `Execute[T]` wrapper (Go 1.18+)
- **O(1) ring buffer** — no re-scanning on every operation
- **Registry** — per-endpoint breakers with thread-safe creation
- **35 tests** across state transitions, fallback, concurrency, generics
- Includes a live HTTP demo service with configurable failure rates

---

### [job-market-analysis](https://github.com/nadzyathedude/job-market-analysis) — Claude Code skill

A Claude Code agent skill that interviews the user about their job search, scrapes 10–15 vacancies across LinkedIn, HH.ru, Indeed, Glassdoor (region-aware), and fills a Google Sheets table — including a "what does this role actually need" column that goes beyond the job description.

Uses Claude in Chrome MCP for JavaScript-heavy pages. Install with:
```bash
claude skills install https://github.com/nadzyathedude/job-market-analysis
```

---

### [qwen-assistant](https://github.com/nadzyathedude/qwen-assistant) — Local LLM on VPS

Self-hosted Qwen model on a personal VPS. Zero API costs, full control over inference.
# Nadezhda Chelyadinova — AI / LLM Engineer

I build production LLM systems: inference pipelines with cost controls, multi-agent orchestration, RAG from scratch, and tooling that ships.

---

## What I work with

**LLM layer** — fine-tuning, confidence-based routing, multi-stage inference, embedding-gate classifiers, evals  
**Agent systems** — LangGraph orchestration, sub-agent patterns, fan-out/fan-in, durable workflows (Temporal)  
**RAG** — custom pipelines without LangChain/LlamaIndex, hybrid retrieval, citation enforcement, incremental sync  
**MCP** — custom MCP servers, Claude Code skills, browser automation  
**Infrastructure** — Redis Streams, Qdrant, FastAPI, Docker, Kubernetes, Prometheus/Grafana  
**Languages** — Python, Go, Swift, Kotlin

---

## Projects

### [sentiment-finetune](https://github.com/nadzyathedude/sentiment-finetune) — Production LLM pipeline on one task
> Full stack of production techniques applied to a single problem (sentiment classification of product reviews)

Five layers that compose into a production-grade inference pyramid:

| Layer | What it does | Result |
|---|---|---|
| **Fine-tuning** | 50-example JSONL dataset → validate → upload → job → poll | Pipeline complete, artifacts ready |
| **Evals** | `confidence_check.py`: self-scoring + redundancy + constraint checks | **15 OK / 2 UNSURE / 0 FAIL, accuracy 79%** |
| **Routing** | `router.py`: gpt-4o-mini → gpt-4o on low confidence | **16 cheap / 1 expensive, 75% cost savings** |
| **Multi-stage** | `multistage.py`: monolithic → 3 stages with strict formats + rule-based aggregator | Unit-tested aggregator |
| **Micro-model first** | `micro_model.py`: embedding centroid classifier as LLM gatekeeper | Cheapest layer, O(1) lookup |

The full pyramid: `embedding-gate → fine-tuned gpt-4o-mini + confidence check → gpt-4o`  


---

### [ci-hub](https://github.com/nadzyathedude/ci-hub) — Multi-agent competitive intelligence system

Production-grade system that monitors competitors via web scraping, news, pricing, and generates weekly digest reports.

**Architecture decisions that matter:**
- **LangGraph** — conditional routing, fan-out/fan-in, subgraph retry loops
- **Sub-agent pattern** — stateless composable units with semaphore-controlled concurrency
- **Two-level parallelism** — top-level fan-out (3 agents) × sub-agent fan-out (N items each)
- **Redis Streams** over Pub/Sub — durability, consumer groups, replay
- **Temporal** — durable scheduling, retries, `continue_as_new` for long-running workflows
- **Qdrant hybrid search** — dense + BM25 sparse with RRF fusion

6 agent groups (WebWatcher, News, Pricing, Analyst, Alerts, Digest), FastAPI + WebSocket for real-time progress, 100+ unit tests with all LLM calls mocked, Kubernetes manifests included.

---

### [GestureFlow](https://github.com/nadzyathedude/GestureFlow) — Touchless AI workstation

Xbox Kinect 360 → MediaPipe (21 hand landmarks) → CoreML on Neural Engine (~4ms) → 6 LangGraph agents → your Mac obeys, the smart plug reacts, the Cardputer updates. Total latency: **~59ms**.

**What makes it interesting:**
- **CoreML on Neural Engine** — gesture classification in ~4ms; sklearn model converted to `.mlmodel` for on-device inference
- **6 LangGraph agents** — GestureAgent, MemoryAgent (bigram pattern detection), CommandAgent (pyautogui HID), NarratorAgent (Claude Sonnet + TTS), MatterAgent (Yandex smart plug via Matter over WiFi), LearningAgent
- **Online retraining** — every 50 high-confidence gestures, LearningAgent grows the RandomForest and redeploys the CoreML model in-flight; force retrain anytime by pressing `R` on the Cardputer
- **Multi-Fabric Matter** — Kinect controls the plug in parallel with Alisa; no factory reset required
- **M5Stack Cardputer** — MicroPython + MQTT physical remote with live LCD status; key presses bypass LangGraph for instant response

Stack: Python, LangGraph, MediaPipe, CoreML, libfreenect, Claude Sonnet, python-matter-server, Mosquitto MQTT, MicroPython

---

### [ai-personal-cognitive-operating-system](https://github.com/nadzyathedude/ai-personal-cognitive-operating-system) — AI wellbeing platform

A full-stack AI system that continuously monitors emotional state through voice, text, and physiology (BLE heart rate), predicts burnout 7 days ahead, and autonomously adapts coaching strategies and calendar scheduling.

**Core technical bets:**
- **Multimodal stress index** — acoustic prosody via wav2vec2 (40+ voice features) + text sentiment via DistilRoBERTa + BLE HRV/HR → composite score; detects when someone says "I'm fine" but their voice says otherwise
- **On-device Thompson Sampling** — multi-armed bandit learns which coaching strategies work for this user without data leaving the device; policy persisted in EncryptedSharedPreferences
- **Burnout prediction** — LightGBM on 14-day temporal features with 7-day projection. Not reactive ("you're stressed") but predictive ("at current trajectory, critical point in 5 days")
- **Stress-aware calendar** — reads Google Calendar events, scores each for stress risk, auto-inserts recovery blocks and reschedules non-critical tasks when stress is elevated
- **Voice biometrics** — ECAPA-TDNN speaker verification + anti-spoof detection (replay and synthetic voice attacks)
- **Wake word** — always-on Picovoice Porcupine foreground service with exclusive mic handoff

Architecture: Kotlin Multiplatform shared module (Android + iOS stubs) → Node.js Fastify/WebSocket server (GPT-4 streaming, tool calling, JWT) → Python FastAPI ML backend (SpeechBrain, HuggingFace Transformers, librosa, SQLAlchemy). Fully async, horizontally scalable.

---

### [notion_mcp_with_rag_memory](https://github.com/nadzyathedude/notion_mcp_with_rag_memory) — MCP server + RAG from scratch

Custom RAG pipeline built without LangChain or LlamaIndex — every component written by hand:

- **Chunker** — fixed-size with overlap, deterministic SHA-256 chunk IDs
- **Retriever** — brute-force cosine similarity, no numpy/FAISS
- **Reranker** — LLM-based rescoring (score 0–1), threshold filtering with fallback
- **Citations** — regex extraction → validation against allowed set → auto-retry on failure
- **Incremental sync** — `last_edited_time` diff, only re-indexes changed Notion pages

3 MCP tools: `sync_notion` / `search_notion_memory` / `ask_notion`  
Tested on 10 Notion pages → 469 chunks indexed. Search "ООП" → top score 0.65, correct page.

---

### [awasameCircuitBreaker](https://github.com/nadzyathedude/awasameCircuitBreaker) — Go Circuit Breaker library

Production-grade Circuit Breaker for protecting calls to unstable services.

- **Zero dependencies** — standard library only
- **97.1% test coverage**, passes `go test -race` cleanly
- **Generics-first** — type-safe `Execute[T]` wrapper (Go 1.18+)
- **O(1) ring buffer** — no re-scanning on every operation
- **Registry** — per-endpoint breakers with thread-safe creation
- **35 tests** across state transitions, fallback, concurrency, generics
- Includes a live HTTP demo service with configurable failure rates

---

### [job-market-analysis](https://github.com/nadzyathedude/job-market-analysis) — Claude Code skill

A Claude Code agent skill that interviews the user about their job search, scrapes 10–15 vacancies across LinkedIn, HH.ru, Indeed, Glassdoor (region-aware), and fills a Google Sheets table — including a "what does this role actually need" column that goes beyond the job description.

Uses Claude in Chrome MCP for JavaScript-heavy pages. Install with:
```bash
claude skills install https://github.com/nadzyathedude/job-market-analysis
```

---

### [voice-recipe-assistant](https://github.com/nadzyathedude/voice-recipe-assistant) — Voice-to-Notion recipe pipeline

Personal Android tool: record a voice description of a recipe → structured data → Notion, in three steps.

**Pipeline:** audio upload → OpenAI Whisper (STT) → GPT-4o Assistants API extracts title, ingredients, steps, time, temperature, notes → formatted preview card → one-tap save to Notion with full metadata.

Stack: Kotlin, Jetpack Compose, Material3, Retrofit / Python 3.12, FastAPI, Pydantic, async. Single-user personal tool — no auth, no multi-tenant logic.

---

### [qwen-assistant](https://github.com/nadzyathedude/qwen-assistant) — Local LLM on VPS

Self-hosted Qwen model on a personal VPS. Zero API costs, full control over inference.

---

## Contact

[github.com/nadzyathedude](https://github.com/nadzyathedude) · nadzyathedude@yandex.ru . tg - @nadezhda_chelyadinova
