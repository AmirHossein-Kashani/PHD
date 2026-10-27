---
type: review
date: 2026-07-15
date-end: 2026-07-21
status: Done
projects: ["Claim-Level Conflict in Debate", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Multi-Agent Debate", "Autonomous Research Agents"]
tags: [type/review, project/claim-conflict, project/reversible-exploration, topic/multi-agent-debate, topic/autonomous-research-agents]
notion: https://app.notion.com/p/39fd68da32f080ecbaebd3f17b497a57
created: 2026-07-16
updated: 2026-08-12
---
# Review - Baselines for Gaps Protocol

> [!summary] In short
> Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.

**Project:** [[Claim-Level Conflict in Debate]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Multi-Agent Debate]], [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Sub-notes:** [[SMADE-IE]], [[MACI - Moderator with Two Dials]], [[ReConcile]], [[MoA - Mixture-of-Agents]], [[Du et al. - Multiagent Debate]], [[AlphaEvolve]], [[Biomni - General Biomedical Agent]], [[AIDE - ML Engineering as Code Search]], [[EviGraph - Evidence-Guided Research Agents]], [[Mage - Memory as Execution State]], [[Arbor - Hypothesis Tree Refinement]]  
**Original:** [Notion page](https://app.notion.com/p/39fd68da32f080ecbaebd3f17b497a57)

---

**Primary three**
1. **Du et al., Multiagent Debate** — non-negotiable. It's the canonical debate paper every reviewer will expect, it uses pure majority vote (the exact limitation you're attacking), the code is clean and minimal (github.com/composable-models/llm_multiagent_debate), and its biographies task gives you an open-ended testbed for free. This is also the easiest to build *on top of* — your claim extraction and manager can literally wrap around its debate loop, which matters since you said you want to code on top of a baseline.
1. **ReConcile** — the strongest answer-level aggregation among debate systems. Confidence-weighted voting is the most sophisticated version of "aggregate without looking at claims," so beating it makes your claim-level argument much more convincing than beating plain majority vote. Code is available (github.com/dinobby/ReConcile) and it covers StrategyQA/GSM8K/MATH, overlapping nicely with Du et al.
1. **MoA (Mixture-of-Agents)** — this is the one people might skip, but for *you* it's critical. On open-ended tasks, majority vote is undefined, and MoA's layered LLM aggregator is the state-of-the-art way to synthesize multiple agent outputs. Your claim-level composer competes directly with MoA's opaque synthesis — if you can show your composed answer has fewer inter-claim contradictions (D-FActScore-style) than MoA's at similar quality, that's your headline open-ended result. Code: github.com/togethercomputer/MoA.

**Backups**
1. **Liang et al., MAD (judge-based debate)** — swap this in if reviewers or your advisor want a judge-style aggregation baseline covered, or if ReConcile's multi-model setup (it originally used ChatGPT/Bard/Claude2) proves annoying to reproduce with current APIs. Judge-picks-winner is a distinct third aggregation philosophy, and your manager agent is arguably a smarter judge, so the comparison is natural.
1. **MAD-Fact** — your nearest full-system neighbor (claim decomposition + jury debate + judge). It's the most scientifically important comparison for novelty positioning, but I'm putting it as backup because code availability was unconfirmed and it's a recent preprint — you may end up reimplementing it, which is a cost. If the code turns out to exist and run, I'd actually promote it into the main three and demote ReConcile.

**Primary three**
1. **Du et al. — Improving Factuality and Reasoning in Language Models through Multiagent Debate**
    - Paper: [https://arxiv.org/abs/2305.14325](https://arxiv.org/abs/2305.14325)
    - Code: [https://github.com/composable-models/llm_multiagent_debate](https://github.com/composable-models/llm_multiagent_debate)
    - Project page: [https://composable-models.github.io/llm_debate/](https://composable-models.github.io/llm_debate/)
1. **Chen et al. — ReConcile: Round-Table Conference Improves Reasoning via Consensus among Diverse LLMs**
    - Paper: [https://arxiv.org/abs/2309.13007](https://arxiv.org/abs/2309.13007)
    - Code: [https://github.com/dinobby/ReConcile](https://github.com/dinobby/ReConcile)
1. **Wang et al. — Mixture-of-Agents (MoA) Enhances Large Language Model Capabilities**
    - Paper: [https://arxiv.org/abs/2406.04692](https://arxiv.org/abs/2406.04692)
    - Code: [https://github.com/togethercomputer/MoA](https://github.com/togethercomputer/MoA)

**Backups**
1. **Liang et al. — Encouraging Divergent Thinking in LLMs through Multi-Agent Debate (MAD)**
    - Paper: [https://arxiv.org/abs/2305.19118](https://arxiv.org/abs/2305.19118)
    - Code: [https://github.com/Skytliang/Multi-Agents-Debate](https://github.com/Skytliang/Multi-Agents-Debate)
1. **Ning et al. — MAD-Fact: A Multi-Agent Debate Framework for Long-Form Factuality Evaluation in LLMs**
    - Paper: [https://arxiv.org/abs/2510.22967](https://arxiv.org/abs/2510.22967)
    - Also published in Frontiers of Computer Science (DOI: 10.1007/s11704-025-51369-x)
    - Code: I checked and found no public repo linked from the paper — this confirms my earlier caution. If you go with MAD-Fact, budget for reimplementation. The architecture is straightforward to rebuild though: a Clerk agent that decomposes the response into atomic claims, a Jury of evaluator agents that assess each claim via retrieval and debate, and a Judge agent that aggregates per-claim verdicts into a final score.
