---
type: topic
aliases: [drug discovery, pharmacogenomics, biomedicine, ADR]
tags: [type/topic, topic/biomedicine-and-drug-discovery]
---
# Biomedicine and Drug Discovery

> [!summary] In short
> The application area of both projects. The multi-source project works on **pharmacogenomics**: which gene variants change the risk of an **adverse drug reaction (ADR)**, like cisplatin hearing loss or doxorubicin heart damage. The auto-researcher work looked at **drug repurposing** and **molecule design** as long-horizon research tasks.

## Key facts from my notes

- **ClinPGx / PharmGKB:** curated gene-variant-drug data with evidence levels 1A (strong) to 4 (weak). Used as the KG in [[Multi-Source Evidence RAG]].
- **Known example links:** ACYP2, GSTM1, TPMT, COMT with cisplatin ototoxicity; SLC28A3 rs7853758 with doxorubicin cardiotoxicity (level 2B). **CYP2D6-doxorubicin is a false premise** (no link).
- **Drug discovery pipeline:** target, hit, lead, optimization, trials; ADMET; Lipinski's rules ([[Biology and Chemistry Background Plan]]).
- **Agent systems:** [[Virtual Lab]], [[Biomni - General Biomedical Agent|Biomni]], [[BioDiscoveryAgent]], [[SAGA]], [[ChemCRAFT]], PharmAgents, MADD, DrugAgent, Robin, Medea, Agentomics.
- **Benchmarks:** [[BioML-bench]], [[GENEDISCO]], [[PMO - Practical Molecular Optimization|PMO]], [[SMDD-Bench]], TRACES DRR, DrugMechDB, repoDB.

## Where it is used

- [[Multi-Source Evidence RAG]]
- [[Auto Researcher]] (possible tasks)
- [[Early Exploration (May-June 2026)]] (protein and molecule generation)

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-13 One-on-One (13 and 20 Aug)

