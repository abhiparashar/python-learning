# Part 3 — Data & AI Foundations (Weeks 14–21)

**Goal:** Get comfortable with data, the math intuition behind AI (no heavy formulas), calling LLM APIs from code, classic machine learning, and embeddings.

**Why this part matters:** Every AI system is mostly data handling plus API calls plus a little math. People who skip this part build AI apps they can't debug.

---

## Week 14 — NumPy and the math you actually need

**Topics**
- NumPy arrays: like lists, but fast and all the same type. Shapes: `(3,)`, `(3, 4)`, `(batch, tokens, dims)`.
- **Vectorization**: do math on whole arrays at once instead of looping. Often 50–100x faster.
- **Broadcasting**: how NumPy combines arrays of different shapes.
- Math intuition, in plain words:
  - A **vector** = a list of numbers describing something (a word, an image, a user).
  - The **dot product** = a measure of how much two vectors "agree."
  - **Cosine similarity** = how similar two vectors' *directions* are (−1 to 1). This powers AI search.
  - A **matrix** = a table of numbers that transforms vectors. Neural networks are mostly matrix multiplications.
- Random numbers and seeds (so results can be repeated).

## Week 15 — DataFrames: pandas and Polars

**Topics**
- **DataFrame** = a table in code (rows and columns), like a spreadsheet you control with Python.
- Loading CSV/JSON/Parquet, selecting, filtering, sorting, `groupby`, joins, handling missing values.
- **pandas** (the classic, used everywhere) vs **Polars** (newer, much faster, written in Rust). Learn pandas' basics; prefer Polars for new work.
- **Jupyter notebooks**: interactive documents for exploring data. Great for exploring, bad for production code.

## Week 16 — Seeing data: plotting and basic statistics

**Topics**
- `matplotlib` and `seaborn`: histograms, scatter plots, line charts.
- Statistics in plain words: mean vs median, spread (standard deviation), distributions, correlation vs causation, sampling.
- **Exploratory data analysis (EDA)**: the habit of *looking* at data before trusting it.
- Hugging Face `datasets` library: load real AI datasets in one line.

## Week 17 — Talking to web APIs

