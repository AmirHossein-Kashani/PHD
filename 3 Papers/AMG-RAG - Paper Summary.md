---
type: paper
date: 2026-06-15
status: Done
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/381d68da32f080ccbc5eda1f366aae4e
created: 2026-06-16
updated: 2026-06-16
---
# AMG-RAG - Paper Summary

> [!summary] In short
> AMG-RAG: LLM agents build and update a medical KG from live sources and reason over it. Our multi-source project extends this system.

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Selected KG and RAG Papers]]  
**Original:** [Notion page](https://app.notion.com/p/381d68da32f080ccbc5eda1f366aae4e)

---

> [!attachment] File in Notion: **Agentic Medical Knowledge Graphs Enhance Medical Question Answering.pdf** (1.6 MiB) - [open the Notion page](https://app.notion.com/p/381d68da32f080ccbc5eda1f366aae4e)

They build **AMG-RAG** (Agentic Medical Graph-RAG), a medical question-answering system whose core trick is that LLM "agents" *automatically construct and continuously update* a Medical Knowledge Graph (MKG) from live sources, then do retrieval + chain-of-thought reasoning *over that graph* rather than over a flat vector store. The headline claim is that an ~8B-parameter system beats models 10–100× larger on two medical QA benchmarks, without any fine-tuning.
The problem they're attacking is twofold, and it's worth keeping in mind because it motivates every design choice:
1. **Knowledge staleness** — medical facts change fast; retraining LLMs or hand-curating KGs to keep up is expensive.
1. **Relational reasoning** — multi-hop medical questions (drug–gene interactions, comorbidities) need explicit relationships, which plain vector-similarity RAG doesn't capture.

A KG solves (2) but normally makes (1) worse because KGs are hand-built. Their contribution is automating the KG construction so you get the relational structure *without* the curation cost.

---

### The method, step by step
It helps to separate two pipelines that run **independently and asynchronously** — this decoupling is a key design point.

#### Pipeline A: Building the MKG (offline / background)
This is the "agentic" part. For a given medical query (or batch), the construction loop is:
1. **Medical entity extraction.** A "Medical Entity Recognizer" (MER) LLM agent parses the query and extracts domain entities `{n₁, …, nₘ}` (diseases, drugs, symptoms, etc.). These become candidate nodes.
1. **Node enrichment via search tools.** For each entity `nᵢ`, specialized search tools fetch contextual descriptions: `d(nᵢ) = Search(nᵢ; source)`. The two sources they test are **Wikipedia (Wiki-MKG)** and **PubMed (PubMed-MKG)**. This is the freshness mechanism — the graph pulls in current literature at build time.
1. **Relationship inference.** An LLM agent looks at pairs of nodes `(nᵢ, nⱼ)` and emits both a typed relationship and a confidence score: `rᵢⱼ, sᵢⱼ = LLM(d(nᵢ), d(nⱼ))`. Relationship types are things like causation, correlation, contraindication. Crucially, every *edge* carries a confidence score (1–10 scale; see step 5).
1. **Graph construction in Neo4j.** Entities, descriptions, relationships, and confidence metrics are assembled into a property graph stored in **Neo4j**. They include **bidirectional edges** (forward + reverse) for flexible traversal, and pair each item with a **summary + reliability indicator**.
1. **Confidence thresholding for quality control.** They apply a **confidence threshold of 8/10** to retain only high-reliability nodes/edges — empirically tuned. The scoring rubric (Appendix A): 10 = direct/unambiguous, 7–9 = moderate/indirect, 4–6 = weak/tangential, 1–3 = minimal/none. This is their answer to the "automated KG generation introduces hallucinated/noisy edges" risk — confidence becomes a filter and a propagation weight.

Because this runs in the background, the QA system at inference time mostly reads from a *pre-populated* graph, which keeps inference latency and search-tool dependency low. The final assembled graphs are large: **~76,681 nodes and ~354,299 edges**.

#### Pipeline B: Inference / QA (the RAG+CoT part)
This is Algorithm 1 in the paper. Given a query `q`:
1. **Extract medical terms** from `q` (same MER agent), initialize confidence `sᵢ = 1.0` per term.
1. **Graph-conditioned retrieval / traversal.** For each term, query the KG for relevant nodes and relationships. Confidence **propagates multiplicatively** along edges:
  `s(nⱼ) = s(nᵢ) · s(rᵢⱼ)`.
  A child node is only added to the exploration set if its propagated confidence `sⱼ ≥ τ` (threshold). They support both **breadth-first and depth-first** traversal, chosen by query characteristics. Exploration stops when cumulative confidence hits `τ` or a document limit `M` is reached.
1. **Chain-of-thought generation per entity.** For each entity, an LLM synthesizes a reasoning trace from its connected nodes:
  `cᵢ = LLM(nᵢ, {d(nⱼ) | j ∈ connected nodes})`.
1. **Answer synthesis.** All traces are aggregated into a final answer with a confidence score:
  `â, ŝ = G({c₁, …, cₘ})`.

So the structural contrast with vanilla RAG (their Fig. 2D) is: vanilla RAG retrieves top-k flat chunks by vector similarity and concatenates them; AMG-RAG instead walks a confidence-weighted typed graph, generates per-entity reasoning, then composes. The graph gives you explicit multi-hop paths *and* interpretability (you can read the edges and confidence scores that led to an answer).
A few implementation details worth noting as a CS reader:
- **Backbone LLM is GPT-4o-mini** (~8B params, per their accounting) for both graph building and QA. No fine-tuning anywhere.
- The vector-DB side (Appendix D) uses **all-mpnet-base-v2** SentenceTransformer embeddings stored in **Chroma**, with 512-token chunks and 100-token overlap, batches of up to 10k. This is the textbook-ingestion path that complements the graph.
- Validation (Appendix B) was done by both expert LLMs (e.g., GPT-4 scoring graph quality ~8.5–8.9/10 across node ID, relationship relevance, summary clarity) and human clinicians.

---

### Datasets, benchmarks, and what they compared against

#### Datasets / benchmarks (both multiple-choice medical QA)
- **MEDQA** — derived from professional medical board exams; ~1,200 test samples used. Reported metric is F1 (and accuracy). This is the harder, reasoning-heavy benchmark.
- **MedMCQA** — broader multi-specialty medical QA; ~4,000 dev-set questions used. Reported metric is accuracy.

Their results: **F1 74.1% on MEDQA** (73.9% accuracy) and **66.34% accuracy on MedMCQA**.

#### Baselines they compared against
On **MEDQA** (Table 1), a mix of much larger and domain-specific models:
- Med-Gemini (~1800B), GPT-4 (~1760B), Med-PaLM 2 (~340B, full and 5-shot) — the big frontier models that score higher (89–91 F1) but at 100–200× the parameters.
- Meerkat (7B), Meditron (70B), Flan-PaLM (540B), LLAMA-2 (70B), Shakti-LLM (2.5B), Codex 5-shot CoT, BioMedGPT (10B), BioLinkBERT. Their key bragging point: AMG-RAG (8B, 74.1 F1) beats Meditron-70B (68.3 F1) and Meerkat-7B on F1 with no fine-tuning.

On **MedMCQA** (Table 2): Meditron-70B, Codex 5-shot, VOD, Flan-PaLM (540B), PaLM (540B), GAL (120B), and the BERT-family encoders (PubMedBERT, SciBERT, BioBERT, BERT). AMG-RAG edges out Meditron-70B (66.34 vs 66.0).
A **RAG-specific head-to-head** (Table 3) — this is the more meaningful comparison for your purposes, since it isolates the retrieval strategy rather than model scale:
- **Self-RAG (8B)** — 67.32%
- **HyDE (8B)** — 68.32%
- **GraphRAG** (Gemini-pro / 70B / 8B) — up to 65.1%
- **MedRAG** (70B / 13B) — up to 49.57%
- **PMC-LLaMA (13B)** — 44.38%
- **AMG-RAG (PubMed-MKG, 8B)** — **73.92%**, the top score.

#### Ablations (the part a CS reviewer cares about most)
These isolate where the gains come from:
- **Search source:** PubMed-MKG (73.92%) > Wiki-MKG (70.62%) — domain-specific source matters.
- **Remove search:** drops to 67.16%.
- **Remove CoT/MKG:** drops to 66.69%. So both structured retrieval *and* CoT reasoning are load-bearing; neither alone explains the result.
- **Backbone swap (Table 4):** GPT-4o-mini (73.92%) > LLaMA-3.1 (66.5%) > Mixtral 8×7B (61.4%) on the same pipeline — they argue domain alignment beats raw size.
- **Domain-shift test (Fig. 4):** stronger gains in fast-moving subfields (Neurology, Genetics), which is the whole "freshness" thesis paying off.

---
The honest caveat as a CS reader: the "~8B" claim for GPT-4o-mini is the authors' estimate and is debatable, and the parameter-count framing flatters the comparison since their pipeline calls an external search tool and a Neo4j graph that the baselines don't get. The cleaner evidence for their actual contribution is the Table 3 RAG comparison and the ablations, where everything is held at 8B and AMG-RAG still wins.
