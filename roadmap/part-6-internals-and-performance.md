# Part 6 — Under the Hood & Performance (Weeks 42–49)

**Goal:** Understand how Python *really* works inside, and make slow Python fast with evidence.

**Why this is the "top 1%" part:** Most Python developers, including many senior ones, never learn this. Python is easy to be productive in without understanding it. That is exactly the gap you will close. It also makes AI work better: PyTorch, tokenizers, Pydantic, and Polars are all "Python on top, C/Rust underneath."

We use **CPython** here: the standard Python you download from python.org, written in C.

---

## Week 42 — The object model

**Topics**
- Everything is an object: numbers, functions, classes, even modules. Every object has an identity (`id`), a type, and a value.
- Names vs objects (revisited in depth): assignment never copies.
- `is` vs `==`, and why `a is b` sometimes "works" for small numbers and short strings (caching and interning) and then suddenly doesn't.
- Mutability and aliasing bugs; shallow vs deep copy.
- **Reference counting**: every object counts how many names point to it, and is freed when the count reaches 0.
- The **garbage collector**: cleans up objects that point to each other in a loop (reference cycles), which counting alone can't free.
- How `__eq__` and `__hash__` must agree.

## Week 43 — Attributes, descriptors, and metaclasses

**Topics**
- What really happens on `obj.name`: the full lookup order (the object's own `__dict__`, its class, parent classes in **MRO** order (Method Resolution Order), and `__getattr__` as the last fallback).
- **Descriptors**: the hidden mechanism behind `@property`, `@classmethod`, `@staticmethod`, and even normal methods. Rebuild each one yourself.
- **MRO** and `super()` with multiple inheritance.
- `__slots__`: saving memory on objects you create millions of.
- **Metaclasses**: classes that create classes. `__init_subclass__` and `__set_name__` as simpler alternatives.
- How Django models, SQLAlchemy models, and Pydantic models use these tricks to turn `name: str` into a validated field.

## Week 44 — How CPython executes code

**Topics**
- The full pipeline: source → tokens → **AST** (a tree version of your code) → bytecode → evaluation loop.
- Reading bytecode fluently with `dis`.
- The `ast` module: parse, inspect, and change Python code with Python.
- The **specializing adaptive interpreter** `[3.11+]`: CPython watches your running code and swaps in faster, specialized instructions. It's the main reason 3.11 got ~25% faster.
- The **JIT compiler** `[3.13+, experimental]`: what it does today and what it doesn't.
- The **GIL** in depth, and **free-threading** `[3.13 experimental → 3.14 officially supported, still optional]`: what changed inside CPython to make it possible, its costs, and why code that "worked with the GIL" can break without it.
- Subinterpreters (`concurrent.interpreters`) `[3.14+]`: several isolated Pythons in one process.
- `sys.monitoring` `[3.12+]`: how debuggers and profilers hook into running code.

## Week 45 — Inside the core data structures

**Topics**
- `dict`: a **hash table**. How lookup works, why keys must be hashable, why dicts keep insertion order, and what resizing costs.
- `list`: over-allocation (why `append` is fast but `insert(0, x)` is slow).
- `str`: how Python stores text compactly (1, 2, or 4 bytes per character depending on content).
- `int`: unlimited-size numbers, and their cost.
- `float`: why `0.1 + 0.2 != 0.3`, and what to do about it.
- `set`, `tuple`, `deque`, `heapq`, `bisect`.
- The Big-O cost of every common built-in operation (a classic interview topic).
- Memory: why `sys.getsizeof` is misleading, and the real memory cost of objects vs NumPy arrays.

## Week 46 — Measuring performance (profiling)

**Topics**
- Rule #1: **measure before optimizing.** Your guess about the slow part is usually wrong.
- `timeit` (and its traps), `cProfile` + `snakeviz`.
- Sampling profilers: **py-spy** (can attach to a *running* program), **Scalene** (CPU + memory + GPU), **pyinstrument**.
- Memory profilers: **memray**, `tracemalloc`. Hunting memory leaks.
- Reading **flame graphs** (a picture showing where time goes).
- Benchmarking fairly: warm-up, repeats, noise, `pytest-benchmark`.

## Week 47 — Making Python fast

**Topics**
- Order of attack: 1) better algorithm/data structure → 2) do less work (cache, batch) → 3) push loops into C (built-ins, NumPy) → 4) compile the hot part (Numba, Cython, Rust).
- Pure-Python speedups: local variables, avoiding repeated attribute lookups, built-ins over hand-written loops, `join` for strings.
- **Numba**: add `@njit` to a numeric function and get near-C speed.
- **Cython** (a glimpse): Python-like code compiled to C.
- Faster libraries: `orjson` / `msgspec` instead of `json`, `uvloop`, Polars instead of pandas.
- When micro-optimizing is a waste of time.

## Week 48 — Rust extensions with PyO3