*Original: [Notion page](https://app.notion.com/p/3bbd68da32f080b9b7e7f143d7fa3505)*  
*Other parts of this meeting:* [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|Creativity in Multi-Agent Debate]], [[MS - Meeting Log#2026-08-13 One-on-One (13 and 20 Aug)|MS - Meeting Log]]

- Read this paper
    MolClaw: An Autonomous Agent with Hierarchical Skills for Drug Molecule Evaluation, Screening, and Optimization.

**Multi-agent — molecular design side**
- **PharmAgents** (Gao et al., 2025) — [https://arxiv.org/abs/2503.22164](https://arxiv.org/abs/2503.22164)
- **MADD: Multi-Agent Drug Discovery Orchestra** (Solovev et al., 2025; EMNLP 2025 Findings) — [https://arxiv.org/abs/2511.08217](https://arxiv.org/abs/2511.08217)
- **DrugSAGE** (self-evolving agent experience; benchmarked on 22 TDC + 11 Polaris datasets) — [https://arxiv.org/abs/2605.15461](https://arxiv.org/abs/2605.15461)
- **ChatInvent** — He et al., *Democratising real-world drug discovery through agentic AI*, Drug Discovery Today 31(2):104605 (2026), doi:10.1016/j.drudis.2026.104605, PMID 41548711. Paywalled; no preprint that I found.

**Multi-agent — pipeline / experiment automation side**
- **DrugAgent** (Liu et al., 2024) — [https://arxiv.org/abs/2411.15692](https://arxiv.org/abs/2411.15692)
- **DiscoVerse** (Zheng et al., Roche, 2025) — [https://arxiv.org/abs/2511.18259](https://arxiv.org/abs/2511.18259)
- **Medea** (Sui et al., Zitnik Lab) — [https://www.biorxiv.org/content/10.64898/2026.01.16.696667v3](https://www.biorxiv.org/content/10.64898/2026.01.16.696667v3) · code: [https://github.com/mims-harvard/Medea](https://github.com/mims-harvard/Medea)
- **Agentomics** — this is the one with the 11/20 result. Benchmarked against human expert solutions, Agentomics generated novel state-of-the-art models for 11 of 20 established benchmark datasets. Preprint: [https://www.biorxiv.org/content/10.64898/2026.01.27.702049v1](https://www.biorxiv.org/content/10.64898/2026.01.27.702049v1) · now peer-reviewed in *Bioinformatics*: [https://academic.oup.com/bioinformatics/article/42/Supplement_1/btag250/8726289](https://academic.oup.com/bioinformatics/article/42/Supplement_1/btag250/8726289) · code: [https://github.com/BioGeMT/Agentomics-ML](https://github.com/BioGeMT/Agentomics-ML) [bioRxiv](https://www.biorxiv.org/content/10.64898/2026.01.27.702049v1.full.pdf)

**End-to-end discovery**
- **Coscientist** (Boiko et al.) — *Nature* 624:570–578 (2023), doi:10.1038/s41586-023-06792-0
- **Robin** — preprint [https://arxiv.org/abs/2505.13400](https://arxiv.org/abs/2505.13400) · code [https://github.com/Future-House/robin](https://github.com/Future-House/robin) · blog [https://www.futurehouse.org/research/demonstrating-end-to-end-scientific-discovery-with-robin-a-multi-agent-system](https://www.futurehouse.org/research/demonstrating-end-to-end-scientific-discovery-with-robin-a-multi-agent-system)
- **Kosmos** (Mitchener et al., 2025) — [https://arxiv.org/abs/2511.02824](https://arxiv.org/abs/2511.02824) · announcement [https://edisonscientific.com/news/announcing-kosmos](https://edisonscientific.com/news/announcing-kosmos)

## Notes on this topic

### Reports and experiments (4)

- [[AMG-RAG - Role of LLM, KG and Internet]] - Write-up: what parametric LLM knowledge, the curated KG, and internet search each add to ADR question answering. The best system is a hybrid.
- [[AMG-RAG Phase 11 - Method Note]] - Full method note of Phase 11: knowledge sources, 6-stage pipeline, evidence ledger with rule-based confidence, contradiction flags, results and limitations.
- [[AMG-RAG Phase Evolution Report]] - Story of all AMG-RAG phases from baseline to Phase 13. Phase 11 is most precise; Phase 13 is best for finding and explaining contradictions.
- [[AMG-RAG Phase 9 - How It Works]] - Phase 9 of AMG-RAG: search related KG entities together as one PubMed AND-query, with back-off, then read open full text.

### Papers (19)

- [[AMG-RAG - Paper Review Notes]] - My review/feedback notes on the AMG-RAG paper (agentic medical KG for QA).
- [[AMG-RAG - Paper Summary]] - AMG-RAG: LLM agents build and update a medical KG from live sources and reason over it. Our multi-source project extends this system.
- [[BioDiscoveryAgent]] - Stanford agent that designs gene perturbation experiments. Main metric: cumulative hit ratio over rounds.
- [[BioML-bench]] - 24 end-to-end biomedical ML tasks (drug discovery, imaging, protein engineering, single-cell). Tests full ML pipelines by agents.
- [[BioML-bench - Drug Discovery Tasks]] - The 9 drug discovery tasks of BioML-bench (potency, ADME, toxicity) from Polaris and TDC.
- [[Biomni - General Biomedical Agent]] - Biomni builds a large biomedical action space (tools, databases, software) from 2,500 papers. Good for medical baselines.
- [[BixBench]] - 61 expert bioinformatics capsules with 205 questions. Decided to ignore it because it needs a specific environment.
- [[BixBench - Tasks and Baselines]] - Details of BixBench tasks (bioinformatics capsules) and baselines.
- [[ChemCRAFT]] - Small LLMs trained step by step to use chemistry tools. Not my favourite.
- [[Contradictions in Context - Medical RAG]] - Shows that medical RAG is not reliable just because papers are relevant: similar papers can disagree. They filter out contradicting papers instead of explaining why.
- [[GENEDISCO]] - Benchmark for choosing which genes to test next in costly experiments (active learning for drug target discovery).
- [[GENEDISCO - Creativity View]] - GENEDISCO seen as a creativity test: maximize target discovery rate and model performance.
- [[MedGraphRAG]] - Graph RAG for medicine with a three-tier KG (patient data, literature, UMLS) and U-Retrieval. Too complex for us.
- [[MedRAGChecker]] - Breaks an answer into claims and checks each claim with NLI on retrieved text and with a biomedical KG (DRKG or BioPortal).
- [[Molecule Generation for Drug Design - Graph Learning View]] - Reading item (no notes yet): molecule generation for drug design from a graph learning view.
- [[PMO - Practical Molecular Optimization]] - Benchmark with 23 molecule objectives and a 10K oracle budget; scores the path too (AUC top-10). Idea: use intermediate rewards in long horizons.
- [[SAGA]] - SAGA paper files. We tried to run SAGA (Compute Canada, then cloud with gpt-mini).
- [[SMDD-Bench]] - 502 medicinal-chemistry tasks over 102 protein targets (pharmacophore, interaction points, scaffold hopping, ...).
- [[Virtual Lab]] - AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

### Literature reviews and lists (3)

- [[Review - Agent Architectures for Molecule and Chemistry Design]] - Survey of agentic AI for molecule, DNA and chemistry design since 2024, with a comparison table and the selected papers to read.
- [[Review - Medical Fact-Verification Bibliography]] - Categorized bibliography (about 55 papers) for stance-aware, KG-grounded, multi-document medical fact verification.
- [[Review - Stance-Aware KG Medical Fact Verification]] - Literature review for a system that splits support vs contradict evidence over a dynamic medical KG and gives a confidence score.

### Learning (2)

- [[Biology and Chemistry Background Plan]] - Week-by-week plan to learn enough chemistry, biology and drug discovery basics.
- [[Biology ML Course (MIT)]] - MIT machine learning for biology lectures (AlphaFold).

