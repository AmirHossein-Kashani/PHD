---
type: review
date: 2026-06-12
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Evaluation and Metrics"]
tags: [type/review, project/claim-conflict, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/37dd68da32f0808d8e8ff90617ad6d4d
created: 2026-06-12
updated: 2026-06-16
---
# Review - LLM Evaluation Metrics

> [!summary] In short
> Table of metrics: accuracy, F1, Brier score, calibration, agreement and more, with notes on how to report them.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Evaluation and Metrics]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Original:** [Notion page](https://app.notion.com/p/37dd68da32f0808d8e8ff90617ad6d4d)

---

### Metrics

| **Metric family** | **Definition or formula** | **Why it matters** | **Recommended implementation note** |
|---|---|---|---|
| Accuracy / exact match | (\frac{1}{N}\sum_i \mathbf{1}[\hat y_i = y_i]) | Core closed-form score | Always report with 95% CI |
| Precision / Recall / F1 | Precision (=\frac{TP}{TP+FP}), Recall (=\frac{TP}{TP+FN}), (F1=\frac{2PR}{P+R}) | Needed for imbalanced labels, refusal/safety classes, answerable vs unanswerable splits | Use macro and micro variants where class imbalance matters |
| Task-native benchmark metric | e.g., exact match, token F1, pass@1/pass@k, win rate | Prevents “generic-score mismatch” across tasks | Always keep benchmark-native score as primary |
| Brier score | Binary: (\frac{1}{N}\sum_i (p_i-y_i)^2); multiclass: (\frac{1}{N}\sum_i\sum_c (p_{ic}-\mathbf{1}[y_i=c])^2) | Proper scoring rule for confidence quality | Lower is better |
| Expected Calibration Error | (ECE=\sum_{m=1}^{M}\frac{ | B_m | }{n}\lvert acc(B_m)-conf(B_m)\rvert) |
| Maximum Calibration Error | (MCE=\max_m \lvert acc(B_m)-conf(B_m)\rvert) | Captures worst-case bin miscalibration | Important for high-stakes use |
| Negative log likelihood | (NLL=-\sum_i \log \hat\pi(y_i | x_i)) | Penalizes overconfident errors strongly |
| Weighted-vote aggregation | (\hat y=\arg\max_c \sum_i w_i\mathbf{1}[\hat y_i=c]) | Baseline to test whether debate adds value beyond aggregation | Use calibrated (w_i), not raw self-reported confidence |
| Pairwise agreement | (\frac{2}{N_a(N_a-1)}\sum_{i&lt;j}\mathbf{1}[\hat y_i=\hat y_j]) | Measures cohesion and diversity collapse | High agreement is not always good |
| Vote entropy | (H(v)=-\sum_c p_c\log p_c) | Measures answer diversity before aggregation | Report pre- and post-debate |
| Reliability coefficients | Cohen’s (\kappa=\frac{p_o-p_e}{1-p_e}), plus Fleiss’ (\kappa) or Krippendorff’s (\alpha) | Distinguishes real agreement from chance agreement | Prefer Krippendorff’s (\alpha) when ratings may be missing or ordinal |
| Decision success rate | Share of items that reach a valid protocol-defined decision | Debate can fail to converge even when agents answer | Report alongside quality |
| Turns to convergence | Number of debate turns before decision or stop | Captures procedural efficiency | Report mean and distribution |
| End-to-end latency | p50 / p95 wall-clock per item | Production relevance | Separate generation time from aggregation time |
| Throughput | items/minute or successful decisions/minute | Measures service capacity | Report under fixed concurrency |
| Cost | total prompt + completion tokens; API or GPU cost per item | Main deployment trade-off in MAD | Normalize score by cost as well as by sample |
| Sample efficiency | score as a function of agents, rounds, and token budget | Tests whether extra compute is actually worthwhile | Report frontier or area-under-budget curve |
| Robustness to bad agents | score under noisy, random, sycophantic, or adversarial-agent injection | Consensus systems fail in distinctive ways | Vary adversarial fraction systematically |
| Drift rate | % of items harmed by additional debate rounds | Captures “talk made it worse” failure mode | Essential for long debates |
| Scalability | score-cost frontier as (N), (R), and topology vary | Many MAD systems do not scale linearly in value | Compare dense, sparse, and grouped topologies |
