---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: Nature 2026 (arXiv 2025)
link: https://arxiv.org/abs/2502.18864
code: no public code
ar_role: related work (hypothesis generation)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Multi-Agent Debate", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/multi-agent-debate, topic/biomedicine-and-drug-discovery]
created: 2026-10-27
updated: 2026-10-27
---
# Google AI Co-Scientist

> [!summary] In short
> Google's multi-agent system that generates, debates, ranks (Elo tournament) and evolves biomedical hypotheses. It has strong wet-lab results (AML drug repurposing). But there is no public code and the system does not run its own experiments, so it is related work, not a runnable baseline.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Multi-Agent Debate]], [[Biomedicine and Drug Discovery]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2502.18864](https://arxiv.org/abs/2502.18864) · [Nature 2026](https://doi.org/10.1038/s41586-026-10644-y)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page, the paper and the DeepMind / Nature pages. Not read in full by me yet.

> [!warning] Not the same as "Coscientist"
> *Coscientist* (Boiko et al., Nature 2023) is a different system: an LLM agent that plans and runs chemistry lab experiments.

---

## Paper

- **Title:** *Towards an AI co-scientist* (arXiv v1, Feb 2025). Renamed *Accelerating scientific discovery with Co-Scientist* (arXiv v2, Jun 2026).
- **Authors:** Juraj Gottweis et al. (Google, 51 authors).
- **Venue:** **Nature** 655, 487-496 (2026). doi:10.1038/s41586-026-10644-y.
- **Code:** not public (pseudocode and prompts are in the supplement). Access through the "Hypothesis Generation" tool in Gemini for Science and an enterprise preview.

## Problem

Help scientists find new, testable hypotheses that build on a large literature.

## Method (main idea)

- A **Supervisor** turns the scientist's goal into a plan and queues work for six agents: **Generation, Reflection, Ranking, Evolution, Proximity, Meta-review**.
- Generation uses literature search and simulated debate. Reflection reviews ideas.
- Ranking runs an **Elo tournament** of pairwise debates. Evolution refines and combines the best ideas.
- A persistent context memory stores the system state. Meta-review feedback is added to later prompts, so the system improves without fine-tuning (Gemini 2.0).

## Results

- On 15 research goals set by experts, experts preferred it on 11 (novelty 3.64/5, impact 3.09/5) over Gemini 2.0 and o1.
- AML drug repurposing: the new candidate KIRA6 had an IC50 of 10 nM in KG-1a cells.
- It recovered an unpublished antimicrobial-resistance mechanism (cf-PICI) in two days.

## Limits

- Uses open-access literature only. LLM hallucination.
- Lab validation is early (cell lines, organoids, no in-vivo tests).

## How it relates to my work

- **Similar to:** ideas that improve over time through tournament and evolution; feedback memory.
- **Different from my work because:** it does not test its own hypotheses and has no automatic score. Its memory is a feedback summary, not a decision path.
- **Can I use it as a baseline?** No (closed). Cite in related work. The Elo tournament could be a component for ranking ideas.
