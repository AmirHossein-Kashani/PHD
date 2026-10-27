---
type: paper
date: 2026-05-13
status: Done
projects: ["Multi-Source Evidence RAG", "Early Exploration (May-June 2026)"]
topics: ["RAG and Knowledge Graphs"]
tags: [type/paper, project/multi-source-rag, project/early-exploration, topic/rag-and-knowledge-graphs]
notion: https://app.notion.com/p/35ed68da32f080a4857bde020ca7a7fc
created: 2026-05-12
updated: 2026-05-25
---
# MindMap - KG Prompting for LLMs

> [!summary] In short
> MindMap: extract path-based and neighbor-based evidence subgraphs from a KG, then let the LLM reason over them. Used as a baseline in the multi-source project.

**Project:** [[Multi-Source Evidence RAG]], [[Early Exploration (May-June 2026)]]  
**Topics:** [[RAG and Knowledge Graphs]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Original:** [Notion page](https://app.notion.com/p/35ed68da32f080a4857bde020ca7a7fc)

---

The core idea is to augment decision-making context with two complementary subgraphs extracted from entities in the query. The pipeline runs in three stages, using three LLM calls plus a BERT matching step and graph search.
**Step I — Evidence Graph Mining.** The LLM extracts candidate entities from the question (call #1), which are then aligned to KG nodes via BERT similarity to produce a verified entity set V_q. Two subgraph types are extracted: **(1) path-based**, tracing up to k hops between pairs of query entities (storing segments when no full path exists), and **(2) neighbor-based**, gathering 1-hop neighbors with a conditional 2nd hop for semantically related ones. Clustering prunes the result for diversity.
**Step II — Evidence Graph Aggregation.** Each subgraph is flattened into an entity chain (e.g., `Fatigue → is_symptom_of → Cirrhosis → has_symptom → Stomach bloating`) and tagged with a citation ID (P1, P2… for paths; N1, N2… for neighbors). A second LLM call rewrites each chain as a natural-language sentence with quoted entity and relation names — e.g., *"'Fatigue' may be a symptom of 'Cirrhosis'..."* The path and neighbor streams are kept separate, not merged.
**Step III — LLM Reasoning with the Mind Map.** A third LLM call receives a five-part prompt: system instruction, question, path evidence, neighbor evidence, and graph-of-thought instructions with exemplars. The prompt explicitly tells the model to "combine with the knowledge you already have" — an instruction worth +8.2% accuracy in the paper, since it pushes the LLM to fuse retrieved evidence with implicit knowledge instead of parroting the KG. The output has three parts: a **summary answer** (diagnosis, tests, drugs), an **inference chain** citing evidence sources (e.g., `P1(...)→N2(...)→result1(...)`), and a **mind map** — a decision tree where each node is tagged with its evidence ID, and LLM-added knowledge is marked with dashed lines.

Main Trick
- Find the enitiy (related node) by LLM and Cosine distance Bert
- extract relevant data from graph by finding paths and neighborhoods
- convert the data to humanised version by LLM
- attaching everything together and ask about steps and tree that would interpret the final decison
