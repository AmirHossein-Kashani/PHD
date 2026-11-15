---
type: paper
date: 2026-08-05
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/reversible-exploration, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3b3d68da32f0801aa190d00395ec908e
created: 2026-08-05
updated: 2026-08-05
---
# BioML-bench - Drug Discovery Tasks

> [!summary] In short
> The 9 drug discovery tasks of BioML-bench (potency, ADME, toxicity) from Polaris and TDC.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[BioML-bench]]  
**Original:** [Notion page](https://app.notion.com/p/3b3d68da32f0801aa190d00395ec908e)

---

### The 9 drug discovery tasks
All are sourced from PolarisHub (which also mirrors Therapeutics Data Commons datasets), and all take molecular structure — typically SMILES strings — as input.
**Potency / target activity**
- `polaris-pkis2-egfr-wt-c-1` — classify compounds as EGFR wild-type kinase inhibitors (>80% inhibition), using a scaffold-based split, framed around glioblastoma drug discovery

**ADME (absorption, distribution, metabolism, excretion)**
- `polaris-adme-fang-hclint-1` — predict human liver microsomal intrinsic clearance from SMILES (Fang et al. 2023, 2,229 compounds)
- `polaris-adme-fang-hppb-1` — predict fraction unbound in human plasma protein binding (2,218 compounds)
- `polaris-adme-fang-solu-1` — predict solubility from SMILES under standardized ADME protocols (1,578 compounds)
- `tdcommons-lipophilicity` — predict logP from SMILES (AstraZeneca dataset)
- `tdcommons-caco2-wang` — predict intestinal permeability in the Caco-2 cell assay
- `tdcommons-bbb-martins` — classify blood–brain barrier penetration, for CNS drug development
- `tdcommons-cyp2d6-substrate` — classify CYP2D6 enzyme substrates, for drug–drug interaction risk

**Toxicity**
- `tdcommons-herg` — classify hERG potassium channel blockers to assess cardiotoxicity risk

So the set is roughly one potency task, seven ADME/physchem tasks, and one safety/tox task — a small-molecule property-prediction slice rather than generative design or docking.

#### How they're evaluated
Each task is packaged as a "task capsule": a structured natural-language task description, training data, test features, and a sample submission file in valid format. The agent runs in a Docker container with a pre-built conda environment, must design and fit its own models, and emit predictions that are automatically validated and graded.
Grading has two layers:
1. **Raw metric** — the task's original metric (e.g. AUROC for the classification tasks, Spearman for the regression ones).
1. **Human-relative scoring** — the score is mapped to a percentile on the task's public leaderboard, which for these sources is largely expert-populated. Runs producing no valid submission are either scored as percentile zero ("penalized") or dropped ("non-penalized").

Reported aggregate metrics are: leaderboard percentile, mean rank across agents, above-median % (tasks beating the median human submission), any-medal % (using MLE-bench's medal thresholds), and completion rate. Four independent replicates per agent–task pair; drug discovery tasks run CPU-only with an 8-hour wall-clock limit.

#### Baselines
Two biomedical specialist agents (Biomni v0.0.5 on Claude Sonnet 4, STELLA on the OpenRouter default), two generalist ML agents (AIDE v6.3.3 and MLAgentBench, both on GPT-4.1), plus a "Dummy" agent emitting random or zero predictions. The human leaderboards themselves are the reference baseline.
Drug discovery results, penalized (leaderboard percentile / above-median % / completion %):

| Agent | Percentile | Above median | Completion |
|---|---|---|---|
| Biomni | 32.55 ± 8.61 | 30.6% | 100% |
| AIDE | 24.75 ± 7.23 | 25.0% | 80.6% |
| MLAgentBench | 22.45 ± 5.93 | 16.7% | 100% |
| STELLA | 12.34 ± 6.62 | 11.1% | 91.7% |
| Dummy | 1.11 ± 1.11 | 0% | — |

Biomni leads drug discovery in both the penalized and non-penalized settings; AIDE improves to 31.06 once failed runs are excluded. Two findings worth carrying over if you're building on this: the best agent runs relied on classical ML — feature engineering, feature selection, hyperparameter tuning, stacking — while deep learning was rarely attempted and almost never chosen as the final model, in contrast to the human leaderboards which are dominated by DL. And most failures were scaffolding problems (silent try–except errors that never saved predictions, uncaught framework exceptions, resource exhaustion, timeouts) rather than genuine capability limits.
