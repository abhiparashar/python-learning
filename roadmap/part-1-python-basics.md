# Part 1 — Python Basics for Java Developers (Weeks 1–6)

**Goal:** Write normal Python programs comfortably, and stop thinking "how would I do this in Java?"

**Why this part matters:** Java habits carry over in ways that make Python code long, clumsy, and sometimes wrong. This part rewires those habits early.

---

## Java → Python: the big differences (read this first)

| Idea | Java | Python |
|---|---|---|
| Blocks | `{ }` braces | Indentation (spaces) *is* the block |
| Types | Declared: `int x = 5;` | Not declared: `x = 5`. Types are checked while the program runs |
| Variables | A box that holds a value | A **name tag** stuck on an object. Two tags can point at the same object |
| Main method | `public static void main` | Just code at the top level. `if __name__ == "__main__":` for scripts |
| Everything in a class? | Yes | No. Plain functions are normal and preferred |
| Interfaces | `interface` + `implements` | "Duck typing": if it has the right methods, it works |
| Checked exceptions | Yes | No. Any function can raise anything |
| `null` | `null` | `None` |
| Integer size | `int` overflows at ~2 billion | `int` has no limit. `2 ** 1000` just works |
| Private | `private` keyword | Convention only: `_name` means "please don't touch" |
| Getters/setters | Everywhere | Almost never. Use plain attributes, `@property` if needed later |
| Build tools | Maven/Gradle | `uv` + `pyproject.toml` |

---

## Week 1 — Setup and first programs

**Topics**
- Install **uv** (a fast tool that installs Python, manages project libraries, and runs code).
- Install Python 3.14 with `uv python install 3.14`.
- Editor: VS Code + the Python extension (or PyCharm, which will feel familiar from IntelliJ).
- The **REPL** (the interactive `>>>` prompt): your playground for trying ideas in seconds.
- **Virtual environments**: a private box of libraries for each project, so projects don't fight over versions. `uv` creates them for you.
- Running a script: `uv run main.py`.
- Basic syntax: indentation, comments, `print()`, `input()`.
- Numbers: `int` (unlimited size), `float`, `/` (always gives a float) vs `//` (whole-number division), `%`, `**` (power).
- `bool` (`True`/`False`, capitalized), `None`.
- **f-strings**: `f"Hello {name}, you are {age} years old"`, Python's main way to build strings.

**Practice:** Rewrite 5 small Java programs you know (FizzBuzz, temperature converter, etc.) in Python.

---

## Week 2 — Strings and the four core collections

**Topics**
- Strings: they can't be changed after creation (immutable, like Java). Useful methods: `split`, `join`, `strip`, `replace`, `lower`, `startswith`, `find`.
- **Slicing**: `text[0:5]`, `text[-3:]`, `text[::-1]` (reversed). Works on lists too.
- `list`: like `ArrayList` but simpler: `[1, 2, 3]`, `append`, `pop`, `sort`, `in`.
- `tuple`: a list you can't change: `(1, 2)`. Used for fixed groups of values.
- `dict`: like `HashMap`: `{"name": "Ana", "age": 30}`. Keeps insertion order. `.get()`, `.items()`, `.keys()`.
- `set`: like `HashSet`: `{1, 2, 3}`, fast "is this in here?" checks, union/intersection.
- **Mutable vs immutable**: which objects can change in place (list, dict, set) and which can't (int, str, tuple). This drives many bugs.

**Watch out:** `a = [1, 2]; b = a; b.append(3)` changes `a` too. Both names point at the same list.

---

## Week 3 — Control flow, the Python way

