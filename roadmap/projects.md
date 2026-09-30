# All Projects — Small, Big, and Capstones

**Sizes:** 🟢 Small (a few hours to 2 days) · 🟡 Medium (3–7 days) · 🔴 Big (2–4 weeks)

Every project is designed to force you to use the concepts of its part. AI flavor increases as you go.

---

## Part 1 — Python Basics

| # | Project | Size | Concepts |
|---|---|---|---|
| 1.1 | Token & cost calculator for LLM models | 🟢 | input, math, dicts, f-strings |
| 1.2 | Prompt template filler (template + JSON → final prompt) | 🟢 | strings, files, JSON |
| 1.3 | Chat log analyzer (stats from a chat export) | 🟢 | dicts, `Counter`, sorting |
| 1.4 | ELIZA-style rule-based chatbot | 🟢 | `match`, regex, functions |
| 1.5 | Training data cleaner (CSV → clean JSONL for fine-tuning) | 🟡 | csv, json, sets, pathlib |
| **SB v1** | **Second Brain: keyword search over your notes (TF-IDF)** | 🔴 | classes, dataclasses, files, errors |

## Part 2 — Pythonic Python

| # | Project | Size | Concepts |
|---|---|---|---|
| 2.1 | Resilient API toolkit (`@retry`, `@timeout`, `@log_calls`) | 🟢 | closures, decorators |
| 2.2 | Streaming "typewriter" + token-counting pipeline | 🟢 | generators, itertools |
| 2.3 | `Vector` class with cosine similarity | 🟢 | magic methods |
| 2.4 | Conversation memory with token budget | 🟡 | deque, dataclasses, tests |
| 2.5 | Add tests, Ruff, and type checking to all Part 1 projects | 🟡 | pytest, mypy/pyright |
| **promptkit** | **Installable prompt-management library, published to TestPyPI** | 🔴 | packaging, Pydantic, CLI, testing |

## Part 3 — Data & AI Foundations

| # | Project | Size | Concepts |
|---|---|---|---|
| 3.1 | Similarity search from scratch with NumPy | 🟢 | vectors, vectorization |
| 3.2 | Hugging Face dataset explorer with Polars + plots | 🟡 | DataFrames, EDA |
| 3.3 | Spam/sentiment classifier with scikit-learn | 🟡 | ML basics, metrics |
| 3.4 | Terminal chat app with streaming + cost counter | 🟡 | LLM APIs, streaming |
| 3.5 | Structured extractor (resume/invoice → Pydantic) | 🟡 | structured output, validation |
| 3.6 | LLM vs classic ML comparison report | 🟢 | evaluation |
| **SB v2** | **Second Brain: semantic + hybrid search, first RAG, recall@5 report** | 🔴 | embeddings, vector DB, evaluation |

## Part 4 — Concurrency, Backends & APIs

| # | Project | Size | Concepts |
|---|---|---|---|
| 4.1 | Concurrency showdown (loop vs threads vs processes vs async) | 🟢 | GIL, concurrency models |
| 4.2 | Batch LLM runner (500 prompts, limit 10, retries, progress) | 🟡 | asyncio, TaskGroup, Semaphore |
| 4.3 | Streaming summarize API (FastAPI + SSE + tiny web page) | 🟡 | FastAPI, async generators |
| 4.4 | Semantic cache for LLM calls in Redis | 🟡 | Redis, embeddings |
| 4.5 | PDF ingestion worker → pgvector | 🟡 | task queues, Postgres |
| **SB v3** | **Second Brain: RAG API service (auth, DB, jobs, streaming chat, Docker, deployed)** | 🔴 | full backend |

## Part 5 — AI Engineering

