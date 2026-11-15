---
type: paper
date: 2026-08-12
status: Done
year: 2026
venue: arXiv preprint (2606.11926)
link: https://arxiv.org/abs/2606.11926
code: https://github.com/RUC-NLPIR/Arbor
projects: ["Auto Researcher", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, project/reversible-exploration, topic/autonomous-research-agents, topic/memory-and-long-context]
notion: https://app.notion.com/p/3bad68da32f080938129e762c1055a2c
created: 2026-08-12
updated: 2026-08-12
---
# Arbor - Hypothesis Tree Refinement

> [!summary] In short
> Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a git worktree. Our auto researcher is built on Arbor.

**Project:** [[AR 0 - Start Here|Auto Researcher]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3bad68da32f080938129e762c1055a2c)

---

**Hypothesis Tree Refinement (HTR):** Arbor stores its entire research state in a persistent tree. Each node binds together a hypothesis, the code that implements it, the experimental evidence it produced, and a distilled insight. The system splits work between:
- A **long-lived coordinator** that owns the tree — it proposes hypotheses, picks which to test, prunes falsified directions, and propagates lessons upward (so a leaf-level failure becomes a direction-level constraint).
- **Short-lived executors** that each test one hypothesis in an isolated git worktree and report back structured evidence.

![[arbor-hypothesis-tree-refinement-1.webp]]

**The repeating cycle.** From here the run loops until the budget (default 20 cycles) runs out, no promising frontier remains, or a stop signal fires:
1. **Observe** — reload the tree from disk: frontier, insights, pruned lessons, current best.
1. **Ideate** — pick any parent (root, an old direction, a half-successful node) and attach new pending children, constrained by what's been validated and ruled out.
1. **Select** — choose 2–4 pending leaves for the next batch, balancing expected impact, cost, and informativeness.
1. **Dispatch** — each selected leaf goes to an isolated executor: fresh git worktree branched from *current trunk*, given only its fixed hypothesis + ancestor insights + eval command. It implements, debugs, runs the dev evaluator, and returns four things: score, factual result, distilled insight, branch reference.
1. **Backpropagate** — write results into the leaf nodes; walk up each path to the root, re-synthesizing every ancestor's insight from its children's insights. Local outcomes become direction-level lessons and global priors.
1. **Decide** — prune falsified subtrees (with reasons, kept as evidence); if a candidate beats the current best on dev by ≥ the threshold, `GitMergeBranch` re-runs the *held-out* evaluator in a fresh worktree and merges into trunk only if the test score strictly improves. Otherwise the branch stays in the tree as evidence, unmerged.

## Paper and code (added 27 Oct 2026)

> [!note] Added by Claude on 2026-10-27 from the arXiv page and the GitHub README.

- **Title:** *Toward Generalist Autonomous Research via Hypothesis-Tree Refinement.*
- **Authors:** Jiajie Jin, Yuyang Hu, Kai Qiu, ..., Zhicheng Dou (Renmin University of China and Microsoft Research), arXiv, 10 Jun 2026.
- **Links:** [arXiv 2606.11926](https://arxiv.org/abs/2606.11926) · [GitHub RUC-NLPIR/Arbor](https://github.com/RUC-NLPIR/Arbor) (Apache-2.0, `pip install arbor-agent`, about 1.1k stars on 27 Oct 2026).
- **Evaluation:** six tasks (optimizer design, architecture design, Terminal-Bench 2.0, BrowseComp, two data-synthesis tasks); 48 h, 20 cycles, tree depth up to 2. Best held-out result on all 6, with more than 2.5x the average relative gain of Codex and Claude Code. BrowseComp 45.33 to 67.67 (Claude Code: 53.33). MLE-bench Lite: 86.36% any medal with GPT-5.5.
- **Limits (authors):** one scalar metric only; mostly local refinements, not new mechanisms; 20-43M tokens per run.

> [!warning] Name collision
> AMD's *Arbor: Tree Search as a Cognition Layer for Autonomous Agents* ([arXiv 2606.12563](https://arxiv.org/abs/2606.12563)) is a different system. Say which Arbor in the paper.

See also: [[AR 4 - Related Work and Baselines#3. Which baselines to run|recent baselines]].
