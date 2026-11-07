# 07. LLM Foundations & Model Mechanics for Engineers

> **Journey Stage**: Core Principles & Infrastructure  
> **Purpose**: Master the technical fundamentals of Large Language Models (LLMs) and reasoning architectures to make informed decisions about model selection, latency, cost, and hosting.

---

## 1. Core Keywords & Terminology

### Model Mechanics & Architecture
- **Transformer Architecture**: The foundational neural network architecture based on self-attention mechanisms that allows models to process long-range dependencies in code and text simultaneously.
- **Tokenization & Byte-Pair Encoding (BPE)**: The process of breaking source code and natural language down into sub-word tokens. Code tokenization often consumes more tokens per character due to indentation, symbols, and camelCase naming.
- **Context Length / Effective Window**: The total token capacity a model can accept. Long-context models (1M+ tokens) allow feeding entire codebases, though retrieval precision can degrade across vast contexts.
- **Reasoning Models & Thinking Tokens**: Models trained via reinforcement learning (e.g., OpenAI o1/o3, DeepSeek R1) that generate hidden "chain of thought" tokens before emitting the final code, excelling at complex algorithmic logic.

### Sampling Parameters
- **Temperature ($T$)**: Controls output randomness. For deterministic coding, schema compliance, and bug fixing, set $T \approx 0.0\text{–}0.2$. For creative brainstorming or alternative architecture proposals, set $T \approx 0.7$.
- **Top-$p$ (Nucleus Sampling)**: Dynamically truncates the probability distribution to tokens whose cumulative probability exceeds $p$.
- **Frequency & Presence Penalties**: Penalties that discourage repetitive loops or encourage exploring novel topics.

### Optimization & Serving
- **Quantization (GGUF, AWQ, FP8, INT4)**: Reducing precision of model weights from 16-bit floating point to 8-bit or 4-bit integers, reducing memory footprint by 50–75% with minimal accuracy loss.
- **vLLM / Ollama / llama.cpp**: Modern high-throughput inference engines and runtimes for hosting open-weights models locally or in private VPCs.
- **Fine-Tuning vs. LoRA vs. RAG**:
  - *RAG*: Inject dynamic documentation/code at query time (fastest, lowest cost).
  - *LoRA (Low-Rank Adaptation)*: Parameter-efficient fine-tuning on proprietary codebase styles.
  - *Full Fine-Tuning*: Retraining all model weights (expensive, high maintenance).

---

## 2. Model Selection Matrix for SWE Tasks

| Tier | Example Models | Best Use Cases | Latency / Cost |
| :--- | :--- | :--- | :--- |
| **High-Speed / Utility** | Gemini 1.5/2.0 Flash, Claude 3.5 Haiku, GPT-4o-mini | Autocomplete, lint fixes, single-function tests, docstrings | Ultra low latency (<500ms), low cost |
| **Frontier Workhorse** | Claude 3.5/3.7 Sonnet, GPT-4o | Multi-file features, PR reviews, refactoring, context search | Balanced latency, medium cost |
| **Deep Reasoning** | OpenAI o1/o3, DeepSeek R1 | Distributed system design, tricky concurrency bugs, algorithmic optimization | High latency (thinking time), higher cost |
| **Local / Private** | Llama 3.3 70B, Qwen 2.5 Coder, DeepSeek Coder | Air-gapped codebases, proprietary IP, offline terminal work | Zero API cost, requires local GPU/RAM |

---

## 3. Cost & Latency Optimization Rules

1. **Route by Complexity**: Don't use a deep-reasoning frontier model to fix a syntax error or generate a standard regex.
2. **Cache Prompts**: Use prompt caching (supported by Anthropic, Gemini, OpenAI) to avoid re-billing static context like system prompts and repository maps.
3. **Trim Context**: High-token inputs drastically increase latency and cost; send only relevant AST chunks.

---

## 4. Journey Self-Reflection

- [ ] Do I understand the trade-offs between local models (Ollama/Qwen) and cloud frontier models (Claude/Gemini)?
- [ ] Am I using appropriate temperature settings for coding vs brainstorming?
- [ ] Have I benchmarked the cost of running coding agents against developer time saved?
