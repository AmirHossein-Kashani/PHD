---
type: report
date: 2026-07-08
status: Not started
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate", "Neuro-Symbolic Reasoning and Argumentation"]
notion-tags: ["PapersList"]
tags: [type/report, project/claim-conflict, topic/multi-agent-debate, topic/neuro-symbolic-reasoning-and-argumentation]
notion: https://app.notion.com/p/397d68da32f0804b8786d07c54136784
created: 2026-07-08
updated: 2026-07-15
---
# Suggested Papers for Gaps Protocol

> [!summary] In short
> Reading list in tiers A to E for the claim-conflict proposal (SMADE-IE, MACI, MAD-Fact, DebateCV, argumentation theory, baselines).

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]], [[Neuro-Symbolic Reasoning and Argumentation]]  
**From meeting:** [[Claim-Level Conflict in Debate#2026-07-07 One-on-One|2026-07-07 One-on-One]]  
**Original:** [Notion page](https://app.notion.com/p/397d68da32f0804b8786d07c54136784)

---

#### Tier A — Read first (closest to your protocol; you must differentiate from these)
1. **SMADE-IE: Sparse Multi-Agent Framework with Evidence-Driven Debate for Zero-Shot Information Extraction** (arXiv: 2606.04691) — per-conflict Toulmin-structured debates, external evidence scorer, Beta-update aggregation. The nearest structural cousin to your pipeline.
1. **MACI: Multi-Agent Collaborative Intelligence — Dual-Dial Control for Reliable LLM Reasoning** (Chang & Chang, arXiv: 2510.04488) — moderator gates information flow by quality and tracks disagreement; closest to your referee-controls-data-flow idea.
1. **Kaesberg et al., "Voting or Consensus? Decision-Making in Multi-Agent Debate"** (ACL Findings 2025, arXiv: 2502.19130) — the systematic protocol comparison; your G1 baseline landscape and the "protocol choice is task-dependent" finding you'll generalize to claim-type-dependent.
1. **MAD-Fact: A Multi-Agent Debate Framework for Long-Form Factuality Evaluation** (arXiv: 2510.22967) — Clerk decomposes into atomic claims, Jury debates each claim; the existing per-claim-debate machinery.
1. **DebateCV: Debate-driven Claim Verification** (arXiv: 2507.19090) — Debaters + Moderator; crucially documents the zero-shot moderator neutrality bias and fixes it with post-training. Directly relevant to designing your referee.

#### Tier B — Core for your gap statements (G1–G3 motivation and framing)
1. **"Multi-agent decision making: A Blackwell's informativeness approach"** (arXiv: 2605.06028) — formal proof that voting/debate lose information relative to agents' joint private information; your strongest theoretical motivation for claim-level aggregation.
1. **Ai et al., "Beyond Majority Voting: LLM Aggregation by Leveraging Higher-Order Information"** (arXiv: 2510.01499) — OW and ISP algorithms; the state of the art in answer-level aggregation you want to move beyond.
1. **DRAGged into Conflicts: Detecting and Addressing Conflicting Sources in Search-Augmented LLMs** (arXiv: 2506.08500) — the conflict-type taxonomy + benchmark for RAG contexts; the template for your G2 in the debate setting. Also chase its citations: ConflictingQA, DebateQA, and the Xu et al. knowledge-conflicts survey.
1. **"Consensus is Strategically Insufficient: Reasoning-Trace Disagreement as a Knowledge-Representation Signal"** (arXiv: 2606.04223) — the categorize-disagreement-then-route thesis in miniature.
1. **Contradiction Detection in RAG Systems: Evaluating LLMs as Context Validators** (arXiv: 2504.00180) — documents why NLI and even strong LLMs fail at multi-document contradiction detection; your G3 evidence base. Pair with **ContraDoc** (Li et al., 2023) and **LegalWiz** (arXiv: 2510.03418) for its six-type contradiction taxonomy.

#### Tier C — Protocol & flow-control design space
1. **ChatEval** (Chan et al., 2023) — origin of the One-By-One / Simultaneous-Talk / Simultaneous-Talk-with-Summarizer communication strategies; canonical citation.
1. **Improving Multi-Agent Debate with Sparse Communication Topology** (Li et al., EMNLP 2024) — who-talks-to-whom as an efficiency lever; then **DySCo** (arXiv: 2606.01828) and **"Hear Both Sides" / DAR** (arXiv: 2603.20640) for dynamic, message-level flow control.
1. **"How susceptible are LLMs to Logical Fallacies?"** (arXiv: 2308.09853) — master moderator delegating to subordinate moderators; a working example of hierarchical referee design.
1. **Token-Level Round-Robin Collaboration ("The Consensus Trap")** (arXiv: 2604.17139) — the argument that answer-level voting is blind to intermediate logic, plus a robustness analysis under adversarial majorities.
1. **"Can LLM Agents Really Debate? A Controlled Study"** (arXiv: 2511.07784) — the per-sub-issue debate loop with revision phases, and a good literature paragraph covering judge-based, consensus-based, heterogeneity, and confidence-reporting protocols.
1. **Conch: Competitive Debate Analysis via Visualizing Clash Points** (arXiv: 2507.14482) — clash-point/disagreement extraction pipeline; methodological template for your detection stage.

#### Tier D — Formal argumentation backbone (for the conflict graph & scoring)
1. **Dung (1995)** — the abstract argumentation framework; unavoidable foundational citation.
1. **ArgLLM** (Freedman et al.) and the **QBAF/gradual-semantics line** (ArgRAG; Görür et al. forecasting) — LLM-generated argument graphs with quantitative scoring; your most principled scoring option.
1. **ARGORA** (arXiv: 2601.21533) — explicitly criticizes MAD for lacking formal debate representations and targeted interventions on arguments; useful both as related work and as a phrasing source for your gap.
1. **Chen & Endriss, "Preservation of Semantic Properties during Aggregation of Abstract Argumentation Frameworks"** (arXiv: 1707.08740) and **"Judgment Aggregation in Multi-Agent Argumentation"** (arXiv: 1405.6509) — pre-LLM claim-level social choice; citing these will make your related work look far deeper than typical MAD papers.
1. **ArgRE: Formal Argumentation for Conflict Resolution in Multi-Agent Requirements Negotiation** (arXiv: 2604.23124) — Dung-style conflict resolution in a multi-agent negotiation setting.

#### Tier E — Baselines, surveys, and context (skim)
1. **Du et al. (2023), "Improving Factuality and Reasoning through Multiagent Debate"** — the original MAD paper; mandatory baseline.
1. **RECONCILE** (Chen et al., 2023) — confidence-weighted voting until consensus; a strong baseline for G1 experiments.
1. **"Literature Review of Multi-Agent Debate for Problem-Solving"** (arXiv: 2506.00066) — survey covering topology, decision-making (majority vote / judge / consensus), and terminology confusion around consensus vs voting.
1. **"Debate or Vote: Which Yields Better Decisions?"** (arXiv: 2508.17536) — recent head-to-head comparison; its bibliography is a goldmine (CIPHER embeddings-based debate, AgentPrune/"Cut the Crap" communication economics, diversity-in-debate work).
1. **"An Electoral Approach to Diversify LLM-based Multi-Agent Collective Decision-Making"** (arXiv: 2410.15168) — social choice theory applied to LLM aggregation; frames the voting design space.
1. **FIPA-ACL / speech-act agent communication languages** (via any classical MAS reference, or the agentic-AI textbook chapter at arXiv: 2606.24937 §24.4) — the historical precedent for typed/tagged messages as flow control, which your referee-assigned tags modernize.
