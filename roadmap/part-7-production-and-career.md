# Part 7 — Production, Interviews & Career (Weeks 50–62)

**Goal:** Ship reliable software, pass interviews at top companies and AI startups, and start building a public reputation.

---

## Week 50 — Testing like a senior engineer

**Topics**
- pytest in depth: fixture scopes, factories, `conftest.py` structure, markers, plugins.
- Mocking correctly: `unittest.mock.patch` ("patch where it's *used*, not where it's defined"), `autospec`, and why too much mocking makes tests useless.
- **Hypothesis** (property-based testing): describe the *rules* your code must follow and let the tool generate hundreds of tricky inputs automatically.
- Testing with real services: `testcontainers` (spins up a real Postgres for tests).
- **Testing AI code**: output changes every run, so test structure, rules, and eval scores instead of exact text; record/replay API calls (`vcrpy`, `respx`).
- Coverage (and why 100% is a trap), running tests in parallel with `pytest-xdist`.
- CI with GitHub Actions: tests, Ruff, type checks, and evals on every pull request.

## Week 51 — Security

**Topics**
- Secrets management; never logging keys or personal data.
- Classic risks in Python: SQL injection, `pickle` (loading a pickle file can run any code), `eval`/`exec`, `yaml.load`, path traversal, SSRF (tricking your server into calling internal URLs).
- **Model files are code too**: loading untrusted model files can be dangerous; prefer the `safetensors` format.
- **AI-specific**: prompt injection, data leaks through RAG (users seeing documents they shouldn't), tool permission limits, OWASP Top 10 for LLM applications.
- Supply chain: lock files (`uv.lock`), `pip-audit`, typosquatting (fake packages with names close to real ones), trusted publishing to PyPI.

## Week 52 — Observability and operations

**Topics**
- The `logging` module properly: loggers, handlers, levels, `dictConfig`; `structlog` for JSON logs.
- Metrics (Prometheus / OpenTelemetry): request rate, errors, latency (the "RED" method); token usage and cost for AI.
- Distributed tracing with OpenTelemetry across API → queue → worker → LLM.
- Error tracking with Sentry.
- Debugging live production: `py-spy dump` (see what a stuck process is doing right now), `faulthandler`.
- Graceful shutdown, health checks, autoscaling basics.

## Week 53 — System design

**Topics**
- The building blocks: load balancers, caches, queues, databases (SQL vs NoSQL), replication, sharding, CAP trade-offs, idempotency, rate limiting.
- Back-of-the-envelope math: requests per second, storage, bandwidth, GPU capacity.
- How Python shapes designs: worker models (Gunicorn/Uvicorn processes), the GIL, where to move work to Rust/C, serialization costs.
- **Classic problems:** URL shortener, rate limiter, news feed, chat system, notification system, distributed cache, web crawler, payment system.
- **AI system problems:** LLM gateway (routing, caching, rate limits, fallbacks, cost tracking), RAG for 10M documents, ChatGPT-like chat service, model-serving platform, AI eval platform, AI content moderation pipeline, semantic search for an e-commerce site, AI coding assistant backend.

## Weeks 54–57 — Data structures & algorithms in Python

**Topics — patterns (≈150 problems, NeetCode 150 is a good list)**
- Arrays & hashing, two pointers, sliding window, stack, binary search, linked lists, trees, tries, heaps, backtracking, graphs (BFS/DFS, topological sort, Dijkstra), intervals, greedy, 1-D and 2-D dynamic programming, bit manipulation.

**Python-specific interview skills**
- `collections.deque` for BFS (never `list.pop(0)`, which is slow).
- `heapq` is min-heap only: store `-value` for a max-heap; tuples compare element by element.
- `bisect` for binary search; `Counter` / `defaultdict` for counting; `@cache` for memoized recursion.
- Recursion limit (~1000 deep): know how to turn recursion into a loop with a stack.
- Hidden costs: `in` on a list is O(n), string `+=` in a loop, slicing makes copies.
- Writing clean, readable interview code: good names, helper functions, test it out loud.

## Week 58 — Python deep-knowledge interview questions

Prepare to answer, with *depth*, questions like:
- Mutable vs immutable; `is` vs `==`; how a dict works; list vs tuple internals.
- Mutable default arguments; late-binding closures.
- Decorators, generators, context managers: implement each on a whiteboard.
- The GIL, threads vs processes vs asyncio, and free-threading.
- MRO, `super()`, descriptors, metaclasses, `__new__` vs `__init__`.
- Memory management: reference counting + garbage collector.
- asyncio: event loop, what `await` really does, cancellation, TaskGroup.
- Typing: Protocol vs ABC, generics, what type hints do (and don't do) at runtime.

For each one, practice three levels: the **basic answer**, the **deep answer** (how it works inside), and the **follow-up** the interviewer will likely ask next.

**Hands-on concurrency problems:** rate limiter, worker pool with backpressure (slowing producers when consumers can't keep up), async retry with jitter, bounded parallel fetcher, producer/consumer with clean shutdown.

**AI-specific interviews:** explain transformers/attention, design a RAG system, debug a bad RAG answer, design evals, cut LLM cost by 5x, and "implement attention / a training loop / BPE from scratch" (you already have).

## Week 59 — Behavioral interviews and your story

**Topics**
- STAR stories (Situation, Task, Action, Result): 8–10 stories covering a hard bug, a performance win, a disagreement, a failure, leading without authority, mentoring.
- Turning your Java background into a strength: "I came from Java; here's what I learned deeply about Python that most people skip."
- Company notes: Amazon's Leadership Principles; Google's "Googleyness"; Meta's focus on speed and impact; AI labs' (Anthropic/OpenAI/etc.) focus on mission, safety, and depth.

## Weeks 60–61 — Portfolio and open source

**Portfolio**
- GitHub: 3–4 *excellent* repos (Second Brain, the PyO3 package, one capstone) beat 30 half-finished ones. Each needs a clear README, a demo, tests, and CI.
- Write 4–6 blog posts from your project write-ups (performance pass, RAG evaluation results, free-threading experiment).

**Open source: where to start**
- **typeshed** (type stubs): small, well-reviewed PRs; a great first step.
- **CPython**: docs and tests first; read the devguide; look for "easy" labeled issues.
- **AI/Python ecosystem**: Pydantic, FastAPI, httpx, Hugging Face (`transformers`, `datasets`), LlamaIndex, LangChain, Pydantic AI, vLLM, the MCP Python SDK.
- **Rust-based tools** (if you enjoyed Part 6): Ruff, uv, Polars.
- How: read CONTRIBUTING.md → reproduce a bug → small PR with a test → respond kindly to review → repeat.

## Week 62 — Final capstone

Pick one from [projects.md](projects.md#capstones) and ship it to production quality.

---

## Realistic milestones

| When | Where you should be |
|---|---|
| Month 3 | Comfortable, idiomatic Python; published first package; tests and types by habit |
| Month 6 | Build AI apps with LLM APIs, embeddings, and semantic search; strong data skills |
| Month 9 | Ship async FastAPI services with databases; Second Brain v3 deployed |
| Month 12 | Understand neural nets and transformers from scratch; build RAG and agents with evals |
| Month 15 | Deep internals knowledge; performance work with evidence; a Rust extension; interview-ready |
| Month 18+ | Open-source contributions merged; writing publicly; a specialty forming |

## Being honest about "top 1%"

- This roadmap can take you to **strong senior-level technical skill** and **ready for top-company interviews**. That already puts you ahead of most Python developers.
- The last step to top 1% comes from things only time and real work give you: running systems with real users, handling real failures, delivering business impact, being recognized in open source, and going very deep in one specialty (e.g. LLM inference performance, AI evaluation, or Python tooling).
- **The biggest opportunity:** many "senior Python engineers" never learned the internals, and many "AI engineers" can't write solid Python or explain what a model does. Being strong in *both* is rare.
- **Plateau warning signs:** only following tutorials, only using frameworks, never measuring, never reading source code, never shipping. Fix: build something real, measure it, read the code of the tools you use, and teach it to someone.

---

## ✅ Final checkpoint

- [ ] A tested, typed, deployed, monitored service in production (Second Brain v6).
- [ ] 150+ DSA problems solved in clean Python; can explain complexity.
- [ ] Can answer the deep Python questions at all three levels.
- [ ] Can design a classic system and an AI system on a whiteboard in 45 minutes.
- [ ] At least one merged open-source PR.
- [ ] 3+ public write-ups of your work.

## 📚 Resources

- **Book:** *Designing Data-Intensive Applications*, 2nd ed. (Kleppmann & Riccomini). The system design bible.
- **Book:** *System Design Interview* Vol. 1 & 2 (Alex Xu).
- **Book:** *Designing Machine Learning Systems* (Chip Huyen).
- **Book:** *Robust Python* (Patrick Viafore). Types and maintainable code at scale.
- **Book:** *Python Testing with pytest*, 2nd ed. (Brian Okken).
- **Practice:** NeetCode 150 / LeetCode; Hello Interview (system design); Exercism for Python idioms.
- **Docs:** OWASP Top 10 for LLM Applications; OpenTelemetry Python docs; CPython devguide "Getting started".
- **Podcasts:** *Talk Python To Me*, *Python Bytes*, *Latent Space* (AI engineering).
- **Newsletters/blogs:** Python Weekly, PyCoder's Weekly, Simon Willison's blog (practical LLM tools), Sebastian Raschka's *Ahead of AI*.
