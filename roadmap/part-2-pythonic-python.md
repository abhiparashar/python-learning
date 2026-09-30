# Part 2 — Writing Real, "Pythonic" Python (Weeks 7–13)

**Goal:** Write code that an experienced Python developer would call clean and idiomatic, using the tools real teams use (tests, linters, type checkers, packaging).

**"Pythonic"** means using the language's own style and features, instead of forcing another language's patterns onto it. For example, a short comprehension instead of a 6-line loop, or a `with` block instead of manual cleanup.

---

## Week 7 — Comprehensions and idioms

**Topics**
- List, dict, and set comprehensions: `[x * 2 for x in nums if x > 0]`.
- When a comprehension is too clever and a normal loop is clearer.
- Unpacking: `a, b = b, a`, `first, *rest = items`, `for key, value in d.items()`.
- `any()`, `all()`, `sum()`, `min()`/`max()` with `key=`.
- `sorted()` vs `.sort()`, sorting by several keys.
- The walrus operator `:=` (assign and use in one step).
- `import this`: *The Zen of Python*, the style philosophy behind all of this.

## Week 8 — Iterators and generators

**Topics**
- What `for` really does behind the scenes (it asks for an **iterator** and calls `next()` until it runs out).
- **Generators**: functions that use `yield` to produce values *one at a time*, only when asked. They use almost no memory, even for millions of items.
- Generator expressions: `(x * 2 for x in nums)`.
- Building data **pipelines**: read → clean → filter → batch, each step a generator.
- `itertools`: `chain`, `islice`, `groupby`, `batched` `[3.12+]`, `pairwise`, `product`.
- **AI link:** when ChatGPT "types" its answer word by word, that's streaming. In Python, you consume streamed LLM output with iterators/generators.

## Week 9 — Functions as tools: closures and decorators

