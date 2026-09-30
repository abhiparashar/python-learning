# Part 4 — Concurrency, Backends & APIs (Weeks 22–29)

**Goal:** Build fast, reliable web services in Python, the kind that power real AI products: many users at once, streaming answers, databases, background jobs, deployed in Docker.

**Java comparison:** Spring Boot → **FastAPI**. Hibernate/JPA → **SQLAlchemy**. `ExecutorService` → `concurrent.futures`. Project Reactor / virtual threads → **asyncio**.

---

## Week 22 — How Python runs your code (the simple version)

**Topics**
- Python first turns your code into **bytecode** (simple instructions), which the **interpreter** then runs one by one. Peek with the `dis` module.
- Why Python is slower than Java for raw loops, and why that often doesn't matter (most time is spent waiting on the network, database, or model).
- **The GIL** (Global Interpreter Lock): in standard Python, only one thread runs Python code at a time. Threads still help when *waiting* (network, disk), but not for heavy calculation.
- Three ways to do many things at once, as a restaurant analogy:
  - **Threads**: several waiters, but only one can write orders at a time (GIL). Good when they mostly wait.
  - **Processes**: several separate restaurants. True parallel work, but more memory and slower communication.
  - **asyncio**: one very efficient waiter who never stands idle while food cooks. Great for thousands of network calls.
- **Free-threaded Python** (no GIL): experimental in 3.13, officially supported but optional in 3.14 `[3.14+]`. Not the default yet. Know it exists; you'll go deeper in Part 6.

## Week 23 — Threads and processes

**Topics**
- `threading`, `ThreadPoolExecutor`, futures, `as_completed`.
- Locks and race conditions (two threads changing the same data at once), and why "it worked in testing" proves nothing.
- `multiprocessing`, `ProcessPoolExecutor`: using all CPU cores for heavy work.
- What can be sent between processes (it must be **picklable**, i.e. convertible to bytes).
- Benchmark: the same job with a plain loop vs threads vs processes. Learn *when* each wins.

## Week 24 — asyncio (the most important week in this part)

**Topics**
- `async def` and `await`: `await` means "pause me here and let other work run until this finishes."
- The **event loop**: the engine that switches between paused tasks.
- Tasks, `asyncio.gather`, and **`TaskGroup`** `[3.11+]` (the modern, safe way to run tasks together; if one fails, the others are cleanly cancelled).
- Timeouts with `asyncio.timeout()` `[3.11+]`.
- Limiting concurrency with `Semaphore` (e.g. "at most 10 LLM calls at once").
- Async HTTP with `httpx.AsyncClient`, async LLM SDK clients.
- **The #1 async bug**: calling slow blocking code (like `time.sleep` or `requests.get`) inside async code freezes *everything*. Fix: async libraries, or `asyncio.to_thread()`.
- Other common bugs: forgetting `await`, and background tasks silently disappearing.

## Week 25 — FastAPI

