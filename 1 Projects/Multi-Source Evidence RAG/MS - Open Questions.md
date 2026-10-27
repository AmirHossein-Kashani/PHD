---
type: project-note
project: Multi-Source Evidence RAG
tags: [type/project-note, project/multi-source-rag]
---
# MS - Open Questions

Part of [[Multi-Source Evidence RAG]]. Collected from [[MS - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]], [[MS - Meeting Log#2026-09-29 One-on-One|2026-09-29 One-on-One]], [[MS - Meeting Log#2026-10-07 BC Children Meeting|2026-10-07 BC Children Meeting]], [[MS - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[MS - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]] and [[01 Now and Next#Daily 2026-10-20 to 2026-10-23|Daily 2026-10-20 to 2026-10-23]].

> [!tip] How to use this note
> When you decide something, move it to [[04 Decisions Log]] and tick it here.

## Experiments
- [ ] **Start experiments with the multi-source system** on the chosen dataset (BioASQ list questions) against the 5 baselines.
- [ ] **Trajectory idea for multi-evidence:** add the trajectory (currently "long path trajectory findings") to the system.
- [ ] How to expand the multi-source part (more sources? which?).
- [ ] Try ideas from Think-on-Graph v3: **hierarchical graph relations** (high level and low level) and **dynamic KG update by multiple agents**.
- [ ] Test the "5 new ideas" explored on 21 Oct (not written in detail in Notion - add them here).

## Method fixes (from the phase reports)
- [ ] Hybrid system: Phase 13 contradiction layer + Phase 11 precise search, calibration and false-premise check.
- [ ] Require a citation for every claim in the final answer.
- [ ] Strip brackets from entity names before searching (`GSTM1 (GSTM1 non-null)`).
- [ ] Normalize gene symbols.
- [ ] Better context extraction (population, endpoint, dose, cancer type).
- [ ] Separate "truly insufficient evidence" from "curated-only evidence".
- [ ] Confidence is too compressed; contradiction precision is low (0.47).

## Paper
- [ ] Write the method section from the outline in [[MS - Paper Plan (Baselines and Datasets)]].
- [ ] Metrics for list answers (precision, recall, F1 of the list? calibration of confidence?).
- [ ] Talk with Tulika about the architecture improvement ideas (21 Sep note).
- [ ] Clean the code into a repository.
- [ ] Discuss the BC Children meeting outcome (20 Oct agenda).
