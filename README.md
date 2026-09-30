# Python Mastery Roadmap — From Java Developer to Top-Tier Python + AI Engineer

This folder is your full learning path. It starts at "I know Java, but I'm new to Python." It ends at "I understand Python deeply, I build real AI systems with it, and I can pass hard interviews."

Everything is written in simple words. When a technical word can't be avoided, it gets explained the first time it shows up.

---

## Who this is for

- You already program (Java). You know what loops, classes, and methods are.
- You are new to Python.
- You are moving into AI, so most projects here are AI-flavored.
- You want real mastery, not just "I can write a script."

---

## The 7 Parts

| Part | Name | Weeks | You will be able to… |
|---|---|---|---|
| 1 | [Python Basics for Java Developers](roadmap/part-1-python-basics.md) | 1–6 | Write normal Python programs with confidence |
| 2 | [Writing Real, "Pythonic" Python](roadmap/part-2-pythonic-python.md) | 7–13 | Write code that looks like a Python expert wrote it, with tests and tools |
| 3 | [Data & AI Foundations](roadmap/part-3-data-and-ai-foundations.md) | 14–21 | Work with data, call LLMs, use embeddings, train simple ML models |
| 4 | [Concurrency, Backends & APIs](roadmap/part-4-backend-and-async.md) | 22–29 | Build fast async web services that power AI apps |
| 5 | [AI Engineering](roadmap/part-5-ai-engineering.md) | 30–41 | Build neural nets from scratch, train a tiny GPT, fine-tune, build RAG and agents |
| 6 | [Under the Hood & Performance](roadmap/part-6-internals-and-performance.md) | 42–49 | Understand how Python really works inside, and make code 10x faster |
| 7 | [Production, Interviews & Career](roadmap/part-7-production-and-career.md) | 50–62 | Ship to production, pass MAANG/AI-startup interviews, contribute to open source |

All projects in one place: **[projects.md](roadmap/projects.md)**

---

## How long will this take?

| Pace | Hours per week | Total time |
|---|---|---|
| Full-time | 35–40 | ~9–12 months |
| Serious part-time | 15–20 | ~14–16 months (the week numbers above assume this pace) |
| Light part-time | 8–10 | ~24–30 months |

Going slower is fine. Skipping the projects is not. **Projects are where learning actually happens.**

---

## The "Thread Project": Second Brain

One project grows with you through the whole roadmap. It is a personal AI assistant for your own notes and documents.

| Version | Part | What it is |
|---|---|---|
| v1 | 1 | A command-line keyword search over your notes folder |
| v2 | 3 | Smart search using embeddings (it finds meaning, not just matching words) |
| v3 | 4 | A real web API with a database, background jobs, and streaming chat |
| v4 | 5 | An AI research agent with RAG, tools, evaluations, and tracing |
| v5 | 6 | Profiled and made 10x faster, with a Rust-powered piece |
| v6 | 7 | Deployed, monitored, tested, secured — a portfolio centerpiece |

Watching one codebase grow teaches you something single projects can't: how real software changes over time.

---

## How to study each week

1. **Learn** — read or watch the week's topics (1/3 of your time).
2. **Type** — write every example yourself. Don't copy and paste. Break it on purpose and see what happens.
3. **Build** — do the small project(s) for that week (the biggest share of your time).
4. **Explain** — write 5–10 lines in your own words in `notes/`. If you can't explain it simply, you don't know it yet.
5. **Review** — at the end of each Part, check every item in its "Checkpoint" list.

### Rules for using AI assistants while learning

- ✅ Ask AI to **explain** a concept, an error message, or someone else's code.
- ✅ Ask AI to **review** your code after you've written it.
- ❌ Don't ask AI to **write** your practice code. You'd skip the exact struggle that builds skill.
- In Parts 5–7, using AI coding tools for real work is fine. By then you'll know enough to judge what they produce.

---

## Python version

- Latest stable right now: **Python 3.14** (3.14.7).
- **Python 3.15** is due for release on **October 1, 2026**.
- **Use 3.14 while learning.** Move to 3.15 once your key libraries (PyTorch, etc.) officially support it, usually a few weeks to a few months after release.
- Anything version-specific is marked in the roadmap like this: `[3.12+]`, `[3.14+]`, `[experimental]`.

---

## Folder layout (the content will be added part by part)

```
python-learning/
├── README.md              ← you are here
├── roadmap/               ← the plan (what to learn, in what order)
│   ├── part-1-python-basics.md
│   ├── ...
│   └── projects.md
├── content/               ← lessons in simple words (we'll create these next)
│   ├── part-1/
│   └── ...
├── projects/              ← your project code
│   ├── small/
│   └── second-brain/
└── notes/                 ← your own notes, in your own words
```

---

## Progress tracker

- [ ] Part 1 — Python Basics for Java Developers
- [ ] Part 2 — Writing Real, Pythonic Python
- [ ] Part 3 — Data & AI Foundations
- [ ] Part 4 — Concurrency, Backends & APIs
- [ ] Part 5 — AI Engineering
- [ ] Part 6 — Under the Hood & Performance
- [ ] Part 7 — Production, Interviews & Career
- [ ] Final capstone shipped
- [ ] First open-source contribution merged

---

## An honest word about "top 1%"

This roadmap can realistically take you to **strong senior-level Python + AI engineering skill** and **interview-ready for top companies**. That already puts you ahead of most people who call themselves "Python developers." Many of them never learn how the language works inside.

Real top 1% also takes things no roadmap can give you: years of running real systems, fixing real outages, shipping work people use, and being known for something (open source, writing, a specialty). Part 7 explains how to start building those too.
