---
type: report
date: 2026-08-25
status: Not started
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Evaluation and Metrics"]
tags: [type/report, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/3c7d68da32f08095980cee1644776b68
created: 2026-08-25
updated: 2026-08-25
---
# AMG-RAG Phases Report (Scores)

> [!summary] In short
> Phase catalogue, scores (on-topic %, grounded %, ClinPGx-valid %, false premise), search shape and genes found per phase.

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Evaluation and Metrics]]  
**From meeting:** [[MS - Meeting Log#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]]  
**Original:** [Notion page](https://app.notion.com/p/3c7d68da32f08095980cee1644776b68)

---

> [!attachment] File in Notion: **Phases Report.md** (380.3 KiB) - [open the Notion page](https://app.notion.com/p/3c7d68da32f08095980cee1644776b68)

### Phase catalogue

| Phase | Configuration | Retrieval behaviour | n |
|---|---|---|---|
| **Baseline A — no external sources** | `AMG_USE_EXTERNAL=0` | no external evidence; LLM knowledge only | 9 |
| **Baseline B — paper setup** | `AMG_USE_EXTERNAL=1` (PubMed + Wikipedia) | the paper's setup: live PubMed + Wikipedia | 9 |
| **Phase 3 (old run) — ClinPGx** | `AMG_KB_SOURCE=pharmgkb` (pre-provenance) | offline ClinPGx tables (before provenance logging) | 9 |
| **Phase 3 — ClinPGx only** | `AMG_KB_SOURCE=pharmgkb` | offline ClinPGx tables | 10 |
| **Phase 4 — ClinPGx + cited PubMed** | `AMG_KB_SOURCE=pharmgkb+pubmed` | ClinPGx, then the exact PMIDs ClinPGx cites for those pairs | 10 |
| **Phase 5 — + PMC full text** | `AMG_KB_SOURCE=pharmgkb+pubmed` `AMG_PUBMED_FULLTEXT=1` | as phase 4, abstracts upgraded to PMC full text | 10 |
| **Phase 6 — open LLM search** | `AMG_KB_SOURCE=pharmgkb+pubmed_open` | LLM picks search topics freely from question + graph; one topic per query | 10 |
| **Phase 7 — open search + full text** | `AMG_KB_SOURCE=pharmgkb+pubmed_open` `AMG_PUBMED_FULLTEXT=1` | as phase 6, plus PMC full text | 10 |
| **Phase 8 — combined-topic search** | `AMG_KB_SOURCE=pharmgkb+pubmed_combo` | graph-related terms combined into ONE conjunctive query; LLM picks the arity | 10 |
| **Phase 9 — combined + full text** | `AMG_KB_SOURCE=pharmgkb+pubmed_combo` `AMG_PUBMED_FULLTEXT=1` | as phase 8, plus PMC full text | 10 |
| **Phase 10 — free arity + own phrasing** | `AMG_KB_SOURCE=pharmgkb+pubmed_free` `AMG_PUBMED_FULLTEXT=1` | model chooses the arity (1, 2, 3, …) AND writes the PubMed query string itself | 10 |

### Scores
No gold answers exist for these questions (`reference` is empty), so BERTScore does not apply. These metrics are computable without one:
- **On-topic %** — retrieved papers mentioning *both* the question's drug *and* its adverse effect
- **Grounded %** — genes named in the answer that appear in evidence actually retrieved
- **ClinPGx-valid %** — genes named that ClinPGx really links to that drug
- **False premise** — Q8 asserts a CYP2D6–doxorubicin link that does not exist; is it caught?

| Phase | Refs/q | PubMed/q | Full text | On-topic % | Grounded % | ClinPGx-valid % | False premise |
|---|---|---|---|---|---|---|---|
| Baseline A — no external sources | 0.0 | 0.0 | 0 | n/a | n/a | 61% | ❌ |
| Baseline B — paper setup | 0.0 | 0.0 | 0 | n/a | n/a | 50% | ❌ |
| Phase 3 (old run) — ClinPGx | 0.0 | 0.0 | 0 | n/a | n/a | 94% | ❌ |
| Phase 3 — ClinPGx only | 8.5 | 0.0 | 0 | n/a | 100% | 95% | ❌ |
| Phase 4 — ClinPGx + cited PubMed | 22.6 | 14.3 | 0 | 22% | 100% | 95% | ✅ |
| Phase 5 — + PMC full text | 23.2 | 14.3 | 2 | 17% | 100% | 95% | ✅ |
| Phase 6 — open LLM search | 25.8 | 17.7 | 0 | 20% | 100% | 94% | ❌ |
| Phase 7 — open search + full text | 26.9 | 18.6 | 6 | 23% | 100% | 95% | ❌ |
| Phase 8 — combined-topic search | 22.2 | 13.5 | 0 | 54% | 100% | 93% | ❌ |
| Phase 9 — combined + full text | 22.5 | 14.0 | 6 | 54% | 100% | 95% | ❌ |
| Phase 10 — free arity + own phrasing | 13.0 | 4.7 | 5 | 60% | 96% | 85% | ❌ |

### Search shape — how many items were searched together (phases 8/9/10)

| Phase | Queries | 1-term | 2-term | 3-term | 4+-term | Mean arity | Back-offs | Empty |
|---|---|---|---|---|---|---|---|---|
| Phase 8 — combined-topic search | 160 | 0 (0%) | 85 (53%) | 75 (47%) | 0 (0%) | 2.47 | 19 | 12 |
| Phase 9 — combined + full text | 159 | 0 (0%) | 80 (50%) | 79 (50%) | 0 (0%) | 2.50 | 18 | 10 |
| Phase 10 — free arity + own phrasing | 392 | 23 (6%) | 237 (60%) | 108 (28%) | 24 (6%) | 2.34 | 0 | 39 |

### Genes surfaced, by phase

| Q | A | B | 3-old | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Q1 | ABCC3, SLC15A4, SLC22A8 | ABCB1, SLC2A1 | GSTM1, TPMT | GSTM1, TPMT | ACYP2, GSTM1 | GSTM1, GSTP1 | GSTM1 | ACYP2, GSTM1 | GSTM1, TPMT | ACYP2, GSTM1, TPMT | COMT, GADD45A, GSTM1, GSTT1, NFE2L2, TPMT |
| Q2 | ABCB1, GSTP1, SLC2A1 | ATP7A, SLC2A1 | GSTM1, TPMT | GSTM1, TPMT | GSTM1 | GSTM1 | COMT, GSTM1, TPMT | COMT, GSTM1, TPMT | GSTM1 | GSTM1, TPMT | ACYP2, COMT, GSTM1, TPMT |
| Q3 | TPMT | TPMT | GSTM1, TPMT | GSTM1, TPMT | ACYP2, GSTM1, TPMT | GSTM1, TPMT | TPMT | ACYP2, GSTM1, TPMT | TPMT | ACYP2, GSTM1, TPMT | TPMT |
| Q4 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1 | GSTT1, NRF2 |
| Q5 | OXR1, SLC7A11, XRCC1 | — | ERCC1, GSTP1 | ERCC1, GSTP1 | ERCC1, GSTM3, GSTP1 | ERCC1, GSTM3, GSTP1 | ERCC1, GSTM3, GSTP1 | ERCC1, GSTM3, GSTP1 | ERCC1, GSTM3, GSTP1 | ERCC1, GSTP1 | ERCC1, GSTM3, GSTP1 |
| Q6 | ABCB1, GSTP1 | ABCB1, CYP3A4, CYP3A5 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 | GSTP1, SLC28A3 |
| Q7 | ABCB1, APOE, GSTP1 | ABCB1, APOE, SLCO1B1 | GSTP1, SLC28A3 | RARG, SLC28A3 | GSTP1, RARG, SLC28A3 | GSTP1, RARG, SLC28A3 | RARG, SLC28A3 | GSTP1, RARG, SLC28A3 | SLC28A3 | RARG, SLC28A3 | GSTP1, RARG, SLC28A3 |
| Q8 | CYP2D6 | CYP2D6 | CYP2D6, SLC28A3 | CYP2D6, SLC28A3 | CYP2D6, SLC28A3 | CYP2D6, SLC28A3 | CYP2D6 | CYP2D6 | CYP2D6 | CYP2D6 | CYP2D6 |
| Q9 | GSTM1 | GSTM1 | GSTM1, GSTP1, SLC28A3 | GSTM1, GSTP1, SLC28A3 | GSTM1, SLC28A3 | GSTM1, SLC28A3 | GSTM1 | GSTM1, SLC28A3 | GSTM1 | GSTM1, SLC28A3 | GSTM1, SLC28A3 |
| Q10 | — | — | — | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 | CYBA, ERCC1 |
