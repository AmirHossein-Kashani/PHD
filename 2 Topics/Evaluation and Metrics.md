---
type: topic
aliases: [metrics, evaluation, statistics]
tags: [type/topic, topic/evaluation-and-metrics]
---
# Evaluation and Metrics

> [!summary] In short
> How we measure results. Always report the **benchmark's own score** first, add **confidence intervals**, and be careful with **LLM judges** (they share bias with the model they judge). With small samples, effect sizes and intervals matter more than p-values.

## Metrics used in my work

| Where | Metrics |
|---|---|
| [[Multi-Source Evidence RAG]] | refs per question, on-topic %, grounded %, ClinPGx-valid %, false-premise test, calibration (Spearman between confidence and label), contradiction precision / recall / F1 |
| [[Creativity in Multi-Agent Debate]] | round-1 agent diversity (cosine, idea-level Jaccard), composite creativity, judge novelty / usefulness / flexibility, rare ideas, usable novelty, flagged implausible ideas, gold recall and rank; Cliff's d, Wilcoxon, bootstrap CI, Spearman |
| [[Auto Researcher]] | leaderboard score, speed to a target score, hours, merged cycles, tree depth, rank |

## Good practice (from the notes)

- Use at least **3 repeats** per setting; 1 repeat mixes noise with the effect.
- Use a **second judge** from another model family.
- Keep **usefulness and safety numbers next to** the creativity score.
- Report **Brier score / calibration** for confidence, not only accuracy ([[Review - LLM Evaluation Metrics]]).

## Notes on this topic

### Reports and experiments (7)

- [[AMG-RAG Phase 11 - Method Note]] - Full method note of Phase 11: knowledge sources, 6-stage pipeline, evidence ledger with rule-based confidence, contradiction flags, results and limitations.
- [[AMG-RAG Phases Comparison Tables]] - All comparison tables: configuration matrix, headline metrics, retrieval precision, search shape, ledger results, calibration and cost.
- [[AMG-RAG Phases Report (Scores)]] - Phase catalogue, scores (on-topic %, grounded %, ClinPGx-valid %, false premise), search shape and genes found per phase.
- [[Creativity Exp 1 - Index and Conclusion]] - Index of both creativity experiments and the full conclusion of Exp 1 (masking and shuffling of context between debating agents).
- [[Creativity Exp 1 - Full Results]] - Full statistics of Exp 1. Masking raised agent diversity; shuffling did nothing; plain debate was worse than one agent.
- [[Creativity Exp 2 - Conclusion]] - Conclusion of Exp 2 (asymmetry levers). Persona works best; representation is a null; the new aggregator helped creativity but lost the correct diagnosis.
- [[Creativity Exp 2 - Full Results]] - Full statistics of Exp 2 (persona, masking, representation, aggregator bottleneck, medical safety).

### Papers (1)

- [[MAD for LLM Judges (Adaptive Stability)]] - Several LLM judges debate and stop when their answers become stable (adaptive stopping).

### Literature reviews and lists (1)

- [[Review - LLM Evaluation Metrics]] - Table of metrics: accuracy, F1, Brier score, calibration, agreement and more, with notes on how to report them.

