# ✨ Generative AI Roadmap

> *A structured, opinionated guide to mastering Generative AI — assumes Deep Learning fundamentals are solid.*

---

## 📍 How to Use This Roadmap

- ✅ = Essential (do this)
- ⭐ = Highly recommended
- 🔬 = Advanced / research-level

Each stage builds on the previous. Don't skip Stage 1 — everything else depends on understanding LLMs deeply.

---

## Stage 1 — Large Language Models (LLMs)

*The backbone of modern Generative AI. Understand this inside-out.*

### 1.1 Architecture & Internals

- [ ] Decoder-only Transformer (GPT-style) in depth
- [ ] Tokenization: BPE, WordPiece, SentencePiece, tiktoken
- [ ] Context window & positional encodings: Sinusoidal → RoPE → ALiBi
- [ ] KV-Cache — how inference is made efficient
- [ ] Flash Attention & memory-efficient attention
- [ ] Grouped Query Attention (GQA), Multi-Query Attention (MQA)

### 1.2 Sampling & Generation

- [ ] Greedy decoding
- [ ] Beam Search
- [ ] Top-k Sampling
- [ ] Top-p / Nucleus Sampling
- [ ] Temperature scaling
- [ ] Repetition penalty, frequency penalty

```python
# Conceptual generation loop
tokens = tokenize(prompt)
while not done:
    logits = model(tokens)              # forward pass
    logits /= temperature               # scale
    probs = softmax(top_p_filter(logits))
    next_token = sample(probs)
    tokens.append(next_token)
```

### 1.3 Training Pipeline

```
Raw Text Corpus (trillions of tokens)
          ↓
  Pretraining — next-token prediction
          ↓
  Supervised Fine-Tuning (SFT) — instruction data
          ↓
  Reward Model Training
          ↓
  RLHF (PPO) or DPO — align to human preferences
          ↓
        Deployed LLM
```

- [ ] Pretraining objectives: Causal LM, Masked LM
- [ ] Instruction tuning & chat templates
- [ ] **RLHF** — reward model + PPO loop
- [ ] **DPO** (Direct Preference Optimization) — simpler RLHF alternative
- [ ] Constitutional AI & RLAIF
- [ ] Chinchilla scaling laws (compute-optimal training)

### 1.4 Landmark Models to Study

| Model | Organization | Why it matters |
|---|---|---|
| GPT-2 / GPT-3 | OpenAI | Scaling laws, in-context learning |
| LLaMA / LLaMA 3 | Meta | Open-weights, research standard |
| Mistral / Mixtral | Mistral AI | MoE, efficient open models |
| Gemma 2 / 3 | Google DeepMind | Compact, high-quality open models |
| Phi-3 / Phi-4 | Microsoft | Small but surprisingly capable |
| Claude 3.x | Anthropic | Long context, safety-focused |
| Qwen 2.5 | Alibaba | Strong multilingual open model |