**Topics**
- Why Rust became the modern way to speed up Python: Pydantic's core, Polars, Ruff, uv, Hugging Face `tokenizers`, and `orjson` are all Rust underneath.
- **PyO3** (write Python modules in Rust) + **maturin** (build and package them).
- Passing data between Python and Rust cheaply; releasing the GIL for parallel Rust code.
- Building **wheels** (pre-compiled packages) for Mac, Linux, and Windows with GitHub Actions.
- A glimpse of the C API: reference ownership and why it's error-prone (which is exactly why Rust is winning).

## Week 49 — Reading real source code

**Topics**
- Build CPython from source on your Mac; run its tests.
- Read, in this order: `Lib/functools.py`, `Lib/collections/__init__.py`, `Lib/dataclasses.py` (pure Python and very readable), then `Objects/listobject.c` and `Objects/dictobject.c` (C, now approachable).
- Read a small real library end-to-end: Karpathy's `micrograd`, then `httpx` or parts of FastAPI/Starlette.
- The CPython developer guide: how contributions work.

---

## Small projects

| # | Project | What you'll practice |
|---|---|---|
| 6.1 | **Bug museum**: small scripts that show 10 classic Python surprises (aliasing, mutable defaults, late binding, `is` with integers, float math, etc.), each with an explanation | object model |
| 6.2 | **Rebuild the built-ins**: write your own `property`, `classmethod`, `staticmethod`, and `functools.cache` in pure Python | descriptors, closures |
| 6.3 | **Mini ORM**: `class User(Model): name = StringField()` turns into SQL tables and queries using descriptors and `__init_subclass__` | descriptors, metaclass alternatives |
| 6.4 | **Code analyzer**: an `ast`-based tool that flags mutable defaults, bare `except:`, and `print` calls in a codebase, with auto-fix | AST, tooling |
| 6.5 | **Leak hunt**: a deliberately leaky service; find the leak with memray/tracemalloc and write it up | memory profiling |
| 6.6 | **Free-threading experiment**: run a CPU-heavy task on normal 3.14 vs free-threaded 3.14 (`python3.14t`) with threads; measure and explain | GIL, free-threading |
| 6.7 | **Rust tokenizer**: rewrite your Part 5 BPE tokenizer's hot loop in Rust with PyO3; benchmark vs pure Python and vs `tiktoken` | PyO3, maturin, benchmarking |

## Big project — Second Brain v5: The 10x Performance Pass

- **Profile** ingestion and query paths with py-spy/Scalene; record the baseline numbers.
- Fix the biggest bottlenecks in order (algorithm, batching, caching, vectorizing, async).
- Move one proven hot spot to Rust (e.g. chunking or tokenizing) as a separate package, published with wheels.
- **Write-up** with before/after flame graphs and benchmark tables. Target: ≥10x faster ingestion and lower p95 query time (the time 95% of requests stay under).

**Why:** "I made X 10x faster, and here's the evidence" is one of the strongest signals you can show any employer.

---

## ✅ Checkpoint — you should be able to…

- [ ] Explain, step by step, what Python does when it runs `obj.attr`.
- [ ] Write a descriptor and explain how `@property` works internally.
- [ ] Read `dis` output for a small function and explain it.
- [ ] Explain the GIL, free-threading, and subinterpreters, and when each matters.
- [ ] Explain how a Python dict works as a hash table.
- [ ] Profile a real program, find the real bottleneck, and fix it with proof.
- [ ] Build and publish a Rust extension with PyO3.

## ⚠️ Common mistakes at this stage

- Optimizing before profiling.
- Micro-optimizing Python loops when the real fix is NumPy, batching, or a better algorithm.
- Using metaclasses where `__init_subclass__` or a decorator would do.
- Benchmarking once and trusting the number.
- Assuming thread-safe code is safe just because the GIL used to hide the bug.

## 📚 Resources

- **Book:** *Fluent Python*, 2nd ed.: finish it now (especially the object model, descriptors, and metaprogramming chapters).
- **Book:** *CPython Internals* (Anthony Shaw). Somewhat dated (written for 3.9) but the best guided tour; pair it with the current devguide.
- **Book:** *High Performance Python*, 3rd ed. (Gorelick & Ozsvald).
- **Docs:** CPython Developer Guide (devguide.python.org), especially the "Internals" section; the `dis` and `ast` docs.
- **Docs:** PyO3 user guide, maturin docs.
- **PEPs to read:** 8 (style), 20 (Zen), 484 (type hints), 659 (adaptive interpreter), 703 (free-threading), 779 (free-threading supported), 734 (subinterpreters).
- **Talks:** Brandon Rhodes, *"The Dictionary Even Mightier"*; Raymond Hettinger, *"Modern Python Dictionaries"*; Larry Hastings, *"Removing Python's GIL: The Gilectomy"* (history).
- **Talks:** David Beazley, *"Generators: The Final Frontier"* and *"Python Concurrency From the Ground Up"*.
