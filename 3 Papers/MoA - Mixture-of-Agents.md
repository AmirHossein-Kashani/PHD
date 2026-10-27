---
type: paper
date: 2026-07-20
status: Done
projects: ["Claim-Level Conflict in Debate", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, project/reversible-exploration, topic/multi-agent-debate]
notion: https://app.notion.com/p/3a3d68da32f080a08880c067ae57788d
created: 2026-07-20
updated: 2026-07-21
---
# MoA - Mixture-of-Agents

> [!summary] In short
> Layers of proposer LLMs and aggregator LLMs. Agents work separately and aggregators control the flow.

**Project:** [[Claim-Level Conflict in Debate]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3a3d68da32f080a08880c067ae57788d)

---

The most important difference is that llms are working seperately and aggregators are controlling the flow compare to reconcile that try to solving sequentially.
- **Layer 1 (Proposers):** Several different LLMs each independently answer the user's prompt.
- **Middle layers (Aggregators):** All those answers are concatenated and fed into another set of LLMs along with the original prompt, using an "Aggregate-and-Synthesize" system prompt telling the model to critically evaluate the previous responses and produce a refined answer.
- **Final layer:** A single aggregator model produces the final output.

![[moa-mixture-of-agents-1.webp]]