**Topics**
- `if` / `elif` / `else`.
- **Truthiness**: empty things (`""`, `[]`, `{}`, `0`, `None`) count as `False`. So `if items:` means "if the list isn't empty."
- `for` loops go over *things*, not indexes: `for name in names:`.
- `range()`, `enumerate()` (index + item), `zip()` (walk two lists together).
- `while`, `break`, `continue`, and the unusual `for ... else`.
- `match` statement (Python's much more powerful `switch`) `[3.10+]`.
- Conditional expression: `x = "yes" if ok else "no"`.

**Practice:** Solve 15 easy problems (loops, strings, dicts) on LeetCode or Exercism's Python track.

---

## Week 4 — Functions

**Topics**
- `def`, `return`, returning several values at once (`return a, b` gives back a tuple).
- Default values, keyword arguments: `send(to="ana", retries=3)`.
- `*args` (any number of positional values) and `**kwargs` (any number of named values).
- **The mutable default trap**: `def f(items=[])` shares ONE list across all calls. Use `None` instead.
- Functions are objects: you can store them in variables, pass them around, return them.
- `lambda`: tiny unnamed functions: `sorted(users, key=lambda u: u.age)`.
- Scope: local vs global, and the LEGB rule (Local → Enclosing → Global → Built-in: the order Python searches for a name).
- Docstrings: the `"""..."""` description at the top of a function.
- **Type hints (intro)**: `def greet(name: str) -> str:`. Python doesn't enforce them, but tools and editors use them. You'll use them everywhere from Part 2 on.

---

## Week 5 — Files, errors, modules, the standard library

**Topics**
- Reading and writing files with `with open(...) as f:`. `with` closes the file for you, like Java's try-with-resources.
- `pathlib.Path`: the modern way to work with file paths.
- `json` and `csv` modules.
- Errors: `try` / `except` / `else` / `finally`, `raise`, making your own exception classes.
- Why you should catch specific errors (`except ValueError:`), never a bare `except:`.
- **EAFP** ("Easier to Ask Forgiveness than Permission"): just try it and handle the error, instead of checking everything first. A very Python habit.
- Modules and packages: `import`, `from x import y`, what `__init__.py` does, and `if __name__ == "__main__":`.
- Standard library tour: `os`, `sys`, `datetime`, `random`, `re` (regular expressions), `argparse` (command-line arguments), `collections.Counter`.

---

## Week 6 — Classes and objects

**Topics**
- `class`, `__init__` (like a constructor), `self` (like `this`, but written explicitly).
- Instance attributes vs class attributes.
- `_private` by convention. No real `private` keyword.
- `__str__` (text for users) and `__repr__` (text for developers and debugging).
- Inheritance, `super()`, overriding.
- `@classmethod` (e.g. alternative constructors like `User.from_json(...)`) and `@staticmethod`.
- `@property`: start with plain attributes; turn one into a property later without breaking any code.
- **Duck typing**: no interfaces needed. If an object has a `.read()` method, you can treat it like a file.
- `@dataclass`: writes `__init__`, `__repr__`, and `__eq__` for you. Replaces most Java boilerplate (like Lombok).
- When NOT to use a class: if it has one method and no state, it should be a function.

---

## Small projects (AI-flavored)

| # | Project | What you'll practice |
|---|---|---|
| 1.1 | **Token & cost calculator**: estimate how many tokens a text uses and what it costs on different LLM models (prices stored in a dict) | input, math, dicts, f-strings |
| 1.2 | **Prompt template filler**: read a prompt template with `{placeholders}` and fill it from a JSON file | strings, files, JSON, errors |
| 1.3 | **Chat log analyzer**: load a chat export (JSON), count messages per role, find the longest replies and the most common words | dicts, `Counter`, sorting, loops |
| 1.4 | **Rule-based chatbot**: a tiny ELIZA-style bot using `match` and regex patterns | control flow, functions, `re` |
| 1.5 | **Training data cleaner**: take a CSV of question/answer pairs, remove duplicates and empty rows, fix whitespace, and write **JSONL** (the file format used to fine-tune LLMs) | csv, json, sets, pathlib |

## Big project — Second Brain v1: Keyword Search

A command-line tool that searches a folder of your `.txt` / `.md` notes.

- `uv run brain index ~/notes` reads all files and builds a word index.
- `uv run brain search "python generators"` shows the top 5 matching notes with a relevance score.
- Scoring: how often the words appear, weighted so rare words count more (a simple form of **TF-IDF**, a classic search scoring method).
- Classes: `Document`, `Index`, `SearchResult` (as dataclasses).
- Saves the index to a JSON file so it doesn't rebuild every time.
- Handles errors cleanly (missing folder, empty files, strange characters).

**Why:** In Part 3 you'll replace keyword search with AI embeddings and *measure* whether it's actually better.

---

## ✅ Checkpoint — you should be able to…

- [ ] Create a project with `uv`, add a library, and run it.
- [ ] Explain why `b = a` doesn't copy a list, and how to actually copy it.
- [ ] Loop the Python way (no `for i in range(len(x))` unless you truly need the index).
- [ ] Explain the mutable default argument trap.
- [ ] Read/write JSON, CSV, and text files safely with `with`.
- [ ] Write a class with a dataclass and explain when a plain function is better.
- [ ] Solve easy LeetCode problems in Python without looking up syntax.

## ⚠️ Common mistakes at this stage

- Writing Java in Python: getters/setters, a class for everything, `for i in range(len(list))`.
- Using `==` vs `is` wrong. Use `is` only for `None` (`if x is None`).
- Catching every error with a bare `except:`, which hides real bugs.
- Forgetting that lists and dicts are shared, not copied, when passed around.
- Installing libraries globally instead of per project.

## 📚 Resources

- **Book:** *Python Crash Course* (Eric Matthes), chapters 1–11. Fast and practical.
- **Book (for Java devs):** *Learning Python* (Mark Lutz), as a reference only.
- **Free:** The official Python Tutorial (docs.python.org/3/tutorial). Short and excellent.
- **Practice:** Exercism Python track (free, with mentors), LeetCode Easy.
- **Talk:** Ned Batchelder, *"Facts and Myths about Python Names and Values"* (PyCon 2015). Watch it this part. It fixes the name-tag vs box confusion for good.
