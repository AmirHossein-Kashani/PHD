---
type: project-note
project: Multi-Source Evidence RAG
tags: [type/project-note, project/multi-source-rag, topic/benchmarks-and-datasets]
---
# MS - Paper Plan (Baselines and Datasets)

Part of [[Multi-Source Evidence RAG]]. Sources: [[MS - Meeting Log#2026-10-07 BC Children Meeting|2026-10-07 BC Children Meeting]], [[MS - Meeting Log#2026-09-29 One-on-One|2026-09-29 One-on-One]], [[MS - Meeting Log#2026-09-08 One-on-One|2026-09-08 One-on-One]], [[01 Now and Next#Daily 2026-10-15 to 2026-10-19|Daily 2026-10-15 to 2026-10-19]].

> [!summary] In short
> Paper outline is ready. Main dataset: **BioASQ list questions** (expert lists, PubMed-based; we add our KG). Five baselines: **LLM only, plain PubMed RAG, MindMap, simple hybrid RAG, AMG-RAG**. The final baseline decision was made on 15 Oct (one new baseline was added).

---

## 1. Paper outline (7 Oct)

1. Introduction
2. Related work
3. Method
   - Overview: define the problem, input (question + KG) and output (a list of possible answers with confidence); architecture figure
   - Evidence retrieval: KG and search augmentation; PubMed literature search
   - Evidence analysis and disagreement management: scoring system; conflict resolution; final answer with cited evidence
4. Experimental evaluation
   - Baselines: MindMap+ (Tulika), AMG-RAG (Tulika)
   - Metrics
   - Datasets: questions with multiple correct answers (Tulika is finding multiple-answer questions)
   - Results
5. Conclusion

Earlier writing: methodology sections 1-3 of the AMG-RAG extension and a proposal (files "Auditing_Evidence_Sections_1_to_3" and "Evidence_Scoped_Revision_Proposal" in [[MS - Meeting Log#2026-09-08 One-on-One|2026-09-08 One-on-One]]).

## 2. Baselines (final set)

| Baseline | Sources | Purpose |
|---|---|---|
| LLM only | none | stored model knowledge |
| Plain PubMed RAG | literature | search without the KG |
| [[MindMap - KG Prompting for LLMs\|MindMap]] | KG | graph-based answering |
| Simple hybrid RAG | KG + PubMed | both sources, but without our evidence assessment |
| [[AMG-RAG - Paper Summary\|AMG-RAG]] | KG + external retrieval | our closest predecessor |

Optional stronger baseline: **ToG-2** (Think-on-Graph 2.0), if it can be adapted to our KG and PubMed.
MindMap and AMG-RAG have public code; document how they were adapted to our sources and list answers.

## 3. Datasets

**Chosen:** **BioASQ list questions** - expert-written lists; PubMed-based. No KG inside, so the KG part is our adaptation.

Other options considered:

| Dataset | Note |
|---|---|
| RiTeK-PharmKG | built-in KG + entity text, but no real text search (crossed out) |
| STaRK-Prime | graph + entity text, text not searchable (crossed out) |
| BioMedHop | good match, but everything already exists in the KG |
| [PGxQA](https://github.com/KarlKeat/PGxQA) | pharmacogenomic QA (drug to gene, guidance, adversarial). Closest domain; mainly guideline knowledge. Planned as main external comparison (29 Sep) |
| HealthContradict | health questions with correct, wrong or conflicting documents. Closest to our conflict handling |
| PGxCorpus | extraction of genes, variants, drugs, phenotypes. Component evaluation |
| PubMedQA, MedQuAD, LiveQA, MedRedQA | general medical QA ([[Benchmarks - BC Children QA Candidates]]) |
| Our 10 ADR questions | internal set used for all phases |

## 4. Multi-source RAG papers to read (7 Oct)

- [HydraRAG: Structured Cross-Source Enhanced LLM Reasoning](https://arxiv.org/abs/2505.17464)
- [Think-on-Graph 2.0 (ToG-2)](https://arxiv.org/abs/2407.10805)
- [Chain-of-Knowledge (CoK)](https://arxiv.org/abs/2305.13269)
- [WeKnow-RAG](https://arxiv.org/abs/2408.07611)
- [HybGRAG](https://aclanthology.org/2025.acl-long.43)
- [DeepSieve](https://arxiv.org/abs/2507.22050)
- [PruningRAG: Multi-Source Knowledge Pruning](https://arxiv.org/abs/2409.13694)
- [QUASAR: RAG over Heterogeneous Data and Text](https://arxiv.org/abs/2412.07420)
- [PolyUQuest](https://arxiv.org/abs/2607.08269)
- Think-on-Graph v3 (read on 22 Oct): hierarchical graph relations (high level and low level); dynamic KG update by multiple agents.

## 5. Closest related work already read

- [[MedRAGChecker]] - claim-level checking with NLI + KG.
- [[Contradictions in Context - Medical RAG]] - filters contradicting papers instead of explaining them (our difference: we explain).
- [[MedGraphRAG]], [[GraphCheck]], [[KAG - Knowledge Augmented Generation|KAG]], [[DoM - Debate over Mixed-knowledge|DoM]], [[Debate on Graph (DoG)]].
- Reviews: [[Review - Stance-Aware KG Medical Fact Verification]], [[Review - Medical Fact-Verification Bibliography]], [[Review - Selected KG and RAG Papers]].