**Topics**
- Routes, path/query parameters, request and response bodies with Pydantic.
- Automatic API docs (OpenAPI / Swagger) at `/docs`.
- **Dependency injection** with `Depends()` (much lighter than Spring's).
- Async endpoints, and when to use a normal `def` endpoint.
- **Streaming responses** with Server-Sent Events (SSE), so a chat answer appears token by token in the browser.
- Error handling, middleware, CORS, the app lifespan (startup/shutdown).
- Servers: **Uvicorn** (runs FastAPI), plus how many workers to run.

## Week 26 — Databases

**Topics**
- SQL refresher (SELECT, JOIN, indexes, transactions).
- **PostgreSQL** (the default database choice), run locally with Docker.
- **SQLAlchemy 2.0**: models, sessions, queries, relationships, async sessions. Compare it to JPA.
- The **N+1 query problem** (1 query becomes 1,000 by accident) and how to fix it.
- **Alembic**: database migrations (like Flyway/Liquibase).
- **pgvector**: store embeddings inside Postgres. Often all the "vector DB" you need.
- **Redis**: caching and simple rate limiting.

## Week 27 — Background jobs, auth, and config

**Topics**
- Why slow work (embedding 500 documents) shouldn't run inside a web request.
- Task queues: **arq** (simple, async, uses Redis) or Celery (older, heavier, very common). Retries, idempotency (running a job twice does no harm).
- Authentication: API keys, password hashing, **JWT** tokens (and their common mistakes), OAuth login basics.
- Rate limiting per user.
- Config with `pydantic-settings` (validated settings loaded from environment variables).

## Week 28 — Shipping it

**Topics**
- **Docker**: writing a good Python Dockerfile (small, multi-stage, with `uv`), `docker compose` for app + Postgres + Redis.
- Testing APIs: FastAPI's `TestClient`, test databases, testing async code with `pytest-asyncio` or AnyIO.
- Structured logging (logs as JSON with a request ID).
- Health check endpoints, graceful shutdown.
- Deploying: Fly.io / Render / Railway, or a small cloud VM.

## Week 29 — Project week

---

## Small projects (AI-flavored)

| # | Project | What you'll practice |
|---|---|---|
| 4.1 | **Concurrency showdown**: download 200 web pages / call a fake API 200 times with a plain loop, threads, processes, and asyncio. Time each and explain the results | threads, processes, async |
| 4.2 | **Batch LLM runner**: send 500 prompts from a JSONL file with max 10 at once, retries on 429 errors, a progress bar, and results + cost saved to a file | asyncio, `Semaphore`, `TaskGroup` |
| 4.3 | **Streaming summarize API**: FastAPI endpoint that takes text and streams an LLM summary back over SSE, with a tiny HTML page showing it live | FastAPI, SSE, async generators |
| 4.4 | **Semantic cache**: before calling the LLM, check Redis for an *almost identical* past question (by embedding similarity), and return the cached answer | Redis, embeddings, caching |
| 4.5 | **Document ingestion worker**: upload a PDF → background job extracts text, chunks it, embeds it, and stores it in pgvector → a status endpoint shows progress | task queues, Postgres, pgvector |

## Big project — Second Brain v3: RAG API Service

Turn Second Brain into a real backend service.

- **FastAPI** app with user accounts (JWT auth) and API keys.
- **Postgres + pgvector** for documents, chunks, and embeddings. **Alembic** migrations.
- **Upload endpoint** → background ingestion job (4.5).
- **Chat endpoint** with streaming answers, citations to the source chunks, and saved conversation history.
- Per-user **rate limiting** and **cost tracking**.
- `docker compose up` starts everything. Tests cover auth, ingestion, and search.
- Deployed publicly with a simple web page to try it.

---

## ✅ Checkpoint — you should be able to…

- [ ] Explain the GIL, and pick threads vs processes vs asyncio for a given task, with a reason.
- [ ] Write async code with `TaskGroup`, timeouts, and a concurrency limit, without blocking the loop.
- [ ] Build a FastAPI service with auth, a database, migrations, and streaming.
- [ ] Spot and fix an N+1 query.
- [ ] Put a service in Docker and deploy it.

## ⚠️ Common mistakes at this stage

- Using `requests` or `time.sleep` inside async code.
- Running heavy CPU work (like local embedding models) inside the async event loop.
- Firing off `asyncio.create_task(...)` without keeping a reference (the task can vanish).
- No limit on concurrency, which floods the LLM API and gets you rate-limited or banned.
- Doing slow work inside the web request instead of a background job.
- Storing JWTs insecurely or never letting them expire.

## 📚 Resources

- **Book:** *Architecture Patterns with Python* (Percival & Gregory). Free online; read chapters 1–6 now.
- **Book:** *Using Asyncio in Python* (Caleb Hattingh).
- **Talk:** David Beazley, *"Python Concurrency From the Ground Up: LIVE!"* (PyCon 2015). Builds concurrency from scratch.
- **Talk:** Łukasz Langa's *"Import asyncio"* video series (EdgeDB YouTube channel).
- **Docs:** FastAPI tutorial (excellent), SQLAlchemy 2.0 Unified Tutorial, asyncio docs "High-level API".
- **Blog:** Nathaniel J. Smith, *"Notes on structured concurrency, or: Go statement considered harmful"*. It explains why `TaskGroup` exists.
