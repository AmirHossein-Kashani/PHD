---
type: paper
date: 2026-06-13
status: Done
projects: ["Claim-Level Conflict in Debate", "Multi-Source Evidence RAG"]
topics: ["Multi-Agent Debate", "RAG and Knowledge Graphs"]
tags: [type/paper, project/claim-conflict, project/multi-source-rag, topic/multi-agent-debate, topic/rag-and-knowledge-graphs]
notion: https://app.notion.com/p/37ed68da32f0800896aafb35567aa473
created: 2026-06-13
updated: 2026-06-16
---
# DoM - Debate over Mixed-knowledge

> [!summary] In short
> For incomplete KGs: a KG agent and a text (RAG) agent work in parallel and a judge decides.

**Project:** [[Claim-Level Conflict in Debate]], [[Multi-Source Evidence RAG]]  
**Topics:** [[Multi-Agent Debate]], [[RAG and Knowledge Graphs]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - State of the Art Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/37ed68da32f0800896aafb35567aa473)

---

The main idea of this paper is running RAG and KG inferencing in parallel and at the end a judge will make the final decision.

This paper introduces a multi-agent framework for Incomplete Knowledge Graph Question Answering (IKGQA). It tackles the problem where traditional KGQA fails because real-world knowledge graphs are missing facts or entities.
Here is the technical breakdown of the paper's core contributions:

#### 1. The Multi-Agent Architecture (DoM)
The authors built "Debate over Mixed-knowledge" (DoM). It is a multi-agent reasoning loop that integrates structured graph data and unstructured text.
    **Initialization & Planning:** An LLM decomposes the user query into a sequence of sub-questions. The system maintains a memory state to track intermediate answers.
- **Parallel Inference:** Two specialized agents execute independently. A **KG Agent** performs entity linking and multi-hop relational searches on the structured graph. A **RAG Agent** retrieves top-k unstructured text chunks from external sources like Wikipedia.
    **Judge Agent Routing:** A judge evaluates the conflicting or complementary outputs from both retrieval agents. It aggregates the evidence, outputs an intermediate answer, and dynamically updates the next sub-questions based on the current state.
    **Final Generation:** A verifier LLM compiles the global memory into the final answer.

#### 2. A New Benchmark Dataset (IKGWQ)
Existing datasets test incompleteness by randomly deleting edges from the graph. This is synthetic and predictable. The authors created the Incomplete Knowledge Graph WebQuestions (IKGWQ) dataset. They used real-world knowledge updates to inject missing entities and relations that fall entirely outside the static graph.

**CWQ (ComplexWebQuestions)** and **2. WebQSP (WebQuestionsSP)** — These are existing benchmarks from Xu et al. (2024). The authors use 1,000 sampled questions from each. To simulate incompleteness, they create four variants by randomly removing 20%, 40%, 60%, or 80% of the crucial triples that appear in the gold relation path (denoted IKG-20% / 40% / 60% / 80%). The main results table reports the IKG-40% versions.

#### 3. Empirical Results
DoM consistently outperforms previous retrieval-augmented graph baselines like **ToG, PoG, and GoG**. It achieved a 70.7% relative improvement on their new **IKGWQ** dataset. The multi-agent debate effectively prevents the LLM from hallucinating when the underlying graph lacks essential topological links.
The authors evaluated DoM against top-tier state-of-the-art baselines (**like GoG, ToG, and RoG**) across both traditional datasets and their new benchmark