**Topics**
- **Closures**: a function that remembers variables from the place it was created.
- The late-binding trap in closures (functions made in a loop all seeing the last value).
- **Decorators**: a function that wraps another function to add behavior (`@timer`, `@retry`, `@cache`). Like Java annotations, except they actually run code.
- Writing decorators, decorators with arguments, stacking them.
- `functools.wraps` (keeps the original function's name and docs), `functools.cache`, `lru_cache`, `partial`.
- **AI link:** almost every LLM app wraps API calls in retry/timeout/logging decorators.

## Week 10 — Context managers and "magic methods"

**Topics**
- Context managers (`with`): how they work (`__enter__` / `__exit__`), writing your own, `contextlib.contextmanager`.
- **Magic (dunder) methods**: special methods with double underscores that let your objects work with Python syntax:
  - `__len__` → `len(obj)`, `__getitem__` → `obj[i]`, `__iter__` → `for x in obj`
  - `__eq__` and `__hash__` → comparisons, and use as dict keys / in sets
  - `__add__`, `__mul__` → `+` and `*` (operator overloading, which Java doesn't have)
  - `__call__` → call an object like a function
- The rule: if you define `__eq__`, think about `__hash__` too.

## Week 11 — Type hints, properly

**Topics**
- Built-in generics: `list[str]`, `dict[str, int]`, `X | None` (instead of `Optional[X]`).
- `TypedDict` (typed dictionaries), `Literal["gpt", "claude"]`, `Final`.
- **`Protocol`**: Python's version of a Java interface, except classes don't need to say `implements`. If the methods match, it fits.
- Generics: `TypeVar`, and the new simpler syntax `def first[T](items: list[T]) -> T:` `[3.12+]`.
- Running a **type checker**: `mypy` or `pyright` (checks your hints without running the code, catching bugs early, much like `javac`).
- **Pydantic**: uses type hints to *validate data at runtime* (e.g. checking that an LLM's JSON reply has the right fields). Used everywhere in AI and backend Python.

## Week 12 — Professional tooling

**Topics**
- `uv` projects in depth: `pyproject.toml` (like `pom.xml`), dependencies, lock files, scripts.
- **Ruff**: one very fast tool for linting (finding bad patterns) and formatting (auto-styling your code).
- **pytest**: writing tests, `assert`, fixtures (reusable setup), `parametrize` (same test, many inputs), testing errors with `pytest.raises`.
- Debugging: `breakpoint()`, the VS Code debugger, reading tracebacks (Python's stack traces) bottom-up.
- `logging` module basics (instead of `print` everywhere).
- `pre-commit`: run Ruff and tests automatically before each Git commit.

## Week 13 — Useful standard library + design, plus project week

**Topics**
- `collections`: `Counter`, `defaultdict`, `deque` (fast at both ends), `namedtuple`.
- `enum.Enum` / `StrEnum`.
- `dataclasses` in depth: `frozen=True` (unchangeable), `slots=True`, `field(default_factory=list)`, `__post_init__`.
- `abc` (abstract base classes), and when to use them vs `Protocol`.
- Design: prefer composition over inheritance; why many Java design patterns become a single function or a module in Python.

---

## Small projects (AI-flavored)

| # | Project | What you'll practice |
|---|---|---|
| 2.1 | **Resilient API toolkit**: `@retry(times=3, backoff=2)`, `@timeout`, `@log_calls` decorators, used on a fake flaky "LLM API" function | closures, decorators, `functools.wraps` |
| 2.2 | **Streaming typewriter**: a generator that yields words from a response with small delays, like ChatGPT streaming, plus a pipeline that counts tokens as they arrive | generators, pipelines, `itertools` |
| 2.3 | **Vector class**: `Vector` supporting `+`, `*`, `len()`, `==`, indexing, `dot()`, and `cosine_similarity()`. This is the math behind AI embeddings | magic methods |
| 2.4 | **Conversation memory**: keeps only the last N messages or the last X tokens of a chat, like real chatbots do to stay within the model's limit | `deque`, dataclasses, tests |
| 2.5 | **Test everything**: add a pytest suite to all Part 1 projects, plus Ruff and type checking | pytest, fixtures, parametrize, mypy |

## Big project — `promptkit`: a real, installable Python package

A small library for managing LLM prompts, built like a professional open-source package.

- Prompt templates with **typed variables** (Pydantic validates them before rendering).
- Prompt **versioning**: `summarize@v1`, `summarize@v2`, stored as files.
- A CLI: `promptkit render summarize@v2 --var text=...`.
- Full test suite (>90% coverage), Ruff, strict type checking, `pre-commit`.
- Proper `pyproject.toml`, README, and published to **TestPyPI** (the practice version of the Python package store).

**Why:** Building one package the right way teaches more about real Python than 20 scripts.

---

## ✅ Checkpoint — you should be able to…

- [ ] Turn a 6-line loop into a clear comprehension, and know when not to.
- [ ] Explain the difference between a list and a generator in memory use.
- [ ] Write a decorator with arguments, from memory.
- [ ] Make your own class work with `len()`, `for`, `==`, `+`, and as a dict key.
- [ ] Add type hints and pass `mypy --strict` (or `pyright` strict) on a small project.
- [ ] Set up a new project with uv + Ruff + pytest in under 5 minutes.
- [ ] Explain why Python usually doesn't need Singleton, Factory, or Strategy classes.

## ⚠️ Common mistakes at this stage

- Nested comprehensions nobody can read.
- Forgetting `functools.wraps` in decorators, which breaks debugging and docs.
- Defining `__eq__` without thinking about `__hash__`.
- Adding type hints but never running a type checker, so the hints silently rot.
- Using `lru_cache` on methods (it keeps objects alive forever, a memory leak).
- Writing tests that only check "it didn't crash."

## 📚 Resources

- **Book (the most important Python book):** *Fluent Python*, 2nd ed. (Luciano Ramalho). Read chapters 1–3, 5, 7–9, 17 in this part. You'll return to it in Part 6.
- **Book:** *Effective Python*, 3rd ed. (Brett Slatkin). 125 short "do this, not that" items.
- **Talk:** Raymond Hettinger, *"Transforming Code into Beautiful, Idiomatic Python"*.
- **Talk:** Raymond Hettinger, *"Beyond PEP 8"*.
- **Talk:** James Powell, *"So you want to be a Python expert?"* (decorators, generators, context managers).
- **Docs:** pytest docs "How-to guides"; Pydantic docs "Concepts"; Ruff docs.
