# 🪩 Machine Learning Interview Prep

> Interview-focused AI/ML coverage split across two tracks: production ML system design and modern model fundamentals. Use the system-design docs when you need architecture trade-offs, and the theory docs when you need concepts, algorithms, and model behavior.

---

## Topics in This Category

| # | Topic | Difficulty | What You'll Learn |
|---|-------|-----------|------------------|
| 1 | [Machine Learning in System Design](ml-in-system-design.md) | 🔴 Advanced | ML system architecture, feature store, recommendation and ranking, rollout, drift, model serving |
| 2 | [AI Agent System Design](ai-agent-system-design.md) | 🔴 Advanced | Agent anatomy, cognitive architectures, multi-agent patterns, function calling, observability, benchmarks |
| 3 | [Classic Machine Learning](classic-ml.md) | 🟡 Intermediate | Bias-variance, Naive Bayes, KNN, bagging vs boosting, SVM, PCA, SHAP/LIME, calibration |
| 4 | [Deep Learning](deep-learning.md) | 🟡 Intermediate | CNNs, LSTMs, Transformers, GANs, VAEs, diffusion, knowledge distillation, GQA/MQA |
| 5 | [LLM Interview Questions](llm-interviews.md) | 🔴 Advanced | Tokenization, RAG, LoRA, RLHF/DPO, MoE, scaling laws, multi-modal, CoT |

*These sections are actively maintained and optimized for interview prep rather than textbook completeness.*

---

## What to Study First

```
New to ML interviews:
  Classic ML → Deep Learning → LLM Interview Questions

System Design focus:
  Machine Learning in System Design → AI Agent System Design

Theory / modeling focus:
  Classic Machine Learning → Deep Learning → LLM Interview Questions
```

---

## Common Interview Questions by Topic

### ML in System Design
- "Design a recommendation system for Netflix."
- "How would you rank a home feed differently from search results?"
- "What metrics would you use for ranking quality offline and online?"
- "Design a fraud detection system for Stripe."
- "What is the role of a Feature Store? Why do we need it?"

### AI Agent Design
- "Design an autonomous coding assistant."
- "How does the ReAct pattern work?"
- "What are the core components of an AI agent's memory?"
- "How does function calling work under the hood?"
- "How would you evaluate an agent's performance?"
- "How do you handle cost control and model routing in a production agent?"

### Classic ML
- "Your model has 99% train accuracy and 70% test accuracy. What do you do?"
- "How do you handle a dataset where 1% of samples are positive?"
- "Explain XGBoost vs Random Forest — when would you use each?"
- "What is data leakage and how do you detect it?"
- "You have 50,000 features. Walk me through your feature selection strategy."
- "When would you use Naive Bayes vs Logistic Regression?"
- "How do you explain a model's predictions to a non-technical stakeholder?" (SHAP/LIME)
- "Your model has great AUC but bad real-world performance." (Calibration)

### Deep Learning
- "Why does ResNet solve the degradation problem?"
- "Why use He initialization with ReLU instead of Xavier?"
- "Explain the difference between BatchNorm and LayerNorm."
- "Walk me through the Transformer architecture from scratch."
- "Why is attention O(n²) and how do you deal with long sequences?"
- "Explain GANs. What are their main failure modes?"
- "What is knowledge distillation and when would you use it?"
- "What is GQA and why is it important for LLM inference?"

### LLMs
- "Explain how RAG works and when you'd prefer it over fine-tuning."
- "What is LoRA? Why does it work?"
- "How does the KV cache improve inference efficiency?"
- "Why does Chain-of-Thought sometimes fail?"
- "What is the 'lost in the middle' problem and how do you mitigate it?"
- "Compare RLHF and DPO — what are the practical differences?"
- "How does tokenization work? What is BPE?"
- "What is Mixture of Experts (MoE)? How does it scale models?"
- "What are scaling laws? What did Chinchilla show?"
- "How do multi-modal models process images alongside text?"

---

## Key Concepts Cheat Sheet

| Concept | One-line answer |
|---------|----------------|
| **Feature Store** | Prevents training-serving skew by providing the exact same features to both |
| **NDCG@K** | Measures whether the most relevant results appear near the top of a ranking |
| **Explore vs Exploit** | Balance immediate performance with learning new user preferences |
| **ReAct Agent** | Pattern where agent iterates through Reason → Act → Observe |
| **Bias vs Variance** | Bias = model too simple; Variance = model too sensitive to training data |
| **L1 vs L2** | L1 → sparsity (zeroes weights); L2 → shrinks weights evenly |
| **Bagging vs Boosting** | Bagging reduces variance (parallel); Boosting reduces bias (sequential) |
| **Data leakage** | Test information contaminating training; diagnose with suspiciously high accuracy |
| **Vanishing gradient** | Products of small derivatives → 0; fix with ReLU, skip connections, BatchNorm |
| **Xavier vs He init** | Xavier: for tanh/sigmoid (`2/(nᵢₙ+nₒᵤₜ)`); He: for ReLU (`2/nᵢₙ`) |
| **BatchNorm vs LayerNorm** | BN normalises over batch (CNNs); LN normalises over features (Transformers) |
| **Self-attention complexity** | O(n²·d) — quadratic in sequence length; FlashAttention for memory efficiency |
| **RAG vs Fine-tuning** | RAG for knowledge (updatable); Fine-tuning for behaviour/style (baked in) |
| **LoRA** | ΔW = B·A (low-rank); train only B and A; merge at inference, zero latency cost |
| **KV cache** | Cache K/V of past tokens; decode step only processes the new token |
| **Top-p sampling** | Adaptive nucleus — conservative when confident, exploratory when uncertain |
| **CoT failure** | Hallucinated reasoning, faithfulness gap, cascade errors, sycophancy |
| **Naive Bayes** | P(y\|x) ∝ P(y)·∏P(xᵢ\|y); strong text baseline; fast, high-dim friendly |
| **KNN** | Classify by majority vote of k nearest neighbors; no training; curse of dimensionality |
| **SHAP** | Shapley values for feature attribution; additive, consistent; TreeSHAP for tree models |
| **Calibration** | Predicted probabilities should match observed frequencies; Platt/temperature scaling |
| **GAN** | Generator vs Discriminator minimax game; mode collapse risk; diffusion models now preferred |
| **Knowledge Distillation** | Train small student to match large teacher's soft predictions; dark knowledge transfer |
| **GQA** | Group K/V heads to reduce KV cache by 4-8× with minimal quality loss; LLaMA-2 standard |
| **Tokenization (BPE)** | Subword merges by frequency; ~1.3 tokens/word English; numbers/multilingual are tricky |
| **MoE** | Route tokens to top-k of N expert FFNs; N× params, same compute; load balancing critical |
| **Scaling Laws** | Performance ∝ power law of N, D, C; Chinchilla: tokens ≈ 20× params for optimal |
| **Multi-modal** | Images → patches → visual tokens → LLM; CLIP for alignment; ~1000 tokens/image |
| **Function Calling** | LLM outputs JSON tool calls; app executes; result fed back as tool message |
| **Agent Observability** | Trace every LLM call, tool call, step; track tokens, latency, cost per request |
| **Model Routing** | Cheap model for simple tasks, expensive for complex; 70%+ cost savings |