| # | Project | Size | Concepts |
|---|---|---|---|
| 5.1 | micrograd: your own autograd engine | 🟡 | backprop |
| 5.2 | MNIST digit recognizer in PyTorch | 🟢 | training loop |
| 5.3 | BPE tokenizer from scratch | 🟡 | tokenization |
| 5.4 | Tiny GPT trained on your own text | 🔴 | transformers, attention |
| 5.5 | LoRA fine-tune of a small open model, with before/after eval | 🟡 | fine-tuning |
| 5.6 | Agent from scratch (no framework) with 3 tools | 🟡 | tool calling, safety |
| 5.7 | MCP server for your notes | 🟢 | MCP |
| 5.8 | Eval harness with HTML report and run comparison | 🟡 | AI testing |
| **SB v4** | **Second Brain: AI research assistant (advanced RAG, agent, MCP, evals, tracing)** | 🔴 | AI engineering end-to-end |

## Part 6 — Under the Hood & Performance

| # | Project | Size | Concepts |
|---|---|---|---|
| 6.1 | Bug museum: 10 classic Python surprises, explained | 🟢 | object model |
| 6.2 | Rebuild `property`, `classmethod`, `staticmethod`, `cache` | 🟡 | descriptors |
| 6.3 | Mini ORM with descriptors and `__init_subclass__` | 🔴 | metaprogramming, SQL |
| 6.4 | AST code analyzer with auto-fix | 🟡 | `ast`, tooling |
| 6.5 | Memory leak hunt + write-up | 🟢 | memray, tracemalloc |
| 6.6 | Free-threading experiment (3.14 vs 3.14t) | 🟢 | GIL, free-threading |
| 6.7 | Rust BPE tokenizer with PyO3, benchmarked | 🟡 | PyO3, maturin |
| **SB v5** | **Second Brain: 10x performance pass with evidence + published Rust wheel** | 🔴 | profiling, optimization |

## Part 7 — Production & Career

| # | Project | Size | Concepts |
|---|---|---|---|
| 7.1 | Add Hypothesis tests + CI evals to Second Brain | 🟡 | testing |
| 7.2 | Security review of your own project (with fixes) | 🟢 | security, prompt injection |
| 7.3 | Full observability: logs, metrics, traces, Sentry | 🟡 | operations |
| 7.4 | 5 concurrency interview problems, fully tested | 🟡 | asyncio, threading |
| **SB v6** | **Second Brain: production-grade — monitored, secured, CI/CD, public demo** | 🔴 | everything |

---

## Capstones

Pick **one or two** at the end (week 62+). Each one should be production-quality: tests, types, CI, docs, deployed, with a write-up.

### C1 — LLM Gateway 🔴
One API in front of many LLM providers. Handles routing (cheap model for easy tasks), fallbacks when a provider fails, caching, per-team rate limits and budgets, cost dashboards, and tracing.
**Shows:** async at scale, reliability, system design. Companies build this internally all the time.

### C2 — Multi-Tenant "Chat with Your Docs" SaaS 🔴
Organizations sign up, upload documents, and chat with them. Strict data isolation between tenants, role-based access, billing by usage, background ingestion, evals per tenant.
**Shows:** full-stack backend + AI + security thinking.

### C3 — Train → Serve → Evaluate Pipeline 🔴
Fine-tune a small open model for a specific task, serve it with vLLM behind an API, load-test it, and compare it (quality, speed, cost) against a big API model.
**Shows:** the ML side of AI engineering, GPUs, serving performance.

### C4 — AI Code Review Bot 🔴
A GitHub app that reviews Python pull requests: uses `ast`/`libcst` for real code analysis plus an LLM for explanations; posts comments; measured against a set of known bugs.
**Shows:** Python internals + tooling + AI, a rare combination.

### C5 — Your Own Open-Source Library 🔴
Turn one of your projects (promptkit, eval harness, Rust tokenizer, semantic cache) into a real library: docs site, versioned releases, CI wheels, and actual users.
**Shows:** ownership, API design, community. The strongest long-term signal.

### C6 — Agentic Data Analyst 🔴
Ask questions in plain English about a dataset; the agent writes and runs Polars/SQL code in a **sandbox**, makes charts, and explains the results, with evals on a question set.
**Shows:** agents, safe code execution, data skills.
