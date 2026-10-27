---
type: project-note
project: Multi-Source Evidence RAG
tags: [type/project-note, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
---
# MS - Method (Pipeline and Ledger)

Part of [[Multi-Source Evidence RAG]]. Main sources: [[AMG-RAG Phase 11 - Method Note]], [[AMG-RAG Phase 9 - How It Works]], [[AMG-RAG Phase Evolution Report]], [[AMG-RAG Final Conclusion]].

> [!summary] In short
> The system builds a small KG from the question and the curated database, lets the LLM write focused PubMed searches using the KG links, reads the most useful paragraphs of each paper, and puts every claim into an **evidence ledger**. Confidence is computed by a **fixed rule, never by the LLM**. In Phase 13, a **contradiction layer** checks if two sources really disagree or only study different contexts.

---

## 0. Three rules of the method

1. **Recall is the goal.** Report every candidate gene, also weak ones. Weak ones are reported *as weak*, not dropped.
2. **Keep disagreement.** When sources conflict, flag the pair and keep both sides. Do not average.
3. **Confidence by rule.** The LLM only extracts and classifies. A formula makes the number, so every score can be traced.

## 1. Knowledge sources

| Source | What it gives |
|---|---|
| ClinPGx / PharmGKB `clinicalVariants.tsv` (5,190 rows) | gene, variant, drug, phenotype, **evidence level 1A to 4** |
| ClinPGx / PharmGKB `relationships.tsv` (127,988 pairs) | gene-drug association status (74,646 associated, 33,100 not associated, 20,032 ambiguous) and the PMIDs cited |
| PubMed / PMC (NCBI E-utilities, about 3 requests per second, cached) | abstracts and open full text |
| Europe PMC and Unpaywall (from Phase 12) | more legal open-access full text |

## 2. Pipeline step by step

1. **Entities and KG.** The LLM extracts entities from the question. ClinPGx rows are added as edges `drug -> gene -> variant`, weighted by evidence level. The LLM also infers relations between entity pairs.
2. **Query writing.** A chain gets the question, the KG edges (like `cisplatin -[response_modulated_by]-> GSTM1`) and the evidence so far. The LLM writes PubMed queries and decides how many concepts to combine (from Phase 10 on). Example: `(GSTM1 OR TPMT) AND cisplatin AND hearing loss`.
   - **Back-off:** if a query returns nothing, drop field tags, then the last `AND` term. This removed all empty searches.
   - Results from different queries are mixed **round-robin**, so each query contributes.
3. **Retrieval.** ClinPGx first, then PubMed (PMID, title, abstract, journal, year). If open access exists: PMID -> PMCID -> JATS XML body split into paragraphs (section titles kept); PDF fallback.
4. **Paper reading (digest).** Paragraphs are grouped into batches of up to 2,400 characters and **ranked before reading**: variant IDs and effect statistics (x3), gene symbols (x2), Results/Discussion sections (+8), Introduction (-6), background phrases (-5). The **top 6 batches** are read. For each, the LLM returns: the paragraph numbers that matter, a 2-3 sentence finding, the **study type**, and the genes it **supports** and **refutes**. Every paragraph has an ID like `PMID:42633148/PMC13499120#p34`.
5. **Evidence ledger.** All claims (curated and literature) are grouped per **(gene, drug)** pair, with confidence and contradiction flags (below).
6. **Contradiction resolution (Phase 13).** See section 5.
7. **Answer.** The ledger is put into the answer prompt. The answer must report every gene with its label, say what disagrees with what, and never claim a link the ledger marks unsupported.

## 3. Confidence formula (Phase 11)

```text
base    = level confidence: 1A .98  1B .90  2A .80  2B .70  3 .55  4 .40
        = 0.25 if the pair is not in the curated tables
        = 0.15 if curators say NOT associated
        = 0.55 if associated but no clinical-variant level

support = sum of study-type weights over DISTINCT sources
          curated / meta-analysis 1.0, systematic review .95, GWAS .9,
          human cohort .85, clinical trial .85, case report .45,
          review .4, animal .3, in vitro .2

confidence = clamp( base * (1 + 0.18*log1p(support)) - 0.30*log1p(refute), 0, 1 )
             x 0.9 if curators mark the pair ambiguous

labels: high >= 0.75, moderate >= 0.50, low >= 0.25, very_low below
```

- One paper counts once (sources are de-duplicated).
- Support is log-damped: the 10th confirming study adds less than the 2nd.

## 4. Contradiction flags (Phase 11)

| Flag | When it fires |
|---|---|
| `curator_ambiguous` | curators recorded conflicting evidence |
| `curated_vs_literature` | tables and papers disagree |
| `literature_internal` | papers support and refute the same pair |
| `uncurated_literature_claim` | not in tables, but backed by a human study - **a candidate new finding** |
| `unsupported_claim` | not in tables, nothing stronger than a review or animal study |

A pair is **disputed** when any flag except `unsupported_claim` fires.

## 5. Contradiction resolution layer (Phase 13)

- Each piece of evidence gets a **tier**: curated, clinical, mechanistic, or narrative.
- A direct contradiction is allowed **only between comparable tiers**. An in-vitro null result cannot "refute" a clinical association; it is **downgraded** (29 such downgrades).
- Claims are normalized by **population, cancer type, endpoint, dose, and effect direction**.
- This explained 3 conflicts as context-dependent:
  - **TPMT - cisplatin:** different hearing-loss endpoints.
  - **SLC28A3 - anthracycline:** children vs adults.
  - **GSTM1 - doxorubicin:** different cancer types.
- When fewer than two comparable clinical sources exist, the system says **INSUFFICIENT** (83% of pairs). This is honest, not a failure.

## 6. Setup

- Model: **Qwen2.5-14B**, served locally with **Ollama** on one H100 MIG 2g.20gb slice, context 16,384 (Compute Canada / Alliance). See [[How to Start Compute Canada]].
- Everything is controlled by environment variables, for example:

```bash
AMG_KB_SOURCE=pharmgkb+pubmed_free   # curated tables + LLM-written literature search
AMG_PUBMED_FULLTEXT=1                # read open full text
AMG_PAPER_DIGEST=1                   # per-paper paragraph reading
AMG_EVIDENCE_LEDGER=1                # confidence + contradiction flags
AMG_ANSWER_STYLE=adr
```

- Key files: `AMG-with-KG.py` (pipeline), `pgx_kb.py` (curated tables), `evidence_ledger.py`, `run_genmedgpt.py` (batch runner), `make_pgx_benchmark.py`, `eval_ledger.py`.
- Run: `sbatch run_adr_phase11_alliance.bash`, then `python eval_ledger.py`.
- Every query is logged in `search_shape`; every retrieved item in `evidence_log`; citations in `references`.

## Related

- [[RAG and Knowledge Graphs]]
- [[MindMap - KG Prompting for LLMs]], [[MedRAGChecker]], [[Contradictions in Context - Medical RAG]]
