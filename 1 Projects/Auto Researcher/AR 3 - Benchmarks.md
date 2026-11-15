---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/benchmarks-and-datasets]
---
# AR 3 - Benchmarks

Part of [[AR 0 - Start Here|Auto Researcher]]. See also the general list in [[Benchmarks and Datasets]].

> [!summary] In short
> We need tasks with an **automatic, hidden and fair score**, so that a change in the agent can be measured. The current Kaggle task works but has problems (few final submissions, agents can copy public answers). Below are all candidate benchmarks from the notes, grouped by type.

> [!info] Status (27 Oct 2026)
> - The Kaggle task is **not** the final evaluation (decision of 14 Oct, see [[04 Decisions Log]]).
> - Under discussion (Claude's suggestion on 26 Oct, **not decided**): **MLE-bench Lite** run offline as the main benchmark (hidden test labels, no web access, and many recent baselines report on it), plus **MLRC-Bench** as a smaller second benchmark for the "research" claim. Drug-repurposing benchmarks later.

---

## 1. What a good benchmark needs (lessons so far)

- A score the agent can run during research (dev) **and** a held-out score it cannot see (test).
- No easy way to find the answer online (contamination).
- Long horizon: many cycles of improvement are possible.
- Not judged only by LLMs.

## 2. ML research engineering (closest to our current setup)

| Benchmark | What it is | Note |
|---|---|---|
| Kaggle competition (current) | Real leaderboard | Few final submissions; agents copy public answers |
| [[RE-Bench]] | 7 open-ended ML research engineering environments with a scoring function | Human expert comparison |
| [[ALE-Bench]] | 40 AtCoder heuristic contests, score-based, long-horizon | Maps to human rankings |
| [[BioML-bench]] | 24 biomedical ML tasks (drug discovery, imaging, proteins, single-cell) | See [[BioML-bench - Drug Discovery Tasks]] |
| ARC-Bench-ML, NanoResearch-20 | Datasets named in the August notes | Not studied yet. ARC-Bench may be the one from [[AutoResearchClaw]] (25 ML + 20 science topics, LLM-judged) |
| Scientist-Bench (AI-Researcher) | 22 AI papers as targets | Weak: contamination, LLM-only judging |
| MLE-bench style tasks | Base of BioML-bench. MLE-bench Lite (22 Kaggle tasks) is used by [[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]] | Kaggle-based: same copying risk as our task |
| [[FML-bench - Search Dynamics of Research Agents\|FML-bench]] | 18 ML research tasks in 10 areas (fairness, privacy, robustness, continual learning, ...), each starting from a real repo; 100-step budget | Built to compare the search behavior of agents; reports process metrics and the val-test gap |

## 3. Biomedical agent benchmarks

| Benchmark | What it is | Note |
|---|---|---|
| [[Biomni - General Biomedical Agent\|Biomni]] tasks | General biomedical agent | Good for medical baselines; repurposing task is easy (6.5/10) |
| [[BixBench]] | 61 bioinformatics capsules, 205 questions | Ignored: needs a specific environment |
| [[GENEDISCO]] | Which gene to test next | Active learning style |
| [[BioDiscoveryAgent]] | Gene perturbation design, cumulative hit ratio | - |
| [[PMO - Practical Molecular Optimization\|PMO]] | 23 molecule objectives, 10K oracle budget | Scores the path too |
| [[SMDD-Bench]] | 502 medicinal chemistry tasks | - |

## 4. Drug repurposing benchmarks (ranking from 25 Aug)

| Benchmark | Fit score | Why |
|---|---|---|
| **TRACES Drug Repurposing and Reformulation (DRR)** | 10/10 | Time cutoff + future clinical outcome + evidence reasoning |
| SerenQA | 9.5/10 | Measures novel / surprising repurposing discoveries |
| KG-Bench | 9/10 | Time-based Open Targets drug-disease prediction |
| repoDB | 9/10 | Has approved **and failed** drug-disease pairs |
| DrugMechDB | 9/10 | Ground-truth drug to disease mechanism paths |
| PrimeKG-CL | 8.5/10 | Tests added, kept and removed knowledge (belief updating) |
| Biomni repurposing task | 6.5/10 | Easy sanity baseline |

**Idea (1 Sep): "DrugMechDB inverted".** Take a disease, hide every curated drug and every mechanism path for it, and ask the model to rebuild them. The hidden material becomes evaluator-only labels.

## 5. Creativity benchmarks

- [[EscapeBench]], [[MUTATE - Divergent Thinking Benchmark|MUTATE]], CreativityBench, MacGyver, DeepMath-Creative - see [[Review - Creativity Measuring Benchmarks]].
- Our own small creativity datasets: [[Creativity Experiments - Datasets]].

## Related

- [[AR 5 - Open Questions]]
- [[Benchmarks and Datasets]]