**Topics**
- HTTP basics: GET/POST, status codes, headers, JSON bodies.
- `httpx` (modern; supports async, which you'll use in Part 4) and `requests` (classic).
- API keys and secrets: `.env` files, environment variables, never committing keys to Git.
- Handling failures: timeouts, retries with backoff, rate limits (HTTP 429).
- Pagination (getting results page by page).

## Week 18 — Calling LLMs from Python

**Topics**
- LLM API basics: messages with **roles** (system / user / assistant), **tokens** (pieces of words the model reads and writes; you pay per token), **context window** (the maximum tokens a model can see at once), **temperature** (randomness).
- Official SDKs (Anthropic, OpenAI, Google) and **Ollama** (runs open models locally on your Mac for free).
- **Streaming** responses (connects to Week 8 generators).
- **Structured output**: making the model return JSON that matches a Pydantic model, then validating it.
- **Tool calling (intro)**: letting the model ask your code to run a function.
- Tracking cost and latency for every call.
- Prompt basics: clear instructions, examples ("few-shot"), step-by-step reasoning, and separating instructions from user data.

## Week 19 — Classic machine learning with scikit-learn

**Topics**
- What "training a model" means: showing examples so it learns patterns.
- Features (inputs) and labels (answers). Train/test split, and why you never test on training data.
- Models: linear/logistic regression, decision trees, random forests, k-nearest neighbors.
- Measuring quality: accuracy, precision, recall, F1, and the confusion matrix, plus when accuracy lies (unbalanced data).
- **Overfitting**: memorizing the examples instead of learning the pattern.
- Text features: bag-of-words, TF-IDF (you built a simple one in Part 1!).

## Week 20 — Embeddings and semantic search

**Topics**
- **Embeddings**: an AI model turns text into a vector so that texts with similar *meaning* get similar vectors.
- `sentence-transformers` (local, free) and API embedding models.
- **Chunking**: splitting long documents into pieces before embedding them, and why the chunk size matters.
- Nearest-neighbor search: brute force with NumPy first, then a **vector database** (Chroma, LanceDB, or Postgres + pgvector).
- Measuring search quality: build a small test set of questions with known correct answers and compute **recall@k** ("was the right answer in the top k results?").

## Week 21 — Project week

Finish the big project below and write up your results.

---

## Small projects (AI-flavored)

| # | Project | What you'll practice |
|---|---|---|
| 3.1 | **Similarity search from scratch**: embed 1,000 sentences, then find the most similar ones using only NumPy (no vector DB) | vectors, dot product, vectorization |
| 3.2 | **Dataset explorer**: load a Hugging Face dataset (e.g. a Q&A dataset) with Polars, clean it, plot its statistics, and report problems you find | Polars, plotting, EDA |
| 3.3 | **Spam / sentiment classifier**: TF-IDF + logistic regression with scikit-learn, with a proper evaluation report | ML basics, metrics |
| 3.4 | **Terminal chat app**: chat with an LLM (API or Ollama), streamed output, conversation memory (from 2.4), and a running cost counter | LLM APIs, streaming |
| 3.5 | **Structured extractor**: pull fields from resumes or invoices into a Pydantic model, retrying when the output is invalid | structured output, validation |
| 3.6 | **LLM vs classic ML**: solve the same classification task with project 3.3 and with an LLM prompt. Compare accuracy, cost, and speed in a table | evaluation, honest comparison |

## Big project — Second Brain v2: Semantic Search

Upgrade v1 (keyword search) to meaning-based search.

- Chunk your notes → embed the chunks → store them in a vector DB (Chroma or LanceDB).
- `brain search "how do generators save memory?"` finds relevant notes even if they don't share exact words.
- **Hybrid mode**: combine keyword (v1) and semantic scores.
- **Evaluation**: write 30 test questions with the note that should answer each. Report recall@5 for keyword vs semantic vs hybrid.
- `brain ask "..."`: send the top chunks to an LLM and get an answer with the source notes listed. This is your first **RAG** (Retrieval-Augmented Generation: *find* relevant text, then have the LLM *answer using it*).

**Why:** You'll learn the core pattern of most real AI products, and that you should *measure* before believing "AI search is better."

---

## ✅ Checkpoint — you should be able to…

- [ ] Explain cosine similarity to a non-programmer.
- [ ] Rewrite a Python loop as NumPy code and measure the speedup.
- [ ] Load, clean, group, and plot a dataset with Polars or pandas.
- [ ] Call an LLM with streaming, structured output, and error handling.
- [ ] Train and *honestly evaluate* a scikit-learn classifier.
- [ ] Build semantic search and prove with numbers whether it beats keyword search.

## ⚠️ Common mistakes at this stage

- Looping over NumPy arrays in Python (throws away the speed).
- Trusting a model's accuracy without a separate test set.
- Trusting LLM JSON without validating it.
- Hard-coding API keys.
- Picking a vector database before understanding brute-force search.
- Judging AI quality by "vibes" instead of a test set.

## 📚 Resources

- **Book:** *Python for Data Analysis*, 3rd ed. (Wes McKinney, creator of pandas). Free online.
- **Book:** *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow*, 3rd ed. (Aurélien Géron), Part I only. (A PyTorch edition also exists; either works.)
- **Video:** 3Blue1Brown, *"Essence of Linear Algebra"* (visual, no heavy math) and the *"Neural Networks"* series.
- **Docs:** NumPy "Absolute Beginner's Guide", Polars User Guide, scikit-learn "Getting Started".
- **Docs:** Anthropic and OpenAI API docs (prompting guides, tool use, structured outputs).
- **Course:** Kaggle Learn (free): Pandas, Intro to ML, Data Visualization.