### 📚 Resources
- [Andrej Karpathy — Let's build GPT from scratch](https://www.youtube.com/watch?v=kCc8FmEb1nY) ⭐
- [GPT-3 Paper](https://arxiv.org/abs/2005.14165)
- [InstructGPT Paper](https://arxiv.org/abs/2203.02155)
- [DPO Paper](https://arxiv.org/abs/2305.18290)

---

## Stage 2 — Prompt Engineering

*Often underrated. A well-crafted prompt unlocks dramatically better outputs.*

### 2.1 Core Techniques

- [ ] Zero-shot prompting
- [ ] One-shot & Few-shot prompting
- [ ] **Chain-of-Thought (CoT)** — "Let's think step by step"
- [ ] Zero-shot CoT
- [ ] Self-consistency (sample multiple CoT paths, majority vote)
- [ ] Tree of Thought (ToT) 🔬
- [ ] **ReAct** — Reasoning + Acting interleaved
- [ ] Role prompting & system prompt design
- [ ] Output format control (JSON mode, structured outputs, XML tags)

```
# Few-shot CoT Example
Q: Roger has 5 tennis balls. He buys 2 cans of 3 balls each. How many does he have?
A: Roger starts with 5. 2 cans × 3 = 6 new balls. 5 + 6 = 11. The answer is 11.

Q: [Your actual question]
A: Let's think step by step...
```

### 2.2 Advanced Prompting

- [ ] Meta-prompting (LLM writes its own prompts)
- [ ] Prompt chaining — break tasks into sequential prompts
- [ ] Generated Knowledge Prompting
- [ ] Least-to-Most Prompting (decompose hard problems)
- [ ] **DSPy** 🔬 — programmatic prompt optimization (no hand-writing)

### 2.3 Security & Robustness

- [ ] Prompt injection attacks
- [ ] Jailbreaking techniques & defenses
- [ ] Prompt leaking
- [ ] Indirect prompt injection (from retrieved content)

### 📚 Resources
- [Prompt Engineering Guide](https://www.promptingguide.ai/) ⭐
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Anthropic Prompt Library](https://docs.anthropic.com/en/prompt-library/library)
- [Learn Prompting](https://learnprompting.org/)

---

## Stage 3 — Working with LLM APIs

*Build real things. Know the ecosystem.*

### 3.1 Foundation Model APIs

| Provider | Models | Strengths |
|---|---|---|
| **OpenAI** | GPT-4o, o1, o3 | Widest adoption, best tool support |
| **Anthropic** | Claude 3.5 / 3.7 Sonnet | Long context, reasoning, safety |
| **Google** | Gemini 2.0 / 2.5 | Multimodal, 1M+ context |
| **Mistral** | Mistral Large, Codestral | Open + API, great for code |
| **Cohere** | Command R+ | RAG-optimized, enterprise |

### 3.2 Running Models Locally

- **Ollama** ⭐ — easiest way to run open models locally
- **LM Studio** — GUI for local models
- **llama.cpp** — CPU inference, quantized models
- **vLLM** — high-throughput GPU serving
- **Together AI / Fireworks AI / Groq** — fast cloud inference

```bash
# Run LLaMA 3 locally with Ollama
ollama run llama3
```

### 3.3 Key API Concepts

- [ ] Chat completions vs completions endpoint
- [ ] System / User / Assistant message roles
- [ ] Function calling & tool use
- [ ] Streaming responses
- [ ] Embeddings API
- [ ] Token counting & context management
- [ ] Rate limits, retries, cost estimation

### 📚 Resources
- [OpenAI API Docs](https://platform.openai.com/docs)
- [Anthropic API Docs](https://docs.anthropic.com)
- [Hugging Face Inference API](https://huggingface.co/docs/api-inference)

---

## Stage 4 — Retrieval-Augmented Generation (RAG)

*Give LLMs access to your own data. The most in-demand practical GenAI skill.*

### 4.1 Core RAG Pipeline

```
Your Documents (PDF, web, DB, etc.)
          ↓
   Chunking (split into passages)
          ↓
   Embedding (encode as vectors)
          ↓
   Vector Store (store + index)

          ↑ At query time:
User Query → Embed → Similarity Search → Top-K Chunks
                                              ↓
                        [Chunks + Query] → LLM → Grounded Answer
```

### 4.2 Chunking Strategies

- [ ] Fixed-size chunking (simple, works okay)
- [ ] Recursive character splitting (respects document structure)
- [ ] Semantic chunking (split on meaning, not just size)
- [ ] Document-aware chunking (headings, paragraphs, tables)
- [ ] Chunk overlap (avoid cutting context mid-sentence)

### 4.3 Embedding Models

| Model | Notes |
|---|---|
| `text-embedding-3-small/large` | OpenAI, strong baseline |
| `bge-m3` | Open, multilingual, state-of-art |
| `e5-mistral-7b` | Very strong open model |
| `nomic-embed-text` | Open, long context |
| `Cohere Embed v3` | Excellent for RAG |

### 4.4 Vector Databases

| DB | Best for |
|---|---|
| **Pinecone** | Managed, production-ready |
| **Weaviate** | Open source + cloud, hybrid search |
| **Qdrant** ⭐ | Fast, open source, great DX |
| **ChromaDB** | Lightweight, local dev |
| **pgvector** | Already using Postgres? Use this |
| **FAISS** | In-memory, research / offline |

### 4.5 Advanced RAG Techniques

- [ ] **Hybrid Search** — dense (vectors) + sparse (BM25) combined
- [ ] **Reranking** — cross-encoder reranker after retrieval (Cohere Rerank, BGE Reranker)
- [ ] **HyDE** — Hypothetical Document Embeddings
- [ ] **RAG-Fusion** — generate multiple queries, fuse results
- [ ] **Multi-query retrieval** — rephrase query N ways
- [ ] **Contextual compression** — extract only relevant parts of chunks
- [ ] **Parent-child chunking** — retrieve small chunks, send large context
- [ ] **Self-RAG** 🔬 — model learns when to retrieve

### 4.6 RAG Evaluation

- **Faithfulness** — is the answer grounded in retrieved docs?
- **Answer Relevance** — does it answer the question?
- **Context Recall** — was the right content retrieved?
- **Tools**: RAGAs, TruLens, DeepEval

### 📚 Resources
- [LlamaIndex Docs](https://docs.llamaindex.ai/) ⭐
- [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/)
- [RAGAs Evaluation](https://docs.ragas.io/)
- [Advanced RAG — Pinecone Guide](https://www.pinecone.io/learn/advanced-rag/)

---

## Stage 5 — Fine-Tuning LLMs

*When prompting and RAG aren't enough.*

### 5.1 When to Fine-Tune

| Scenario | Approach |
|---|---|
| Need knowledge from your docs | → RAG |
| Need specific tone / style / format | → Fine-tune |
| Latency-critical, no retrieval step | → Fine-tune |
| Domain-specific language (medical, legal) | → Fine-tune |
| Both style + dynamic knowledge | → Fine-tune + RAG |

### 5.2 PEFT Methods

- [ ] **LoRA** — inject low-rank matrices (A×B), train only those ⭐
- [ ] **QLoRA** — 4-bit quantization + LoRA, runs on a single GPU ⭐
- [ ] **DoRA** — weight decomposition LoRA
- [ ] **IA³** — scales activations, fewer params than LoRA
- [ ] Full fine-tuning — requires serious hardware (A100s+)

```
LoRA: W' = W₀ + α · (B × A)
      W₀ frozen  ↑ only A and B trained
      rank r ≪ d  →  massive parameter savings
```

```python
from peft import LoraConfig, get_peft_model

config = LoraConfig(
    r=16,                    # rank
    lora_alpha=32,           # scaling
    target_modules=["q_proj", "v_proj", "k_proj", "o_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)
model = get_peft_model(model, config)
model.print_trainable_parameters()
# trainable params: 6,553,600 || all params: 7,248,375,808 || trainable: 0.09%
```

### 5.3 Training Tools

| Tool | Notes |
|---|---|
| **Unsloth** ⭐ | 2x faster, 70% less memory, easiest to start |
| **Hugging Face TRL** | SFT, DPO, PPO trainers, production-grade |
| **Axolotl** | Config-driven, very flexible |
| **LitGPT** | Clean, hackable, great for learning |
| **torchtune** | PyTorch-native fine-tuning |

### 5.4 Dataset Preparation

- [ ] Instruction format: `{"prompt": ..., "response": ...}`
- [ ] Chat format: `[{"role": "user", ...}, {"role": "assistant", ...}]`
- [ ] Data quality >> data quantity (always)
- [ ] Deduplication, filtering, quality scoring
- [ ] Synthetic data generation with stronger models

### 5.5 Alignment Techniques

- [ ] **SFT** (Supervised Fine-Tuning) — train on (prompt, ideal response) pairs
- [ ] **RLHF** — train reward model → optimize with PPO
- [ ] **DPO** — use preference pairs directly, no reward model needed
- [ ] **ORPO** — odds ratio preference optimization, simpler than DPO
- [ ] **SimPO** 🔬 — reference-free preference optimization

### 📚 Resources
- [QLoRA Paper](https://arxiv.org/abs/2305.14314)
- [LoRA Paper](https://arxiv.org/abs/2106.09685)
- [Unsloth Notebooks](https://github.com/unslothai/unsloth)
- [Maxime Labonne — LLM Fine-Tuning Course](https://github.com/mlabonne/llm-course) ⭐

---

## Stage 6 — AI Agents & Tool Use

*LLMs that reason, plan, search, and take actions.*

### 6.1 Core Concepts

- [ ] Function calling / tool use (OpenAI, Anthropic, Gemini APIs)
- [ ] **ReAct loop**: Reason → Act → Observe → Repeat
- [ ] Task decomposition & planning
- [ ] Memory types: in-context, external (vector store), episodic, semantic
- [ ] Multi-agent systems

### 6.2 Agent Architecture

```
User Input
    ↓
Agent (LLM) ── decides to call a tool
    ↓
Tool Execution
  ├── Web Search
  ├── Code Interpreter
  ├── Database Query
  ├── API Call
  └── File Read/Write
    ↓
Observation → back to Agent
    ↓
Repeat until final answer → User
```

### 6.3 Agent Frameworks

| Framework | Best for |
|---|---|
| **LangGraph** ⭐ | Graph-based workflows, complex multi-step agents |
| **AutoGen** | Multi-agent conversations (Microsoft) |
| **CrewAI** | Role-based multi-agent teams |
| **Smolagents** | Lightweight, Hugging Face-native |
| **OpenAI Agents SDK** | Simple tool-use agents with OpenAI |
| **LlamaIndex Workflows** | Agent pipelines with RAG integration |

### 6.4 Key Agent Patterns

- [ ] **Tool-calling agent** — basic function calling
- [ ] **Code agent** — writes & executes code to solve tasks
- [ ] **RAG agent** — decides when and what to retrieve
- [ ] **Planner-executor** — separate planning and execution steps
- [ ] **Reflection agent** — critiques and revises its own output
- [ ] **Multi-agent** — orchestrator delegates to specialist agents

### 📚 Resources
- [ReAct Paper](https://arxiv.org/abs/2210.03629)
- [LangGraph Docs](https://langchain-ai.github.io/langgraph/)
- [Hugging Face Agents Course](https://huggingface.co/learn/agents-course) ⭐

---

## Stage 7 — Multimodal Generative AI

*Models that see, speak, hear, and create.*

### 7.1 Vision-Language Models (VLMs)

- [ ] How images are fed into LLMs (patch embeddings, ViT encoder)
- [ ] LLaVA, PaliGemma, Qwen-VL, InternVL
- [ ] GPT-4V / GPT-4o — vision + text + audio
- [ ] Tasks: image captioning, visual QA, OCR, chart understanding, document parsing

**Paper**: [LLaVA: Visual Instruction Tuning](https://arxiv.org/abs/2304.08485)

### 7.2 Text-to-Image Generation

| Model | Notes |
|---|---|
| **Stable Diffusion (SDXL / SD3)** | Open source, most flexible |
| **FLUX.1** ⭐ | Current open-source SOTA quality |
| **DALL·E 3** | Best prompt following via ChatGPT |
| **Midjourney** | Best aesthetic quality |
| **Ideogram 2** | Great text rendering in images |

- [ ] Diffusion process (DDPM, DDIM, flow matching)
- [ ] Text conditioning with CLIP / T5
- [ ] CFG (Classifier-Free Guidance)
- [ ] ControlNet — spatial control over generation
- [ ] IP-Adapter — image prompt conditioning
- [ ] LoRA for image fine-tuning (Dreambooth, textual inversion)

### 7.3 Text-to-Video

| Model | Notes |
|---|---|
| **Sora** | OpenAI, high quality |
| **Runway Gen-3** | Commercial, accessible |
| **Kling** | Strong Chinese open model |
| **CogVideoX** | Open source option |
| **Wan 2.1** | Strong open-source, 2025 |

### 7.4 Text-to-Audio & Speech

- Text-to-Speech: **ElevenLabs**, Bark, Parler-TTS, Kokoro
- Speech-to-Text: **Whisper** ⭐ (OpenAI open model)
- Music generation: **Suno**, Udio, MusicGen (Meta, open)
- Voice cloning: ElevenLabs, VITS

### 7.5 Any-to-Any Models

- GPT-4o — text + vision + audio in/out
- Gemini 2.0 Flash — native multimodal
- The trend: unified models over specialized pipelines

---

## Stage 8 — GenAI Orchestration & Ecosystem

*The tools and patterns for building real GenAI systems.*

### 8.1 Orchestration Frameworks

| Tool | Use Case |
|---|---|
| **LangChain** | Most popular, extensive integrations |
| **LlamaIndex** ⭐ | Purpose-built for RAG & data |
| **LangGraph** | Stateful, graph-based agent workflows |
| **DSPy** 🔬 | Programmatic LM pipelines, no hand-crafted prompts |
| **Instructor** | Structured outputs from any LLM |
| **Outlines** | Guided generation with regex / JSON schemas |
| **Haystack** | Production NLP pipelines |

### 8.2 Structured Outputs

```python
import instructor
from openai import OpenAI
from pydantic import BaseModel

class Movie(BaseModel):
    title: str
    year: int
    genre: str

client = instructor.from_openai(OpenAI())
movie = client.chat.completions.create(
    model="gpt-4o-mini",
    response_model=Movie,
    messages=[{"role": "user", "content": "Recommend a sci-fi movie"}]
)
# → Movie(title='Dune', year=2021, genre='Science Fiction')
```

### 8.3 Memory & State Management

- [ ] Short-term: conversation history in context window
- [ ] Long-term: summarization + vector store retrieval
- [ ] Entity memory: track people, facts across sessions
- [ ] Tools: **Mem0**, Zep, LangMem

### 8.4 Inference Optimization

- [ ] **Quantization**: INT8, INT4, GPTQ, AWQ, GGUF
- [ ] **Speculative decoding** — faster generation
- [ ] **Continuous batching** — maximize GPU utilization
- [ ] **vLLM** ⭐ — PagedAttention, highest throughput
- [ ] **TensorRT-LLM** — NVIDIA-optimized serving

---

## Stage 9 — Evaluation & Safety

*You can't improve what you don't measure.*

### 9.1 LLM Benchmarks

| Benchmark | Tests |
|---|---|
| **MMLU** | General knowledge, 57 subjects |
| **HumanEval / MBPP** | Code generation |
| **GSM8K / MATH** | Mathematical reasoning |
| **GPQA** | Graduate-level science Q&A |
| **MT-Bench** | Multi-turn conversation quality |
| **LMSYS Chatbot Arena** | ELO-based human preference |

### 9.2 RAG & Application Eval

- **RAGAs** — faithfulness, answer relevance, context recall
- **TruLens** — LLM app evaluation
- **DeepEval** — unit-test style LLM evaluation
- **LLM-as-a-judge** — GPT-4 / Claude grades outputs (cheaper than human eval)
- **ARES** 🔬 — automated RAG evaluation system

### 9.3 Safety & Alignment

- [ ] Hallucination — causes, mitigation (grounding, citations, self-check)
- [ ] Toxicity & bias evaluation
- [ ] Jailbreaking & adversarial prompts
- [ ] **Llama Guard** — open-source content moderation model
- [ ] **NeMo Guardrails** — programmable safety rails
- [ ] **Guardrails AI** — output validation framework
- [ ] Responsible AI practices: fairness, transparency, accountability

---

## Stage 10 — Specializations

*Pick your lane.*

### 🤖 LLM Applications
- Chatbots & virtual assistants
- Code generation tools (Copilot-style)
- Document intelligence & summarization
- Search & knowledge management

### 🎨 Creative AI
- Image generation & editing workflows
- Video production with AI
- AI music & sound design
- Multimodal storytelling

### 🏭 Enterprise GenAI
- Private deployments (on-premise, VPC)
- Document processing pipelines
- Customer support automation
- GenAI for structured data (text-to-SQL)

### 🔬 Research Directions
- Long-context & memory architectures (Mamba, RWKV)
- Mixture of Experts (MoE) models
- LLM reasoning (o1-style chain-of-thought)
- Embodied AI & robotics
- AI agents & world models

---

## 🗂️ Essential Practice Projects

| Project | Skills Covered |
|---|---|
| **Prompt library for a specific domain** | Prompt engineering, few-shot, CoT |
| **RAG chatbot over your own documents** | Embeddings, vector DB, LlamaIndex |
| **Fine-tune LLaMA 3 with QLoRA** | Unsloth, TRL, dataset prep |
| **AI agent with web search + code exec** | LangGraph, tool use, ReAct |
| **Text-to-image pipeline with ControlNet** | Diffusion models, image conditioning |
| **LLM evaluation harness** | RAGAs, LLM-as-judge, benchmarks |
| **End-to-end multimodal app** | Vision LLM + generation + API |
| **Personal AI assistant with memory** | Agents, Mem0, long-term memory |

---

## 📖 Canonical Books & Courses

### Books
| Resource | Best for |
|---|---|
| *Hands-On Large Language Models* — Alammar & Grootendorst | Visual, beginner-friendly, highly recommended ⭐ |
| *Building LLMs for Production* — Labonne | Practical deployment end-to-end |
| *Developing Apps with GPT-4 and ChatGPT* — Louf | API-focused, practical |
| *AI Engineering* — Chip Huyen | Systems thinking for GenAI |

### Courses
| Course | Platform |
|---|---|
| [DeepLearning.AI Short Courses](https://www.deeplearning.ai/short-courses/) ⭐ | Free, 1–2 hrs each, very practical |
| [Full Stack LLM Bootcamp](https://fullstackdeeplearning.com/llm-bootcamp/) | End-to-end app building |
| [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course) | Free, transformers + fine-tuning |
| [Hugging Face Agents Course](https://huggingface.co/learn/agents-course) | Free, agents from scratch |
| [LLM Zoomcamp](https://github.com/DataTalksClub/llm-zoomcamp) | Free, RAG + fine-tuning + deployment |
| [Maxime Labonne LLM Course](https://github.com/mlabonne/llm-course) | GitHub, free, comprehensive |

---

## ⏱️ Suggested Timeline

```
Stage 1      (LLMs — architecture, training pipeline)
Stage 2 + 3  (Prompt Engineering + APIs)
Stage 4      (RAG — build a real RAG system)
Stage 5      (Fine-Tuning — QLoRA a model)
Stage 6      (AI Agents — build a working agent)
Stage 7      (Multimodal — image, video, audio)
Stage 8 + 9  (Orchestration + Evaluation + Safety)
Stage 10     (Specialization of your choice)
Build projects, follow papers, stay current
```

---

## 🗞️ Stay Current

The field moves extremely fast. Follow these:

- [Hugging Face Daily Papers](https://huggingface.co/papers) ⭐
- [Lilian Weng's Blog](https://lilianweng.github.io/) — best deep dives
- [Sebastian Raschka's Newsletter](https://magazine.sebastianraschka.com/)
- [The Batch — DeepLearning.AI](https://www.deeplearning.ai/the-batch/)
- [arxiv cs.LG + cs.CL](https://arxiv.org) — source of truth
- [Papers With Code](https://paperswithcode.com) — benchmarks + code

---

> **The most important rule**: *Every week, ship something. A notebook, a chatbot, a fine-tuned model, a RAG pipeline. Reading without building is trivia, not skill.*

---

*The field is young, the opportunities are massive — stay curious, stay building.* 🚀
