---
type: paper
date: 2026-08-03
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/reversible-exploration, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3b1d68da32f08018a104f3efc13a929a
created: 2026-08-03
updated: 2026-08-05
---
# BioML-bench

> [!summary] In short
> 24 end-to-end biomedical ML tasks (drug discovery, imaging, protein engineering, single-cell). Tests full ML pipelines by agents.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Benchmarks for MoA and Multi-Agent Systems]]  
**Sub-notes:** [[BioML-bench - Drug Discovery Tasks]]  
**Original:** [Notion page](https://app.notion.com/p/3b1d68da32f08018a104f3efc13a929a)

---

### BioML-bench — Brief
**The question.** Existing biomedical agent evaluations test question answering or narrow bioinformatics analyses. Nobody had checked whether LLM agents can run a *complete* ML pipeline — read a task description, build a data pipeline, implement and train models, and submit predictions that get graded against held-out truth. BioML-bench is the first benchmark to test that in biomedical domains.
**What it contains.** 24 tasks across four domains, adapted from MLE-bench's scaffold but extended for biology (new metrics, new agents, support for formats like h5ad):

| Domain | Tasks | Source |
|---|---|---|
| Drug discovery | 9 | PolarisHub, Therapeutics Data Commons |
| Biomedical imaging | 4 | Kaggle |
| Protein engineering | 6 | ProteinGym |
| Single-cell omics | 5 | OpenProblems |

Each task ships as a **task capsule** — description, training data, test features, sample submission format. Agents run in Docker with a pre-built conda environment. Imaging got one L4 GPU and 16 hours; everything else was CPU-only with 8 hours. Four replicates per agent–task pair.
**The baseline.** Agent scores are mapped to **public leaderboard percentiles**. The authors argue these are more meaningful than MLE-bench's Kaggle leaderboards because ProteinGym, OpenProblems, and PolarisHub are populated largely by domain experts. Derived metrics: mean rank, above-median %, any-medal %, completion rate. Everything is reported under **penalized** scoring (failed run = percentile zero) and **non-penalized** (failed runs dropped) — the gap between the two separates capability from scaffolding reliability.
**Agents tested.** Two biomedical specialists (Biomni, STELLA) and two generalists (AIDE, MLAgentBench), plus a Dummy floor at ~2.5 percentile.
**Three findings.** All agents underperform humans on average, topping out at 34–37th percentile. Biomedical specialization gives no consistent advantage — generalist AIDE matched Biomni once failed-run penalties were removed. And agents almost never used deep learning, even on imaging, defaulting to random forests and gradient boosting, while human leaderboards are DL-dominated. Failure analysis (41 runs) attributed most crashes to resource exhaustion, silent try–except errors, and stalled installs — addressable engineering, not architecture.

---

#### The drug discovery section
**The tasks.** All nine are **molecular property prediction** from SMILES strings. Eight are ADME/Tox endpoints — the pharmacokinetic and safety filters that decide whether an active compound can be a drug:
- *Absorption* — Caco-2 permeability, solubility, lipophilicity (logP)
- *Distribution* — blood–brain barrier penetration, plasma protein binding
- *Metabolism* — human liver microsomal clearance, CYP2D6 substrate status
- *Toxicity* — hERG blockade (cardiotoxicity)

The ninth is a potency task: classifying EGFR wild-type kinase inhibitors above 80% inhibition, framed around glioblastoma. Datasets are small by ML standards — roughly 1,500–2,300 compounds each.
**Evaluation.** Binary tasks (hERG, BBB, CYP2D6, EGFR) are graded by **AUROC**; continuous tasks (solubility, logP, clearance, protein binding, permeability) by **Spearman** or the source benchmark's native metric. Notably, the EGFR task uses a **scaffold-based split** — train and test compounds have different core skeletons, so a model can't succeed by memorising close analogues. That's the realistic setting, since new drug programs work on novel chemistry.
