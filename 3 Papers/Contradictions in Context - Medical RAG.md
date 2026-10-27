---
type: paper
date: 2026-09-14
status: Done
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
tags: [type/paper, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/3dbd68da32f0801a83e2e8320973f2ed
created: 2026-09-14
updated: 2026-09-14
---
# Contradictions in Context - Medical RAG

> [!summary] In short
> Shows that medical RAG is not reliable just because papers are relevant: similar papers can disagree. They filter out contradicting papers instead of explaining why.

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - BC Children Related Works]]  
**Original:** [Notion page](https://app.notion.com/p/3dbd68da32f0801a83e2e8320973f2ed)

---

+++ This paper tries to prioritize citations to find relevant documents alongside their publication dates. They incorporate random selection to introduce diversity among the top candidates. They also add a filtering layer that drops papers when they highly contradict other papers, which could otherwise pose problems for final decision-making. This is their most unusual claim: rather than trying to figure out why the papers contradict one another, they simply filter them out.

  The paper’s central idea is that **RAG is not automatically reliable just because it retrieves relevant medical papers**. Two PubMed papers can be highly semantically similar but still contain **contradictory conclusions**, especially if they come from different years. The authors therefore study how **outdated and contradictory evidence affects medical RAG** and whether LLMs can handle those conflicts
  
  

### What is innovative in the paper?
I would separate the innovation into **four main pieces**.
1. **A temporally diverse medical RAG benchmark**

They create a dataset connecting **TGA Consumer Medicine Information** with PubMed literature. They use six standardized questions for 1,476 medicines, producing 8,856 query instances, while PubMed evidence spans approximately **1975–2025**.
The important part is that they intentionally keep papers from different time periods rather than simply retrieving the newest or most similar documents.
This lets them experimentally study **knowledge evolution**.

---
1. **Temporal + citation-aware evidence selection**

Instead of simply keeping the top 20 most similar papers, they design a selection strategy that tries to cover different publication years.
If there are many years, they sample approximately across the whole time range. Within every year, they prefer highly cited papers. Then they use round-robin selection across years.
Conceptually:
Evidence quality=temporal coverage+citation importance\text{Evidence quality}
  =
  \text{temporal coverage}
  +
  \text{citation importance}
So rather than getting:

```text
2023
2023
2024
2024
2024
2025
```
they try to construct something more like:

```text
1995
1999
2003
2007
2011
2015
2019
2022
2024
2025
```
This makes it possible to observe how medical knowledge has changed.

---
1. **Temporal MMR retrieval**

They also modify MMR — **Maximal Marginal Relevance**.
Normal MMR considers:
relevance−redundancy\text{relevance} - \text{redundancy}
So you want documents that are relevant to the query without retrieving five almost identical documents.
The paper adds a temporal term:
S(d∣q)=α MMR(d∣q)+(1−α)τ(d)S(d|q)=
  \alpha\,MMR(d|q)
  +
  (1-\alpha)\tau(d)
where τ(d)\tau(d) represents temporal information.
So their retrieval ranking combines:
**Query relevance + document diversity + publication time**
rather than only semantic similarity.

---
1. **Contradiction-aware retrieval**

This is probably the most interesting methodological part.
They calculate how strongly PubMed documents contradict one another.
The procedure is roughly:

```text
Abstract A
      \
       → similar sentence pairs → Biomedical NLI → contradiction score
      /
Abstract B
```
They first represent scientific abstracts using **SPECTER**. Then, for two documents, they compare sentences and retain sufficiently similar sentence pairs. The threshold used in the paper is cosine similarity ≥ **0.75**.
Those sentence pairs go into:
**PubMedBERT-MNLI-MedNLI**
which produces probabilities for:
P(entailment),P(neutral),P(contradiction)P(\text{entailment}),
  P(\text{neutral}),
  P(\text{contradiction})
They then define the contradiction between two documents using the **maximum contradiction probability** among the matching sentence pairs.
That gives them something like:

```text
Paper A ↔ Paper B = contradiction 0.91
Paper A ↔ Paper C = contradiction 0.12
Paper A ↔ Paper D = contradiction 0.73
```
They then calculate an average contradiction salience for each document.
