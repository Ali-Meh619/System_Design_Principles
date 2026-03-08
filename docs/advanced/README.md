# 🩷 AI & Machine Learning

> From ML system architecture to cutting-edge LLMs — these topics appear at staff/principal engineer interviews and increasingly at senior-level interviews as AI systems become mainstream.

---

## Topics in This Category

| # | Topic | Difficulty | What You'll Learn |
|---|-------|-----------|------------------|
| 1 | [Machine Learning in System Design](ml-in-system-design.md) | 🔴 Advanced | ML system architecture, feature store, model serving |
| 2 | [AI Agent System Design](ai-agent-system-design.md) | 🔴 Advanced | Agent anatomy, cognitive architectures, multi-agent patterns |
| 3 | [Classic Machine Learning](../machine-learning/classic-ml.md) | 🟡 Intermediate | Bias-variance, Random Forests, XGBoost, SVM, PCA, data leakage |
| 4 | [Deep Learning](../machine-learning/deep-learning.md) | 🟡 Intermediate | Backprop, Adam, BatchNorm, CNNs, LSTMs, full Transformer deep-dive |
| 5 | [LLM Interview Questions](../machine-learning/llm-interviews.md) | 🔴 Advanced | Transformers, RAG, LoRA, RLHF/DPO, decoding, KV cache, CoT |

---

## When These Topics Come Up

| Topic | Interview context |
|-------|-----------------|
| ML system design | "Design a recommendation system / feed ranking / fraud detection" |
| AI agents | "Design an autonomous coding assistant (like Cursor/Devin)" |
| Classic ML | "Walk me through your approach to a classification problem with imbalanced data" |
| Deep Learning | "Explain how you would train a CNN for image classification; why does ResNet solve the degradation problem?" |
| LLMs | "Design a document Q&A system / explain how RAG works / what is LoRA?" |

---

## ML System Architecture Layers

```
1. Data Pipeline
   └─ Collect raw events → Feature engineering → Feature Store

2. Training
   └─ Feature Store → Model Training → Model Registry

3. Serving
   └─ Model Registry → Online Serving API
                     → Batch Prediction Job

4. Monitoring
   └─ Track: prediction quality, feature drift, latency, errors

5. Feedback Loop
   └─ User actions → Labels → Retrain triggers
```

**Feature Store is the most important ML infrastructure:**
- Provides the same features to both training and serving
- Prevents **training-serving skew** (different data → different predictions)

---

## AI Agent Design — Core Components

```
┌─────────────────────────────────────────────────┐
│                   AI Agent                       │
│                                                  │
│  [Planner/LLM] ←──→ [Memory]                    │
│       │                  ├─ Short-term (context) │
│       ▼                  └─ Long-term (vector DB)│
│  [Tool Executor]                                 │
│       ├─ Web search                              │
│       ├─ Code execution                          │
│       ├─ File system                             │
│       └─ API calls                               │
└─────────────────────────────────────────────────┘
```

**Key design patterns:**
- **ReAct**: Reason → Act → Observe → Repeat (most common)
- **Reflection**: Agent critiques its own output and improves
- **Multi-agent**: Orchestrator delegates to specialized sub-agents

---

## LLM Quick Reference

| Concept | Key point |
|---------|-----------|
| **RAG** | Retrieve relevant context at inference time; grounds answers, reduces hallucination |
| **LoRA** | Fine-tune only low-rank matrices (B·A); 1000× fewer parameters than full fine-tune |
| **KV Cache** | Cache K/V of past tokens to avoid recomputing them every decode step |
| **CoT** | Externalises reasoning as tokens; more compute per answer; fails on factual recall |
| **RLHF** | Train reward model from human prefs → PPO; complex; DPO is simpler alternative |
| **Top-p sampling** | Adaptive nucleus sampling; standard for chat; temperature=0 for factual tasks |

---

## Practice Questions

1. Design a recommendation system for Netflix. Cover: data collection, feature engineering, offline training, online serving, A/B testing, and feedback loop.

2. An AI coding assistant needs to maintain context across a 10,000-line codebase. Design its memory system — what stays in context, what goes to long-term memory, and how retrieval works.

3. Design a document Q&A system using RAG. How do you handle: long documents, irrelevant chunks being retrieved, and keeping answers up to date?

4. You have a classification model with 99% accuracy on test data, but it's performing poorly in production. What are the likely causes and how do you diagnose them?
