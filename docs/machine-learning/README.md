# 🪩 Machine Learning Interview Prep

> Comprehensive ML interview coverage — from classic algorithms and deep learning foundations to modern LLMs and GenAI. These are now a core part of engineering interviews at AI-forward companies.

---

## Topics in This Category

| # | Topic | Difficulty | What You'll Learn |
|---|-------|-----------|------------------|
| 1 | [Classic Machine Learning](classic-ml.md) | 🟡 Intermediate | Bias-variance, bagging vs boosting, data leakage, XGBoost, SVM, PCA, class imbalance, missing values, high-dimensional features |
| 2 | [Deep Learning](deep-learning.md) | 🟡 Intermediate | Weight init (Xavier/He), backprop, Adam/AdamW, BatchNorm, ResNet, LSTMs, full Transformer deep-dive, FlashAttention, ViT |
| 3 | [LLM Interview Questions](llm-interviews.md) | 🔴 Advanced | Transformer architecture, RAG, LoRA/QLoRA, RLHF/DPO, decoding strategies, KV cache, CoT failure modes, hallucination mitigation |

*(These sections are fully completed with interview-ready content.)*

---

## What to Study First

```
New to ML interviews:
  Classic ML → Deep Learning → LLM Interview Questions

Strong ML background, weak on LLMs:
  LLM Interview Questions → Deep Learning (Transformer section)

Strong on LLMs, weak on classical:
  Classic ML (bias-variance, data leakage, ensembles)
```

---

## Common Interview Questions by Topic

### Classic ML
- "Your model has 99% train accuracy and 70% test accuracy. What do you do?"
- "How do you handle a dataset where 1% of samples are positive?"
- "Explain XGBoost vs Random Forest — when would you use each?"
- "What is data leakage and how do you detect it?"
- "You have 50,000 features. Walk me through your feature selection strategy."

### Deep Learning
- "Why does ResNet solve the degradation problem?"
- "Why use He initialization with ReLU instead of Xavier?"
- "Explain the difference between BatchNorm and LayerNorm."
- "Walk me through the Transformer architecture from scratch."
- "Why is attention O(n²) and how do you deal with long sequences?"

### LLMs
- "Explain how RAG works and when you'd prefer it over fine-tuning."
- "What is LoRA? Why does it work?"
- "How does the KV cache improve inference efficiency?"
- "Why does Chain-of-Thought sometimes fail?"
- "What is the 'lost in the middle' problem and how do you mitigate it?"
- "Compare RLHF and DPO — what are the practical differences?"

---

## Key Concepts Cheat Sheet

| Concept | One-line answer |
|---------|----------------|
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
