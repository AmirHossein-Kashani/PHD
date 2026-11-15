---
type: paper
date: 2026-05-15
status: Done
projects: ["Early Exploration (May-June 2026)", "Auto Researcher"]
topics: ["Agent Architectures and Frameworks", "Memory and Long Context"]
tags: [type/paper, project/early-exploration, project/auto-researcher, topic/agent-architectures-and-frameworks, topic/memory-and-long-context]
notion: https://app.notion.com/p/362d68da32f080e9a5d2e166ed33b899
created: 2026-05-16
updated: 2026-05-16
---
# Reflexion

> [!summary] In short
> Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.

**Project:** [[Early Exploration (May-June 2026)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Agent Architectures and Frameworks]], [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Protein Generation]]  
**Original:** [Notion page](https://app.notion.com/p/362d68da32f080e9a5d2e166ed33b899)

---

> [!attachment] File in Notion: **Reflexion.pdf** (553.2 KiB) - [open the Notion page](https://app.notion.com/p/362d68da32f080e9a5d2e166ed33b899)

Traditional RL approaches for language agents have two big issues:
1. They need massive compute and training samples to fine-tune large models
1. Scalar/numeric rewards make it hard to figure out *what* went wrong (the credit assignment problem)

#### Architecture: Three Cooperating Models
Reflexion uses a modular design with three LLM-based components plus memory:
**1. Actor (Mₐ)** — Generates text and actions based on observations. Built on models like ReAct or Chain-of-Thought. It conditions its decisions on both short-term memory (current trajectory) and long-term memory (past reflections).
**2. Evaluator (Mₑ)** — Scores the Actor's outputs. Different tasks use different evaluators:
- Reasoning tasks → exact match grading
- Decision-making → heuristics or LLM-based classification
- Programming → self-generated unit tests

**3. Self-Reflection Model (Mₛᵣ)** — The key innovation. Takes the trajectory and reward signal, then generates a verbal reflection explaining *why* things went wrong and *how* to improve.
**Memory Component:**
- **Short-term memory** = the current trajectory (recent actions/observations)
- **Long-term memory** = a buffer of past self-reflections (typically capped at 1–3 entries due to context limits)

There is two llm in this architecture Actor and Self-Reflection
