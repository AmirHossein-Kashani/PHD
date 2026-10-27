---
type: paper
date: 2026-07-16
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/3a0d68da32f0809fb536f39ff6adcd83
created: 2026-07-17
updated: 2026-07-21
---
# MACI - Moderator with Two Dials

> [!summary] In short
> A moderator controls debate with an information dial (evidence quality) and a behavior dial (how agents interact).

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3a0d68da32f0809fb536f39ff6adcd83)

---

This paper introduces **MACI** (Multi-Agent Collaborative Intelligence), a control framework for making multi-agent LLM debate more reliable, efficient, and measurable.
The problem it tackles: when you have multiple LLMs debate to reach a better answer, existing approaches waste compute. They either keep agents in a fixed adversarial stance the whole time, just aggregate votes without any real deliberation, or stop the debate based on arbitrary rules. This burns tokens and can lock in early mistakes.
MACI's core idea is an active "moderator" with **two independent dials**:
1. **Information dial (τ)** — controls *what evidence gets in*. Arguments are only admitted if their cited evidence meets a quality threshold, and this bar rises as the debate progresses, so agents must rely on increasingly better-supported claims.
1. **Behavior dial (CL)** — controls *how agents interact*. It schedules "contentiousness" from high to low: early rounds are exploratory (agents aggressively challenge each other, CL ≈ 0.9), later rounds are consolidative (agents build on agreements, CL ≈ 0.5 or lower).

The key insight is decoupling these — prior work like fixed-contentiousness debate only had one lever (behavior), and no one had combined it with independent evidence gating.
To know when to stop, the moderator tracks **four signals**: disagreement between agents (Jensen-Shannon divergence), overlap of the evidence they cite, evidence quality, and argument quality (scored by CRIT, a cross-family LLM judge). The debate halts when progress plateaus across these signals — a principled, measurable stopping rule rather than a fixed round count. The paper also provides "theory-lite" guarantees: dispersion between agents is non-increasing, and the controller provably terminates in O(1/ε) rounds (or O(log(1/ε)) under a contraction condition), plus a budget-aware bandit scheduler with a no-regret guarantee.
A neat extra feature: when the debate ends with residual uncertainty, MACI doesn't just output a confidence score — it produces a **precision RAG plan** specifying exactly what information to retrieve next (e.g., for a dengue diagnosis: travel history questions, CBC, NS1 antigen test).
Empirically, on clinical diagnosis (1,500 cases, 100+ possible diseases), MACI beat majority voting by +3.9 percentage points on accuracy, improved calibration (ECE 0.081 vs 0.103), and used 19% fewer tokens than fixed debate. On a news bias task (619 articles), the same setup with no re-tuning narrowed the partisan gap between opposing annotators by 68% — showing the controls transfer across domains.
In short: the paper reframes multi-agent debate from an ad hoc heuristic into a closed-loop, budget-aware controller with measurable convergence and provable termination.
