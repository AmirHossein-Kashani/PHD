---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: Nature 2026 (arXiv 2025)
link: https://arxiv.org/abs/2505.13400
code: https://github.com/Future-House/robin
ar_role: biomedical baseline candidate; related work
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/biomedicine-and-drug-discovery]
created: 2026-10-27
updated: 2026-10-27
---
# Robin - Drug Repurposing Agent

> [!summary] In short
> FutureHouse's multi-agent system for drug repurposing. Literature agents propose mechanisms and drugs, a tournament ranks them, humans run the lab tests, and a data-analysis agent reads the results to plan the next round. It found ripasudil as a candidate for dry AMD.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Biomedicine and Drug Discovery]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2505.13400](https://arxiv.org/abs/2505.13400) · [Nature 2026](https://doi.org/10.1038/s41586-026-10652-y) · [GitHub](https://github.com/Future-House/robin)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page, the Nature abstract and the GitHub README. Not read in full by me yet.

---

## Paper

- **Title:** *Robin: A multi-agent system for automating scientific discovery* (arXiv, May 2025). Nature title: *A multi-agent system for automating scientific discovery*.
- **Authors:** Ali E. Ghareeb et al. (FutureHouse).
- **Venue:** **Nature** 655, 497-505 (2026). doi:10.1038/s41586-026-10652-y.
- **Code:** Apache-2.0, but the literature and data-analysis agents run on the **paid Edison platform** (API key and credits). Hypothesis generation alone works without it. About 716 GitHub stars.

## Problem

Automate the loop of a drug discovery project: read the literature, propose drugs, analyse lab data, and plan the next experiment.

## Method (main idea)

- Agents: **Crow** and **Falcon** (literature agents built on PaperQA2) and **Finch** (writes and runs Jupyter notebooks for data analysis).
- **Generation:** for dry AMD it read 151 papers, proposed 10 mechanisms with matching assays, and ranked them in pairwise LLM tournaments (Claude 3.7 Sonnet as judge).
- **Selection:** then 30 drug candidates, evaluated by Falcon and ranked the same way.
- **Testing:** humans ran the lab experiments; Finch analysed the data (10 runs plus a consensus).
- **Revision:** the results seeded the next round (an RNA-seq experiment, then a second set of candidates).

## Results

- Ripasudil increased RPE phagocytosis. Finch reported 7.5x vs control; a human analysis of the same data found 1.75x.
- RNA-seq after Y-27632 showed ABCA1 up about 3x.
- The LLM judge agreed with experts on 7.25 of their top-10 picks on average.
- The Nature version adds KL001 as a second confirmed in-vitro hit.

## Limits

- Humans did all the lab work; Robin does not write executable protocols.
- Finch's numbers were different from the human analysis.
- In-vitro results only; one disease studied in depth.

## How it relates to my work

- **Similar to:** rounds of hypothesis, test, and revision; a drug repurposing task, like the benchmark ideas in [[AR 3 - Benchmarks]].
- **Different from my work because:** one case study, no automatic score, humans in the lab, and the state between rounds is reports and rankings, not a decision path.
- **Can I use it as a baseline?** Only on a biomedical hypothesis task (for example TRACES DRR or "DrugMechDB inverted"), and it needs paid Edison credits.
