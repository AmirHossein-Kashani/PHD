---
type: project-note
project: Multi-Source Evidence RAG
tags: [type/project-note, project/multi-source-rag, topic/evaluation-and-metrics]
---
# MS - Phases and Results

Part of [[Multi-Source Evidence RAG]]. Main sources: [[AMG-RAG Phase Evolution Report]], [[AMG-RAG Phases Comparison Tables]], [[AMG-RAG Phases Report (Scores)]], [[AMG-RAG Final Conclusion]], [[AMG-RAG - Role of LLM, KG and Internet]].

> [!summary] In short
> All phases use the **same model (Qwen2.5-14B)** and the **same 10 ADR questions**. Only the retrieval and evidence rules change (by environment variables). Precision went from about 20% to **65%** (Phase 11). Phase 13 is the best for **explaining contradictions**. No single phase is best for everything.

---

## 1. Evaluation set and metrics

- **10 ADR clinical questions**, mostly cisplatin ototoxicity / neuropathy and anthracycline (doxorubicin) cardiotoxicity. Early baselines used 9 of them.
- There are **no gold answers**, so no BERTScore or accuracy. Instead:

| Metric | Meaning |
|---|---|
| Refs/q | citations per question |
| On-topic % | retrieved papers that mention **both** the drug and the side effect (a lower bound) |
| Grounded % | genes in the answer that appear in retrieved evidence |
| ClinPGx-valid % | genes in the answer that ClinPGx links to that drug |
| False premise | Q8 claims a CYP2D6-doxorubicin link that does not exist. Does the answer catch it? |

## 2. All phases at a glance

| Phase | What changed | Refs/q | On-topic | ClinPGx-valid | False premise |
|---|---|---|---|---|---|
| Baseline A | LLM only, no external sources | 0 | - | 61% | missed |
| Baseline B | original AMG-RAG (PubMed + Wikipedia) | 0 | - | 50% | missed |
| 3 | ClinPGx tables only | 8.5 | - | 95% | missed |
| **4** | + PubMed papers cited by ClinPGx | 22.6 | 22% | 95% | **caught** |
| 5 | + full text | 23.2 | 17% | 95% | caught |
| 6 | LLM picks open search topics | 25.8 | 20% | 94% | missed |
| 7 | open search + full text | 26.9 | 23% | 95% | missed |
| 8 | related KG terms joined with AND | 22.2 | 54% | 93% | missed |
| 9 | 8 + full text | 22.5 | 54% | 95% | missed |
| 10 | LLM writes free queries + paragraph reading | 13.0 | 59% | 80% | missed |
| 9L | Phase 9 retrieval + ledger | 23.4 | 52% | 89% | missed |
| **11** | **evidence ledger** | 13.0 | **65%** | 85% | **caught** |
| 12 | + Europe PMC and Unpaywall full text | - | - | - | - |
| **13** | **contradiction resolution layer** | 23.8 | 49% | - | missed |

Note: for 9L and 11, "ClinPGx-valid" punishes new genes that ClinPGx has not curated, so it is not a pure quality score.

## 3. The story in seven steps

1. **Baselines:** fluent answers, but genes were guessed. No reliable sources. The original PubMed code even removed PMID lines.
2. **Phase 3 - ground in ClinPGx:** gene validity from 50-61% to **95%**.
3. **Phase 4 - cite the papers:** first citable evidence (22.6 refs/q). Found ACYP2 for cisplatin hearing loss. Rejected the false premise.
4. **Phases 6-7 - open search:** more papers but too broad ("cancer", "tinnitus"). Only 20-23% on topic. The noise made the model miss the false premise.
5. **Phases 8-9 - search relations:** `cisplatin AND ACYP2 AND hearing loss`. On-topic **54%**.
6. **Phases 10-11 - free queries, paragraph reading, ledger:** on-topic **65%**, calibrated confidence, contradiction recall 0.95.
7. **Phases 12-13 - more full text and contradiction resolution:** full text 6% to 43%; separate real conflicts from context differences.

## 4. Phase 11 numbers (precision and safety)

- 96 gene-drug pairs (9.6 per question); 61 with a contradiction flag.
- 29 papers read, 41 paragraphs cited, 4 full text.
- **Calibration** on 72 labelled ClinPGx pairs: positive mean confidence 0.424; separation positive minus negative **+0.351**; **Spearman 0.721**; all 16 negatives placed in `very_low`.
- **Contradiction detection:** precision 0.47, recall **0.95**, F1 0.62.
- Runtime: 45-47 minutes for 10 questions on one GPU slice.

## 5. Phase 13 numbers (research goal)

- 145 gene-drug pairs; 23.8 refs per question; 17 papers in full; 70 paragraphs cited.
- **22** truly conflicting pairs; **3** context-dependent disagreements; **29** mechanistic results downgraded.
- Research-weighted score **4.44** (first place).
- Limits: 83% of pairs INSUFFICIENT; half of conflicts without a clear explaining factor; precision 49%; missed the false premise.

## 6. Final assessment

| Purpose | Best phase |
|---|---|
| Precision, calibrated confidence, safer answers | **Phase 11** |
| Broad research and contradiction resolution | **Phase 13** |
| Conservative baseline (mostly curated evidence) | **Phase 4** |

**Future system:** Phase 13 contradiction layer + Phase 11 focused retrieval, confidence and false-premise check. Also: better context extraction, normalize gene symbols, keep provenance, separate "truly insufficient" from "curated-only", and **require citations in every final answer**.

## 7. What each source adds (from the write-up)

- **LLM knowledge:** fluent and good for mechanisms and hypotheses, but turns plausibility into unsupported specifics.
- **Curated KG:** biggest gain in validity, exact variants, provenance, evidence levels. It also helps a second time: **as a structure for better searches**.
- **Internet search:** new studies, missing biomarkers, full text, contradictions. But unconstrained search can add enough noise to **reverse a correct safety judgment**.

## Known problems to fix

- Answer prose rarely cites (1 of 10 answers in Phase 11). **The ledger is the real deliverable.**
- Confidence is compressed (nothing reaches `high`).
- Contradiction precision 0.47 (over-flags).
- Entity names with brackets, like `GSTM1 (GSTM1 non-null)`, break searches. Strip the bracket part.
- Gene symbols not normalized; glossary sections can look like findings.
- Calibration labels come from the same source the system retrieves from (partly self-consistent).
- Only 10 questions; results are descriptive, not significant.
