---
type: paper
date: 2026-06-26
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Neuro-Symbolic Reasoning and Argumentation"]
tags: [type/paper, project/claim-conflict, topic/neuro-symbolic-reasoning-and-argumentation]
notion: https://app.notion.com/p/38bd68da32f0805185e2c9a7348b9a21
created: 2026-06-26
updated: 2026-06-26
---
# ArgLLM

> [!summary] In short
> Builds a tree of supporting and attacking arguments, scores them, and uses a formula to accept or reject a claim.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Neuro-Symbolic Reasoning and Argumentation]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Neuro-Symbolic Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/38bd68da32f0805185e2c9a7348b9a21)

---

### The system contains 3 steps
1. create supporter and attacker statement in structure of a tree 

![[argllm-1.svg]]

1. evaluate the fact and scoring them
    ![[argllm-2.svg]]
1. solver that calculate the support of the first statement or refuse score to make accept or reject decision
