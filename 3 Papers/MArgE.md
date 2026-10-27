---
type: paper
date: 2026-06-27
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Neuro-Symbolic Reasoning and Argumentation", "Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/neuro-symbolic-reasoning-and-argumentation, topic/multi-agent-debate]
notion: https://app.notion.com/p/38ed68da32f080c99a6dcdfd6a346c99
created: 2026-06-29
updated: 2026-06-30
---
# MArgE

> [!summary] In short
> Debate version of ArgLLM: LLMs build pro/con argument trees that are merged and scored. Only binary decisions.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Neuro-Symbolic Reasoning and Argumentation]], [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Neuro-Symbolic Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/38ed68da32f080c99a6dcdfd6a346c99)

---

This paper is the debate version of [[ArgLLM]] , but the main limitation of this approach is that the architecture currently only supports binary decisions. This is partly because the binary setting is simpler — all LLM outputs can be organized under a single claim that is either supported or attacked, making the tree structure straightforward to construct and evaluate.
  
  

![[marge-1.webp]]

**MArgE: Meshing Argumentative Evidence from Multiple LLMs (Preprint 2025)**
**Core Problem**
Multi-LLM approaches (debate, voting) improve accuracy but remain **unjustifiable** — you can't trace *why* a final decision was made. CoT outputs also suffer from hallucination and may not faithfully reflect true internal reasoning.
**Key Idea**
Instead of letting LLMs freely debate or vote, force each LLM to produce **structured pro/con argument trees**, merge them into a single formal argumentation framework, and let **mathematical semantics** (not another LLM) decide the final answer.
**Four-Step Pipeline**
1. **Argument Tree Generation** — Each LLM independently generates short supporting/attacking arguments for a claim, forming a Bipolar Argumentation Framework tree. No free-form debate, just structured Agree/Disagree stances.
1. **Meshing** — Trees from all LLMs merged into one via simple union or semantic similarity merging (Sentence-BERT deduplicates near-identical arguments across models).
1. **Quality Scoring** — An external LLM (GPT-4o-mini) scores each argument node on relevance, factuality, and ambiguity as a scalar in [0,1]. Separate from the generators to reduce systematic bias, acting like a reward model.
1. **Gradual Semantics Propagation** — DF-QuAD mathematically propagates argument strengths bottom-up through the tree, updating the root claim's score. Final prediction: True if score ≥ 0.5. No LLM judge involved.

**The Justifiability Argument**
The entire decision path is inspectable — you can trace exactly which arguments contributed what strength to the final score. Hallucinated arguments receive low quality scores and naturally lose influence, unlike debate (opaque LLM judge) or CoT (unfaithful to internal reasoning). Also more robust to adversarial agents since a bad argument simply gets scored low rather than derailing the whole debate.
**Key Results**
- Outperforms single LLMs, majority vote ensembles, ensemble of ArgLLMs, and LLM debate across 3 datasets
- Beats GPT-4o-mini's own CoT on 2 of 3 datasets using only small open-source models (4B–8B) for argument generation
- More stable than debate — debate is highly sensitive to ordering (who defends/attacks first); MArgE variance is ~1%
- Most impactful design choice: letting the scorer also evaluate the root claim itself adds ~13% accuracy

**Relation to MAD-Logic**
Both use structured multi-agent reasoning but with opposing philosophies. MAD-Logic uses iterative free debate to refine answers, with majority vote as aggregator. MArgE **eliminates free debate entirely** — agents never communicate with each other, and aggregation is done by formal mathematical semantics. MArgE prioritizes **justifiability**; MAD-Logic prioritizes **reasoning accuracy through refinement**.
**Critical Limitation: Binary Only**
The framework is currently restricted to binary True/False decisions, and this is architectural rather than incidental. Three layers of the pipeline enforce this:
- Argument generation is hardcoded to Agree/Disagree stances
- The quality scorer maps claims to a single [0,1] truthfulness interval
- The QBAF has one root node (the claim) with a single threshold at 0.5

Extending to multi-class would require rethinking the tree topology entirely — you'd need either multiple competing root nodes or a fundamentally different graph structure, neither of which QBAF natively supports. Notably this is a limitation MAD-Logic doesn't share, since majority vote over labeled classes naturally extends to k classes, which is why their theoretical analysis covers the general multiclass case (k ≥ 2) from the start. The authors acknowledge multi-class as future work.
**Key Takeaway for Your Notes**
The core insight is replacing the "LLM as judge" bottleneck with **computational argumentation semantics** as a transparent, deterministic aggregator. LLMs contribute evidence; the math decides. This makes the framework more interpretable and less vulnerable to adversarial agents or echo chambers — but at the cost of flexibility, since the formal structure currently only supports binary outcomes.
