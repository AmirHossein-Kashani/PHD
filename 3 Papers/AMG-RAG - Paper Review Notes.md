---
type: paper
date: 2026-06-27
status: In Progress
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
notion-tags: ["FeedBack"]
tags: [type/paper, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/38dd68da32f080a9a0afdd8569e21499
created: 2026-06-28
updated: 2026-07-06
---
# AMG-RAG - Paper Review Notes

> [!summary] In short
> My review/feedback notes on the AMG-RAG paper (agentic medical KG for QA).

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Original:** [Notion page](https://app.notion.com/p/38dd68da32f080a9a0afdd8569e21499)

---

### Agentic Medical Knowledge Graphs Enhance Medical Question Answering: Bridging the Gap Between LLMs and Evolving Medical Knowledge

---

### Introduction
Despite their promise, LLMs face two persistent challenges.
First, they must remain factually current in a field where knowledge can become obsolete almost overnight.
Second, they must correctly model the intricate relationships among medical entities.
Knowledge Graph (KG) provide a structured and interconnected view that supports nuanced reasoning, yet creating and maintaining them by hand is costly.
We introduce an automated framework that constructs and continuously refines Medical Knowledge Graphs (MKGs) for QA.
Our system embeds confidence scoring mechanisms that explicitly model information uncertainty, providing transparent reliability assessments for medical information.

### Method AMG-RAG
**question parsing**
an LLM agent extracts medical terms {n1, n2, . . . , nm} from the user query q:
> {n1, n2, . . . , nm} = LLM(q, M), m ≤ M
> ??? M seems to be hard to specify in advance
@user : `M` caps the *number of extracted terms* (`m ≤ M`); a few lines later in node exploration (p. 4) the same `M` is a *document limit* ("until either cumulative confidence meets threshold τ or document limit M is reached"). it seems the notation is wrong, and this number is considered as hyper parameter. in some cases, the consider this parameter as maximum number of extracted point
> ??? Would it not make more sense to provide a knowledge graph K as input in addition to q?
@user : this algorithm consider KG as an input for the system which enable the framework to be compatible with dynamic KG, there is no change require for the system if the KG updated so it required to be given as input. the paper does not mentioned anything about the KG update or generation but it mention the size of extracted KG that they have been used.
**node exploration**
the system queries the KG for each term ni, applying a confidence threshold that filters relationships based on their reliability scores.
The system propagates confidence through the KG by computing child confidence as:
> s(nj) = s(ni) ° s(rij), ∀j ∈ children of i.
> ??? What is s? I suspect it is a confidence score.
@user 
`s` is a confidence score — Appendix A (p. 11): "A confidence score, sij, is assigned to each inferred relationship." So `s(ni)` is node confidence, `s(rij)` is relationship reliability, multiplied to propagate confidence to the child.
The exploration continues until either cumulative confidence meets threshold τ or document limit M is reached, ensuring comprehensive yet focused information gathering.
**chain-of-thought generation phase**
synthesizes reasoning traces ci for each entity by integrating information from connected nodes:
> ci = LLM(ni, {d(nj) | j ∈ connected nodes}).
**answer synthesis**
aggregates these reasoning traces to produce the final output â with an associated confidence score:
> â, ŝ = G({c1, c2, . . . , cm}).
> !!! It seems that we could use multi-agent debate for the task of answer synthesis, with one agent for each of the input papers. That way we could provide some reasoning for the result of the synthesis.

### Relationship Inference
- dynamically infer relationships between medical entities.

An LLM agent analyzes pairs of nodes (ni, nj) to determine potential relationships rij and quantify their reliability:
> rij, sij = LLM(d(ni), d(nj)).
This process generates not just connection types (e.g., causation, correlation, contraindication) but also confidence scores.
A confidence score, sij, is assigned to each inferred relationship, using the following scoring criteria:
- **10:** The target is directly and strongly related to the item, with clear, unambiguous relevance.
- **7-9:** The target is moderately to highly relevant to the item but may have some ambiguity or indirect association.
- **4-6:** The target has some relevance to the item but is weak or only tangentially related.
- **1-3:** The target has minimal or no meaningful connection to the item.
> ??? These are vague criteria. How to decide whether a relationship has, e.g., "clear, unambiguous relevance"?

@user : The paper mentioned LLm is deciding for this section

### Knowledge Graph Construction
Unlike traditional static knowledge bases, our method allows the graph structure to adapt continuously in response to new queries and the evidence they uncover.
Furthermore, it assigns a confidence score to each edge in the graph, indicating the reliability of each relationship.

### Comparison to our research
We also have a knowledge graph KG and a set of documents D as input.
Our query is not a string, from which medical terms need to be extracted, but an adverse drug reaction, and possibly a set of genes that should be considered as potential causes.
We want to extract evidence from each document in D separately. We can probably also use chain-of-thought prompting for that purpose.
We have domain knowledge that can be employed to compute the confidence of an ADR-gene relationship.
For example:
- confidence of a relationship obtained from a randomized controlled trial > confidence of a relationship obtained from observational data
- confidence of a relationship increases with increasing number of samples used to obtain the relationship

We also need to aggregate multiple reasoning chains, one from each document. We can probably use multi-agent debate for the task of answer synthesis, with one agent for each of the input papers.
> Do we also want to update the KG?
@user : If we have a paper that provide addition category to our KG or subject we have to update the graph. 

---
