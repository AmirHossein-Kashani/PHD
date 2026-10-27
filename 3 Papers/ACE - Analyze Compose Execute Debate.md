---
type: paper
date: 2026-06-13
date-end: 2026-06-17
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/37ed68da32f080b1b260f7a8fc859a6d
created: 2026-06-13
updated: 2026-06-17
---
# ACE - Analyze Compose Execute Debate

> [!summary] In short
> Dynamic debate: agents choose the next discussion act (sense-plan-act) instead of a fixed number of rounds.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - State of the Art Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/37ed68da32f080b1b260f7a8fc859a6d)

---

![[ace-analyze-compose-execute-debate-1.webp]]

the output of composer will be used in the next step 

### What they did
**Problem they target:** Existing MAD methods (multiple LLM agents iteratively discussing to solve a problem) lock agents into a *fixed* discussion pipeline — they repeat the same discussion act for a predetermined number of rounds. The authors categorize prior work into two paradigms:
- **Convergent debate** — agents revise answers by absorbing peers' reasoning, aiming for consensus. Failure mode: agents over-reference each other and get stuck in "response swapping" loops that never converge.
- **Competition debate** — agents defend their own answers and mutually refute. Failure mode: if everyone starts wrong, mutual defense traps the discussion in invalid reasoning and forces an incorrect final answer.

**Their solution:** Inspired by the **Sense-Plan-Act** robotics paradigm (Brooks 1986), ACE lets agents dynamically choose discussion strategies based on dialogue context instead of following a rigid script. Three modules:
1. **Analyze** — splits agents into *debaters*   and an *observer* $A_o$. Debaters produce answers + itemized explanations; the observer compares them pairwise to extract correlation signals: answer agreement $\delta_a$ and explanation similarity $\delta_E$, producing sets of similar $S$ and dissimilar $D$ explanation entries. The observer also decides whether to terminate the discussion.
1. **Compose** — they build the **Atomic Discussion Act Library (ADAL)** along two axes. *Self-oriented acts*: **Adapt** (update using peers' input), **Revise** (update independently), **Retain** (keep answer). *Peer-oriented acts*: **Comment** (give feedback), **Withhold** (stay silent). Agents select and combine atomic acts into a discussion action, conditioned on the auxiliary info from Analyze. (E.g., high agreement → adapt + withhold; strong conflict → revise or comment.) The compositional scheme exists to keep dynamic behavior controllable given LLM hallucination.
1. **Execute** — agents run the composed action, emitting answer/explanation/comments in JSON. Loop repeats until the observer terminates.

### Datasets
**Primary:** **BIG-Bench Hard (BBH)** — 23 challenging tasks, grouped here into Mathematical Reasoning, Commonsense Understanding, and Scenario-based QA.
**Supplementary:** **MMLU** (factual knowledge, accuracy), **StrategyQA** (logical reasoning, accuracy), and **Commonsense Machine Translation (Common MT)** (ambiguous translation, COMET metric).

### Baselines compared
Both paradigms are represented — four baselines plus ACE:
- **PAR** (Peer-Aware Revision, Du et al. 2023) — convergent, independent answers then mutual revision.
- **RPD** (Role-Playing Debate / ChatEval, Chan et al. 2023) — convergent, assigns personas (e.g. "scientist") for diverse perspectives.
- **RTC** (Round Table Conference / ReConcile, Chen et al. 2023) — convergent, heterogeneous models (ChatGPT, Bard, Claude) with confidence-weighted aggregation.
- **TAR** (Triadic Adversarial Reasoning / Liang et al. 2023) — the only competition/adversarial baseline; rounds of rebuttal then a judge picks the answer.

Main experiments use **GPT-3.5-turbo**, 3 agents (2 debaters + 1 observer), zero-shot, temperature 0, no CoT, baselines fixed at 4 rounds. ACE wins **17/23** BBH tasks with **+8.5%** average gain. Biggest jump is Multi-Step Arithmetic (+29.7% over the best baseline). It also tops all three supplementary datasets.
Ablations (on TSO, DU, PIA, GS) show removing **Compose** hurts most (random atomic-act selection), Analyze second (~10% drop on PIA), Execute least. A rounds-sensitivity study argues fixed-round baselines are unstable across tasks — motivating ACE's self-terminating design. Cross-model transfer is shown on **GPT-4.0** and **LLaMA-2-13B**, where ACE remains best within each backbone (and GPT-3.5 ACE sometimes matches GPT-4.0 baselines).
One caveat worth flagging as a reader: the whole thing rests on GPT-3.5-turbo (with smaller GPT-4/LLaMA checks), and the observer's similarity judgments ($sim(\cdot,\cdot)$) are themselves LLM-produced, so reliability of the Analyze signals isn't independently validated. Want me to dig into any module's mechanics or the experimental setup more closely?
