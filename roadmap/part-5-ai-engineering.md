# Part 5 — AI Engineering (Weeks 30–41)

**Goal:** Understand AI from the inside (build a neural network and a tiny GPT from scratch), then build the real things AI companies ship: fine-tuned models, strong RAG systems, agents, evaluations, and model serving.

**The rule for this part:** *build it from scratch first, then use the library.* Libraries and frameworks in AI change every few months. Understanding lasts.

---

## Weeks 30–31 — Neural networks from scratch (pure Python)

**Topics**
- What a neuron is: inputs × weights, added up, passed through a simple function.
- **Loss**: a number measuring "how wrong" the model is.
- **Gradient**: which direction to nudge each weight to make the loss smaller.
- **Backpropagation**: calculating all those gradients automatically, working backwards through the calculation.
- **Gradient descent**: nudge, check, repeat, thousands of times.
- Build **micrograd**: a tiny "autograd" engine (automatic gradients) in ~150 lines of Python, then a small neural network on top of it.
- Uses everything from Part 2: classes, magic methods (`__add__`, `__mul__`), and recursion.

## Weeks 32–33 — PyTorch

**Topics**
- **Tensors**: NumPy arrays that can run on a GPU and track gradients.
- `autograd` (what you built in micrograd, but industrial-strength).
- `nn.Module`, layers, activation functions, loss functions, optimizers (SGD, AdamW).
- The **training loop**: forward → loss → backward → step. Know it by heart.
- `Dataset` and `DataLoader` (feeding data in batches).
- Running on Apple Silicon (`mps` device) or a cloud GPU (Colab, Kaggle, RunPod, Lambda).
- Saving and loading models; watching training with TensorBoard or Weights & Biases.

## Weeks 34–35 — Transformers and a tiny GPT

**Topics**
- **Tokenization**: how text becomes numbers. Build a **BPE** tokenizer (Byte-Pair Encoding, the method GPT models use) yourself.
- A character-level language model (predict the next character) → a bigram model → a small **Transformer**.
- **Attention**, in plain words: each word looks at the other words and decides which ones matter for understanding it.
- Build and train a tiny GPT (nanoGPT style) on Shakespeare, then on your own notes.
- What changes at big scale: data, compute, and training tricks. Why you won't train GPT-5 at home, and why understanding this still makes you a much better AI engineer.

## Week 36 — Hugging Face and open models

**Topics**
- `transformers`: loading pre-trained models, tokenizers, and pipelines.
- The Hugging Face Hub: finding models and datasets, reading model cards.
- Running open models locally: **Ollama**, **llama.cpp**, **MLX** (fast on Apple Silicon).
- **Quantization** in plain words: storing model weights with fewer bits (e.g. 4-bit), so big models fit in less memory with a small quality loss.
- Picking a model: size vs speed vs quality vs cost vs license.

## Week 37 — Fine-tuning

**Topics**
- When to fine-tune vs when to just prompt better or use RAG. Most of the time, try those two first.
- **LoRA / QLoRA**: fine-tuning by training small "adapter" layers instead of the whole model. Cheap enough for one GPU.
- Tools: Hugging Face `peft` + `trl`, or **Unsloth** (fast, beginner-friendly).
- Preparing a dataset (the JSONL cleaner from project 1.5 comes back here).
- Evaluating before/after on a held-out test set. No evaluation = no proof it helped.

## Week 38 — RAG done properly

**Topics**
- Chunking strategies (fixed size, by section, by meaning) and their trade-offs.
- **Hybrid search** (keyword + embeddings) and **reranking** (a second model that re-sorts the top results more accurately).
- Query rewriting, metadata filters, citing sources.
- Why RAG fails: wrong chunks retrieved, right chunks ignored, made-up answers. How to find which one is happening.
- **Evaluating RAG**: retrieval metrics (recall@k) + answer quality (faithfulness: does the answer stick to the sources?). Tools: Ragas, or your own scripts.

## Week 39 — Agents and tool use

**Topics**
- An **agent** = an LLM in a loop: think → choose a tool → run it → look at the result → repeat until done.
- Build an agent **from scratch** first: raw API + tool calling + a while loop, with no framework.
- Then compare frameworks: **Pydantic AI**, **LangGraph**, **OpenAI Agents SDK**, Anthropic's **Claude Agent SDK**. Learn one well; know what the others offer.
- **MCP (Model Context Protocol)**: a standard way to give AI apps access to tools and data. Build an MCP server in Python with the official SDK.
- Safety: **prompt injection** (malicious text tricking the model into misbehaving), limiting what tools can do, asking a human before risky actions.
- Why agents fail (loops, wrong tools, lost context) and how to keep them reliable.

