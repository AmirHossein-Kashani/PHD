---
type: topic
aliases: [neuro-symbolic, argumentation, ArgLLM]
tags: [type/topic, topic/neuro-symbolic-reasoning-and-argumentation]
---
# Neuro-Symbolic Reasoning and Argumentation

> [!summary] In short
> Mix neural models (LLMs) with **symbolic tools** (logic solvers, formal argumentation, knowledge graphs) so that decisions are **checkable and explainable**. In debate, this means: LLMs make arguments, and a fixed formula or solver decides, instead of an opaque LLM judge.

## Key ideas from my notes

- **Formal argumentation + LLM debate** (Toni's group, Imperial): [[ArgLLM]] builds support/attack trees and scores them; [[MArgE]] merges argument trees from many LLMs (binary decisions only).
- **Solvers as judges:** [[MAD-Logic]] (symbolic and neural solvers debate), Logic-LM line, Z3, theorem provers.
- **KGs for grounding:** [[Debate on Graph (DoG)]].
- **Foundations to cite:** Dung (1995) abstract argumentation; QBAF / gradual semantics; judgment aggregation (Chen and Endriss).
- Full review: [[Review - Neuro-Symbolic Multi-Agent Debate]].

## Where it is used

- [[Claim-Level Conflict in Debate]] (conflict graph and scoring)
- [[Multi-Source Evidence RAG]] (rule-based confidence instead of LLM scores)

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-06-23 One-on-One

*Original: [Notion page](https://app.notion.com/p/388d68da32f080e0a389d4f9502a1fd1)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-23 One-on-One|Multi-Agent Debate]], [[Early Exploration (May-June 2026)#2026-06-23 One-on-One|Early Exploration (May-June 2026)]], [[MS - Meeting Log#2026-06-23 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-23 One-on-One|Skills and Tools]]

What i have dont
- [x] 4 paper about neuro symbolic
    - [[ArgLLM]] 
    - [[Debate on Graph (DoG)]] 
    - [[MArgE]] 
    - [[MAD-Logic]]

### 2026-06-01 Weekly Group Meeting

> [!summary] This meeting in short
> Group meeting topic: logical reasoning for LLMs.

*Original: [Notion page](https://app.notion.com/p/373d68da32f080b5bc80e01d7d03e57d)*

## Notes on this topic

### Reports and experiments (2)

- [[Gaps Protocol - Claim Conflict Proposal]] - Proposal: debate systems vote on whole answers and miss which claims conflict. Idea: extract claims, detect and classify conflicts, build a claim-conflict graph,...
- [[Suggested Papers for Gaps Protocol]] - Reading list in tiers A to E for the claim-conflict proposal (SMADE-IE, MACI, MAD-Fact, DebateCV, argumentation theory, baselines).

### Papers (5)

- [[ArgLLM]] - Builds a tree of supporting and attacking arguments, scores them, and uses a formula to accept or reject a claim.
- [[Debate on Graph (DoG)]] - KG question answering with debate to avoid long reasoning paths and false-positive relations.
- [[MAD-Logic]] - Symbolic solvers and LLM solvers debate in a loop for logical reasoning, with pruning of low-confidence talk.
- [[MArgE]] - Debate version of ArgLLM: LLMs build pro/con argument trees that are merged and scored. Only binary decisions.
- [[SMADE-IE]] - Sparse multi-agent debate with evidence for zero-shot information extraction (entities and relations). Good trick, but a different problem.

### Literature reviews and lists (1)

- [[Review - Neuro-Symbolic Multi-Agent Debate]] - Literature review of neuro-symbolic methods in multi-agent debate: argumentation, symbolic solvers as judges, KG grounding.

