---
type: paper
date: 2026-08-05
status: Not started
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/reversible-exploration, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3b3d68da32f080adbb7dce7f996ef30f
created: 2026-08-05
updated: 2026-08-13
---
# BixBench - Tasks and Baselines

> [!summary] In short
> Details of BixBench tasks (bioinformatics capsules) and baselines.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[BixBench]]  
**Original:** [Notion page](https://app.notion.com/p/3b3d68da32f080adbb7dce7f996ef30f)

---

#### Biological subject matter

| Area | Example capsules |
|---|---|
| Rare disease genetics | ASXL1 truncating mutations in Bohring-Opitz syndrome; Bloom syndrome probands/carriers and somatic CHIP variants |
| Cancer biology | CBD treatment of colorectal cancer cells; gene expression vs essentiality across cancer cell lines (DepMap-style) |
| Neurodegeneration | Alzheimer's patient subtyping by expression clustering |
| Microbiology | *P. aeruginosa* quorum sensing (rhlI/lasI/rhlR/lasR knockouts), swarming motility, media-dependent KEGG enrichment |
| Comparative genomics / evolution | Fungi vs animals — treeness, parsimony-informative sites, long branch scores, saturation (PhyKIT) |
| Immunology | Gene length vs expression across CD4/CD8/CD14/CD19 cells |
| Neuroscience | OXTR knockdown and NeuN counts in rodents |
| Clinical / epidemiological | BCG revaccination and COVID-19 severity in healthcare workers |
|  |  |

The agent gets raw data files and a question, must plan and execute a multi-step analysis in a Jupyter notebook (Python/R/bash), and submits a short answer. The *output* is a string; the *work* is code execution. This is why it's called an agent benchmark rather than a model benchmark — a zero-shot LLM has nothing to answer from.
Answer shapes, taken from the dataset's `eval_mode` field:

| Answer type | Verifier | Example |
|---|---|---|
| Exact value / name | `str_verifier` | `166`, `CDKN1A`, `4,550`, `1-50` |
| Numeric w/ tolerance | `range_verifier` | `(1.50,1.54)`, `(0.023,0.025)`, `(60,70)` |
| Semantic / free-form | `llm_verifier` | "Right-skewed with a long tail", "Downregulation" |

The MCQ regime is a *derived* format, not the native one: each question also ships an `ideal` answer plus 3 `distractors`, so it can be recast as 4-way classification (5-way when the refusal option is added). The paper treats open-answer as primary, because real bioinformaticians don't get answer choices.
Contrast with BioML-bench: there the task **is** classification/regression, and the agent's output is a prediction file. Here classification only exists as an evaluation convenience wrapper.

#### How it's evaluated
Three regimes, all run **10 parallel replicates per capsule** to handle stochastic trajectories.
**1. Open-answer (primary).** The agent calls `submit_answer` to end the trajectory. A judge LLM (Claude 3.5 Sonnet) compares the submission to ground truth, scored **binary 1/0**. Accuracy = fraction correct across all runs × all questions.
**2. MCQ with refusal.** After the trajectory, a second LLM receives the notebook + the open answer + the options, including an *"Insufficient information"* opt-out, and picks one. Majority vote across the 10 runs. Reports accuracy **and precision** (correct ÷ answered, i.e. excluding refusals). The opt-out isn't padding — it measures whether the agent knows when its analysis is inadequate.
**3. MCQ without refusal.** Same, opt-out removed. Random chance ≈ 25% (1 ideal + 3 distractors).
**Calibration baseline — pure recall.** The same questions asked with no notebook, no data, no analysis. This is the critical control: questions are designed by construction to be unanswerable from memory, so any score at or below this line means the agent isn't actually using its analysis. Each row also carries a **canary GUID** to detect training-corpus contamination.
**No human baseline exists.** The authors assumed experts would score far higher and deprioritized collecting it. So unlike BioML-bench (which ranks against expert leaderboards), you cannot state an agent-vs-human gap here.

#### Baselines and SOTA
**Original paper (Feb 2025), full 296-question set, open-answer:**

| Model | Open-answer acc. |
|---|---|
| GPT-4o | 9% |
| Claude 3.5 Sonnet | 17% |

Both used the same custom ReAct/Aviary scaffold with three tools (`edit_cell`, `list_workdir`, `submit_answer`). The ICML version reports ~21% at best. In MCQ **with** refusal both scored *worse than random* — heavy abstention under complexity, worsening as vote count rose. **Without** refusal, only marginally above random and near the recall baseline.
**Progression on the full benchmark:**

| System | Open-answer acc. |
|---|---|
| GPT-4o | 9% |
| Claude 3.5 Sonnet | 17% |
| Gemini 2.5 Pro | 18.3% |
| GPT-5 | 22.9% |
| K-Dense Analyst | 29.2% |
| Kepler | 33.4% |
| **BioAgent Deep Research** | **48.8%** |

BioAgent Deep Research also reports 55.1% MCQ-with-refusal and 64.4% MCQ-without-refusal (vs GPT-4o 21%, Claude 25% on the refusal variant).
**BixBench-Verified-50** (curated subset, ambiguous items removed/revised) — much higher, near saturation:

| System | V50 acc. |
|---|---|
| Biomni | 60.0% |
| Crow | 70.0% |
| Raw coding agent | 76.0% |
| StatefulDiscovery-QG | 80.0% |
| Skill-augmented frontier agents | 84.0 / 88.0 / 98.0% |
