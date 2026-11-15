---
type: topic
aliases: [benchmarks, datasets]
tags: [type/topic, topic/benchmarks-and-datasets]
---
# Benchmarks and Datasets

> [!summary] In short
> All benchmarks and datasets that appear in the notes, grouped by what they test. For project-specific choices see [[AR 3 - Benchmarks]] and [[MS - Paper Plan (Baselines and Datasets)]].

## Research agents and ML engineering
- Kaggle competition (current AR test), [[RE-Bench]], [[ALE-Bench]], [[BioML-bench]], Scientist-Bench, ARC-Bench-ML, NanoResearch-20, MLE-bench.

## Biomedical agents
- [[Biomni - General Biomedical Agent|Biomni]], [[BixBench]], [[GENEDISCO]], [[BioDiscoveryAgent]], [[PMO - Practical Molecular Optimization|PMO]], [[SMDD-Bench]].

## Drug repurposing (for long-horizon research)
- TRACES DRR (10/10), SerenQA, KG-Bench, repoDB, DrugMechDB (and the "inverted" idea), PrimeKG-CL. Details: [[AR 3 - Benchmarks#4. Drug repurposing benchmarks (ranking from 25 Aug)]].

## Medical QA and evidence (multi-source project)
- **BioASQ list questions** (chosen), PGxQA, HealthContradict, PGxCorpus, RiTeK-PharmKG, STaRK-Prime, BioMedHop, PubMedQA, MedQuAD, LiveQA, MedRedQA, MedQA (used in `med_ddx`), our 10 ADR questions.

## Creativity
- [[EscapeBench]], [[MUTATE - Divergent Thinking Benchmark|MUTATE]], CreativityBench, MacGyver, DeepMath-Creative, VisEscape / VillagerBench, Alternative Uses Task. Our datasets: [[Creativity Experiments - Datasets]].

## Multi-agent debate
- GSM8K, MATH, MMLU, arithmetic, chess, biographies (Du et al.), and harder gradable tasks (HumanEval, SWE-bench, ALFWorld, PlanBench, miniF2F). See [[Review - MAD Benchmarks for Complex Tasks]].

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-25 One-on-One

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-08-25 One-on-One|AR - Meeting Log]], [[Creativity in Multi-Agent Debate#2026-08-25 One-on-One|Creativity in Multi-Agent Debate]], [[Agent Architectures and Frameworks#2026-08-25 One-on-One|Agent Architectures and Frameworks]], [[Creativity and Exploration#2026-08-25 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-08-25 One-on-One|MS - Meeting Log]]

| Benchmark | Fits | My fit score | Why |
|---|---|---|---|
| **TRACES Drug Repurposing & Reformulation (DRR)** | Whole PhD / especially long-horizon system | **10/10** | Temporal cutoff + future clinical outcome + evidence reasoning |
| **SerenQA** | Aim 1 + Aim 3 | **9.5/10** | Specifically measures novel/surprising drug-repurposing discoveries |
| **KG-Bench** | Aim 1 + retrospective validation | **9/10** | Temporal Open Targets drug–disease prediction |
| **repoDB** | Aim 2 | **9/10** | Contains both approved **and failed** drug–disease pairs |
| **DrugMechDB** | Aim 1 mechanistic creativity | **9/10** | Ground-truth mechanistic drug→disease paths |
| **PrimeKG-CL** | Aim 2 belief updating | **8.5/10** | Explicitly tests added, persistent, and removed biomedical knowledge |
| Biomni repurposing task | Initial sanity baseline | **6.5/10** | Easy agent benchmark, but not sufficiently creative |

## Notes on this topic

### Reports and experiments (1)

- [[Creativity Experiments - Datasets]] - The 4 small datasets used in the creativity experiments: med_ddx (real MedQA), med_repurpose (synthetic), gen_aut (alternative uses), gen_problems (synthetic city...

### Papers (17)

- [[AI-Researcher and Scientist-Bench]] - Paper-to-paper pipeline with an advisor agent; Scientist-Bench of 22 papers, judged by LLMs (NeurIPS 2025).
- [[ALE-Bench]] - Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.
- [[BioDiscoveryAgent]] - Stanford agent that designs gene perturbation experiments. Main metric: cumulative hit ratio over rounds.
- [[BioML-bench]] - 24 end-to-end biomedical ML tasks (drug discovery, imaging, protein engineering, single-cell). Tests full ML pipelines by agents.
- [[BioML-bench - Drug Discovery Tasks]] - The 9 drug discovery tasks of BioML-bench (potency, ADME, toxicity) from Polaris and TDC.
- [[Biomni - General Biomedical Agent]] - Biomni builds a large biomedical action space (tools, databases, software) from 2,500 papers. Good for medical baselines.
- [[BixBench]] - 61 expert bioinformatics capsules with 205 questions. Decided to ignore it because it needs a specific environment.
- [[BixBench - Tasks and Baselines]] - Details of BixBench tasks (bioinformatics capsules) and baselines.
- [[EndoBench]] - Reading item (no notes yet): evaluation of multimodal LLMs for endoscopy.
- [[EscapeBench]] - Text-only room-escape games to measure creative intelligence of LLM agents. Top models reach only about 15% progress without hints.
- [[FML-bench - Search Dynamics of Research Agents]] - 18 ML research tasks to compare the search behavior of six agents. Early gains predict the final score; scattered search does not help. AdaptiveSearch (greedy, then branch when stuck) is close to our exploration idea.
- [[GENEDISCO]] - Benchmark for choosing which genes to test next in costly experiments (active learning for drug target discovery).
- [[GENEDISCO - Creativity View]] - GENEDISCO seen as a creativity test: maximize target discovery rate and model performance.
- [[MUTATE - Divergent Thinking Benchmark]] - Text sandboxes where the obvious solution fails. Measures path-level and action-level divergence of agents. Close to EscapeBench.
- [[PMO - Practical Molecular Optimization]] - Benchmark with 23 molecule objectives and a 10K oracle budget; scores the path too (AUC top-10). Idea: use intermediate rewards in long horizons.
- [[RE-Bench]] - Seven open-ended ML research engineering environments with a scoring function, compared with human experts.
- [[SMDD-Bench]] - 502 medicinal-chemistry tasks over 102 protein targets (pharmacophore, interaction points, scaffold hopping, ...).

### Literature reviews and lists (4)

- [[Benchmarks - BC Children QA Candidates]] - Short list of QA benchmarks: PubMedQA, MedQuAD, LiveQA, MedRedQA.
- [[Review - Benchmarks for MoA and Multi-Agent Systems]] - Folder page for benchmark notes (GENEDISCO, RE-Bench, ALE-Bench, BioML, BixBench).
- [[Review - Creativity Measuring Benchmarks]] - Overview of benchmarks that measure creativity (CreativityBench, MacGyver, DeepMath-Creative, EscapeBench, MUTATE, bio tasks).
- [[Review - MAD Benchmarks for Complex Tasks]] - Which benchmarks MAD papers use, and which complex but gradable tasks could be used instead.

