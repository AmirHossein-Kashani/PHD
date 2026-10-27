---
type: decisions
tags: [type/decisions]
---
# Decisions Log

> [!summary] In short
> Important decisions found in the notes, newest first. Each line says **what** was decided, **why** (if written), and **where** it came from. Add new decisions at the top.

| Date | Project | Decision | Why / note | Source |
|---|---|---|---|---|
| 2026-10-20 | All | Move research notes from Notion to **Obsidian** | easier to hold and link content | [[AR - Meeting Log#2026-10-20 One-on-One\|2026-10-20 One-on-One]] |
| 2026-10-15 | Multi-Source | **Final baseline set** for the multi-evidence system (one new baseline added) | - | [[01 Now and Next#Daily 2026-10-15 to 2026-10-19\|Daily 2026-10-15 to 2026-10-19]] |
| 2026-10-14 | Auto Researcher | The Kaggle competition is **not a good final evaluation** | few final submissions; validation and final scores disagree; agents copy public answers | [[AR - Meeting Log#2026-10-14 One-on-One\|2026-10-14 One-on-One]] |
| 2026-10-14 | Auto Researcher | In the paper, claim **local knowledge (path to root)** as the contribution; **do not** claim shared knowledge; do not mention explicit research steps yet | shared summaries exist in Kosmos; research steps not built yet | [[AR Paper - Introduction Draft]] |
| 2026-10-07 | Multi-Source | Dataset: **BioASQ list questions**; baselines: LLM only, plain PubMed RAG, MindMap, simple hybrid RAG, AMG-RAG (ToG-2 optional) | best fit for a PubMed-based system with list answers | [[MS - Meeting Log#2026-10-07 BC Children Meeting\|2026-10-07 BC Children Meeting]] |
| 2026-10-07 | Multi-Source | Drop RiTeK-PharmKG and STaRK-Prime | no searchable text | [[MS - Meeting Log#2026-10-07 BC Children Meeting\|2026-10-07 BC Children Meeting]] |
| 2026-09-29 | Multi-Source | PGxQA as main external comparison; HealthContradict if the ledger can be generalized; PGxCorpus for the extraction stage | - | [[MS - Meeting Log#2026-09-29 One-on-One\|2026-09-29 One-on-One]] |
| 2026-09-01 | Multi-Source | **Phase 13** = main research configuration; **Phase 11** = precision and safety control; Phase 4 = conservative baseline | Phase 13 explains contradictions; Phase 11 is most precise and passes the false-premise test | [[AMG-RAG Final Conclusion]] |
| 2026-08-20 | Creativity | Adopt **persona** asymmetry; use a **dual-output** aggregator; drop representation / shuffling; use 3+ repeats next time | see experiment results | [[Creativity Exp 2 - Conclusion]] |
| 2026-08-12 (approx.) | Auto Researcher | Build on **Arbor** | closest dynamic research-search system; persistent tree | [[Arbor - Hypothesis Tree Refinement]] |
| 2026-08-11 | Auto Researcher | EviGraph is **not very similar** to our work | it uses the graph for evidence, not for thinking steps | [[EviGraph - Evidence-Guided Research Agents]] |
| 2026-08-04 | Auto Researcher | Ignore **BixBench** | needs a specific environment | [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One\|2026-08-04 One-on-One]] |
| 2026-08-04 | Auto Researcher | Biomni good for medical baselines; AIDE close to our idea | - | [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One\|2026-08-04 One-on-One]] |
| 2026-07-27 | Debate | **Stay close to MAD and MoA** | - | [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-07-27 One-on-One\|2026-07-27 One-on-One]] |
| 2026-07-15 | Debate | Baselines for the Gaps protocol: Du et al. MAD, ReConcile (+ MACI, SMADE-IE) | Du et al. is canonical; ReConcile is the strongest answer-level aggregation | [[Review - Baselines for Gaps Protocol]] |
| 2026-06-17 | Tools | Use **Fir** (at SFU) but **Nibi** is more reliable | fewer users | [[How to Start Compute Canada]] |
| 2026-06-16 | Early | Run SAGA with **gpt-mini** | reduce API cost | [[Early Exploration (May-June 2026)#2026-06-16 One-on-One\|2026-06-16 One-on-One]] |
| 2026-06-09 | Multi-Source | **AMG-RAG** is the base system | straightforward and close to our approach; MedGraphRAG too complex; GraphCheck uses GNN | [[MS - Meeting Log#2026-06-09 One-on-One\|2026-06-09 One-on-One]] |
| 2026-06-09 | Early | SAGA must run in the **cloud**, not HPC | no way to run on HPC | [[Multi-Agent Debate#2026-06-09 One-on-One\|2026-06-09 One-on-One]] |
| 2026-06-09 | Early | No need to read AutoGen / MetaGPT further | framework, not research | [[Multi-Agent Debate#2026-06-09 One-on-One\|2026-06-09 One-on-One]] |
| 2026-05-15 | Early | Plan-and-Execute: no need to continue; ChemCRAFT: done but not my favourite | too simple / not interesting | [[Plan-and-Execute]], [[ChemCRAFT]] |
