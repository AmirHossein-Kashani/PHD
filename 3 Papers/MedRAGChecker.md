---
type: paper
date: 2026-09-13
status: Done
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/3dad68da32f0806b8186fe3c703bb09d
created: 2026-09-13
updated: 2026-09-14
---
# MedRAGChecker

> [!summary] In short
> Breaks an answer into claims and checks each claim with NLI on retrieved text and with a biomedical KG (DRKG or BioPortal).

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - BC Children Related Works]]  
**Original:** [Notion page](https://app.notion.com/p/3dad68da32f0806b8186fe3c703bb09d)

---

![[medragchecker-1.webp]]

MedRAGChecker first breaks a generated answer into individual claims. Each claim is then verified using two complementary signals: textual NLI, which checks whether the retrieved evidence supports or contradicts the claim, and a biomedical knowledge graph, which checks whether the claim is consistent with structured biomedical knowledge. The system then selectively combines these two signals to produce the final verdict. The authors test two different KG backends: DRKG, which focuses mainly on drug–gene–disease and drug-repurposing relationships, and BioPortal, an online biomedical ontology platform accessed through an API.
