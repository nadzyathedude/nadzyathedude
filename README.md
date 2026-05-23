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

---

## Contact

[github.com/nadzyathedude](https://github.com/nadzyathedude) · nadzyathedude@yandex.ru . tg - @nadezhda_chelyadinova