## Week 40 — Evaluation, observability, and serving

**Topics**
- **Evals**: test sets for AI behavior; exact-match checks, code-based checks, and **LLM-as-judge** (using a model to grade answers, plus its weaknesses).
- Running evals like unit tests, so a prompt change that makes things worse gets caught.
- **Tracing**: recording every step of an LLM app (prompt, tools, tokens, time, cost). Tools: Langfuse, Logfire, Arize Phoenix, OpenTelemetry.
- **Serving** your own models: **vLLM** (fast, high-throughput LLM server), batching, streaming, GPU memory basics.
- Cost and speed: prompt caching, smaller models for easy tasks, routing between models.

## Week 41 — Project week

---

## Small projects (AI)

| # | Project | What you'll practice |
|---|---|---|
| 5.1 | **micrograd**: build your own autograd engine + a small neural net that learns a toy dataset | backprop, magic methods |
| 5.2 | **Digit recognizer**: train an MNIST classifier in PyTorch, reach >98% accuracy, then plot its mistakes | PyTorch, training loop |
| 5.3 | **BPE tokenizer**: train your own tokenizer, compare it with a real one (`tiktoken`) | algorithms, strings |
| 5.4 | **Tiny GPT**: train a small transformer on text you choose and generate new text from it | transformers, attention |
| 5.5 | **LoRA fine-tune**: fine-tune a small open model (1–4B parameters) on a narrow task, measure before vs after | fine-tuning, evaluation |
| 5.6 | **Agent from scratch**: an agent with calculator, web search, and file-reading tools, with no framework | tool calling, loops, safety |
| 5.7 | **MCP server**: expose your Second Brain notes as an MCP server and use it from Claude Desktop or another MCP client | MCP, protocols |
| 5.8 | **Eval harness**: a pytest-style runner for prompts: test cases, scoring, an HTML report, and comparison between runs | testing, evaluation |

## Big project — Second Brain v4: AI Research Assistant

- **Advanced RAG**: hybrid search + reranking + citations, with measured improvement over v2/v3.
- **Agent mode**: can search your notes, search the web, read a URL, and write a new summary note, asking you before saving anything.
- **MCP server** so other AI tools can use your notes.
- **Eval suite**: 50+ test questions. Retrieval and answer-quality scores tracked over time.
- **Tracing** on every request (tokens, cost, time, tool calls).
- Works with either an API model or a local model (Ollama/vLLM), switchable in config.
- A write-up: what you tried, what the numbers showed, what failed.

---

## ✅ Checkpoint — you should be able to…

- [ ] Explain backpropagation using your own micrograd code.
- [ ] Write a PyTorch training loop from memory.
- [ ] Explain attention and tokenization simply, using your own implementations.
- [ ] Decide between prompting, RAG, and fine-tuning for a problem, with reasons.
- [ ] Build an agent without a framework, and explain what frameworks add.
- [ ] Show *numbers* proving an AI change made things better.

## ⚠️ Common mistakes at this stage

- Jumping to frameworks (LangChain etc.) without understanding the raw API calls underneath.
- Fine-tuning when better prompts or RAG would have worked.
- No evaluation set. "It looks good" is not a result.
- Giving an agent powerful tools (delete files, send email, run code) with no limits.
- Ignoring cost and latency until the bill arrives.
- Chasing every new model and framework instead of going deep.

## 📚 Resources

- **Video course (essential):** Andrej Karpathy, *"Neural Networks: Zero to Hero"*: micrograd, makemore, GPT from scratch, tokenizer. The spine of weeks 30–35.
- **Book:** *Build a Large Language Model (From Scratch)* (Sebastian Raschka).
- **Book:** *AI Engineering* (Chip Huyen). The best overview of building real products on top of models.
- **Book:** *Deep Learning with PyTorch Step-by-Step* (Daniel Voigt Godoy), or the official PyTorch tutorials.
- **Course:** Hugging Face LLM Course (free).
- **Course:** fast.ai *"Practical Deep Learning for Coders"* (free).
- **Docs:** Anthropic "Building effective agents" (engineering blog), MCP docs (modelcontextprotocol.io), vLLM docs.
- **Blog:** Hamel Husain on LLM evals; Eugene Yan's writing on applied ML/LLM systems.
