---
type: review
date: 2026-06-12
status: In Progress
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs"]
tags: [type/review, project/multi-source-rag, topic/rag-and-knowledge-graphs]
notion: https://app.notion.com/p/37dd68da32f080aa96bdcd70a2ca0dc2
created: 2026-06-12
updated: 2026-06-16
---
# Review - Selected KG and RAG Papers

> [!summary] In short
> Selected papers that do documents to graph to LLM decision in one pipeline (MedGraphRAG, GraphCheck, AMG-RAG, ...).

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Stance-Aware KG Medical Fact Verification]]  
**Sub-notes:** [[MedGraphRAG]], [[GraphCheck]], [[AMG-RAG - Paper Summary]]  
**Original:** [Notion page](https://app.notion.com/p/37dd68da32f080aa96bdcd70a2ca0dc2)

---

- [x] MedGraphRAG

GraphCheck 
AMG-RAG (confidence)

Think-on-Graph, Reasoning-on-Graphs, KGARevion, and KRAGEN,

**The "do BOTH in one pipeline" set is small and biomedical-heavy.** MedGraphRAG, GraphCheck, KGARevion, KRAGEN, and AMG-RAG all run the documents/knowledge→graph→LLM-decision loop. They differ on (a) whether the KG is built from a *bounded corpus* vs. assembled from existing KGs, and (b) whether they emit confidence.
