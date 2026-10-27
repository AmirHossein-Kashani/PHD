---
type: paper
date: 2026-08-11
status: Done
projects: ["Auto Researcher", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Memory and Long Context", "Autonomous Research Agents"]
tags: [type/paper, project/auto-researcher, project/reversible-exploration, topic/memory-and-long-context, topic/autonomous-research-agents]
notion: https://app.notion.com/p/3b9d68da32f0805886fee5e0593b684f
created: 2026-08-11
updated: 2026-08-12
---
# Mage - Memory as Execution State

> [!summary] In short
> Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.

**Project:** [[Auto Researcher]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Memory and Long Context]], [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3b9d68da32f0805886fee5e0593b684f)

---

This paper focuses on how to store the knowledge it has negative opinion about RAG to store data for long hirizoning because it will lost the sequence of knowledge and the states.
they mention a two layer architecture about how to store data for steps one layer only store raw data about each node of movement, and the other layer can play as summary for multiple steps

![[mage-memory-as-execution-state-1.webp]]
