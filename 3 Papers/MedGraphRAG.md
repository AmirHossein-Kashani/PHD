---
type: paper
date: 2026-06-15
status: Done
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/381d68da32f0809e9b4ad15e1846c6e6
created: 2026-06-16
updated: 2026-06-16
---
# MedGraphRAG

> [!summary] In short
> Graph RAG for medicine with a three-tier KG (patient data, literature, UMLS) and U-Retrieval. Too complex for us.

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Selected KG and RAG Papers]]  
**Original:** [Notion page](https://app.notion.com/p/381d68da32f0809e9b4ad15e1846c6e6)

---

![[medgraphrag-1.webp]]

---

### Overview
MedGraphRAG is a graph-based RAG framework for the medical domain. It addresses two core limitations: standard RAG fails at holistic reasoning across long documents, and standard GraphRAG cannot produce evidence-backed, source-traceable responses. The system fuses private patient records (G1), medical literature (G2), and UMLS vocabulary (G3) through a three-tier knowledge graph. Its two key innovations are **Triple Graph Construction** and **U-Retrieval**.

---

### Method — 6-Step Pipeline

#### Offline Phase (Steps 1–5): Building the Graph
- **Step 1 — Semantic Chunking:** Paragraphs are grouped into topic-consistent chunks using an LLM with a sliding window of 5 paragraphs (FIFO), avoiding naive fixed-size splitting.
- **Step 2 — Entity Extraction:** The LLM extracts entities per chunk as `{name, type, context}` triples. Types are constrained to UMLS semantic types. No connections yet — pure node population.
- **Step 3 — Triple Linking:** Each user entity (G1) is linked to relevant paper entities (G2) and UMLS definitions (G3) via cosine similarity of content embeddings (threshold δr = 0.5), forming traceable triples `[user entity → source paper → UMLS definition]`.
- **Step 4 — Relationship Linking:** The LLM generates relationship phrases between entity pairs within each chunk, informed by their paper references from Step 3. Output per chunk is a Meta-MedGraph `Gm = {Em, R(Em)}`.
- **Step 5 — Tag the Graphs:** Each Meta-MedGraph is summarized using predefined medical tag categories (Symptoms, Medication, Body Functions, etc.). Agglomerative clustering groups related graphs into a multi-layer tag hierarchy (up to 12 layers), replacing GraphRAG's expensive community detection.

#### Online Phase (Step 6): U-Retrieval
The LLM generates tags for the user query and descends the tag hierarchy layer-by-layer to find the most relevant Meta-MedGraph **(top-down)**. It retrieves the top 60 entities with 16-hop triple neighbors and generates an initial response. It then walks back up the hierarchy, refining the response at each level using broader tag summaries **(bottom-up)**. This U-shape balances specificity with comprehensive context.

---

### Evaluation

#### Datasets & Benchmarks

| Category | Datasets |
|---|---|
| Medical Q&A (9 datasets) | MedQA, MedMCQA, PubMedQA, MMLU (Clinical Med, College Bio, Pro-Med, Anatomy, Med Genetics, College Med) |
| Health Fact-Checking (2) | FakeHealth, PubHealth |
| Long-form Generation (1) | DiverseHealth (self-collected) — 50 real-world clinical questions |

#### Compared Against

| Type | Models / Methods |
|---|---|
| Retrieval baselines | No retrieval, Standard RAG (LangChain), GraphRAG (Microsoft Azure) |
| Backbone LLMs | Llama2-13B/70B, Llama3-8B/70B, Gemini-pro, GPT-4 |
| SOTA Medical LLMs | Med-PaLM 2, Med-Gemini (both fine-tuned) |
| Long-form baselines | Inline Search, ATTR-FIRST, MIRAGE |

#### Key Results
- ~10% improvement over no-retrieval on fact-checking; ~8% on medical Q&A.
- ~8% over GraphRAG on fact-checking; ~5% on medical Q&A.
- With GPT-4 or Llama3-70B, surpasses fine-tuned Med-PaLM 2 and Med-Gemini — new SOTA on all 11 benchmarks.
- Human evaluation: highest scores across all 5 dimensions (pertinence, correctness, citation precision, citation recall, understandability).
