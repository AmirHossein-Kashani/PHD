---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2608.14354)
link: https://arxiv.org/abs/2608.14354
code: https://github.com/science-learner/ScienceFlow
ar_role: must cite (closest to "reopen old ideas"); possible baseline
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-10-27
updated: 2026-10-27
---
# ScienceFlow - Re-Anchoring Long-Horizon Agent

> [!summary] In short
> Splits long research into segments tied to executable workspaces. At each step a worker can continue from the live state or **jump back to an archived checkpoint**, then extend or redirect (ESTRA, "re-anchoring"). 70.22% any medal on the full MLE-bench (24 h). The most dangerous overlap with our "reopen old ideas" claim.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2608.14354](https://arxiv.org/abs/2608.14354) · [GitHub](https://github.com/science-learner/ScienceFlow)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv abstract and the GitHub README (source code not inspected). Found as an extra system (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *ScienceFlow: A Long-horizon Agent for ML Research, Scientific Discovery and Beyond.*
- **Authors:** Mingming Zhao, Jiqian Dong, Kangping Xu, ..., Yanhui Geng (Huawei Noah's Ark), arXiv, 14 Aug 2026.
- **Code:** MIT license.

## Method (main idea)

- Work is split into **research segments**, each tied to an executable workspace, so progress can be recovered, revised and re-run.
- **ESTRA (Executable-State Transition through Re-Anchoring):** at each segment boundary, choose the anchor (live state or an archived checkpoint) and choose to extend or redirect.
- Each **checkpoint** bundles a workspace snapshot, structured memory, validation evidence and resource records.
- Memory operations: Add, Fold, Unfold, Assemble. Parallel workers exchange short summaries.
- An evidence-aware controller gives compute based on budget and validated progress.

## Results

- Full MLE-bench (24 h): **70.22 ± 1.18% any medal** (+4.92 points over the best earlier result).
- MLE-bench Lite: 80.30%, down to 66.67% without ESTRA.
- Also scientific modelling and math optimization tasks, including a ten-day campaign.

## Limits

- I only have the abstract and the README; how the anchor is chosen needs reading.

## How it relates to my work

- **Similar to:** going back to an archived state together with its evidence is very close to our path to the root plus reopening.
- **Different from my work because (what we must show):** checkpoints keep state and evidence, but not an **undo condition** that says *when* to go back. The anchor is chosen by the worker each segment.
- **Can I use it as a baseline?** Maybe (open code). If reopening is a headline claim, we must compare with it or explain why not.
