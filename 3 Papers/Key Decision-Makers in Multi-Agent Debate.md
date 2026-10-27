---
type: paper
date: 2026-06-13
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/37ed68da32f080a3ad76d1788c13f952
created: 2026-06-13
updated: 2026-06-13
---
# Key Decision-Makers in Multi-Agent Debate

> [!summary] In short
> Speaking order matters: agents who speak later have more power over the final answer.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - State of the Art Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/37ed68da32f080a3ad76d1788c13f952)

---

> [!attachment] File in Notion: **29418-AAAI26.ZhangQ-MS.pdf** (1.6 MiB) - [open the Notion page](https://app.notion.com/p/37ed68da32f080a3ad76d1788c13f952)

personal opinion
papers working on order of conversation and they mention it is important that who talks last, they try to give the more important positions to the agents that has concistency which i think, we could find some metric better that what they have mentioned.

### Paper Overview: The Power of Speaking Order
- **The Problem:** While Multi-Agent Debate (MAD) frameworks enhance Large Language Model (LLM) reasoning, they often overlook how the speaking order of agents impacts the final decision.
- **The Discovery:** The authors discovered that role allocation is a critical scaling dimension; agents that speak *later* in a debate exert significantly more influence over the final outcome, regardless of whether they are correct.
- **The Ideal Scenario:** An idealized "Truth Last" strategy—where the agent with the correct viewpoint is forced to speak at the end of the sequence—improved MAD reasoning performance by up to 22%.

#### The Approach: Multi-Agent Debate Consistency (MADC)
Because the correct answer is unknown in real-world applications, the authors proposed the **Multi-Agent Debate Consistency (MADC)** strategy to simulate the "Truth Last" effect.
- **Path Consistency Evaluation:** The system tracks each agent's reasoning sequence ("Debating Path") and calculates a "Path Consistency" score after each round. This score measures how likely different independent agents are to converge on the same intermediate conclusion.
- **Dynamic Reordering:** High consistency indicates a more reliable viewpoint. MADC dynamically reorders the agents before the next debate round, placing agents with the highest consistency scores at the end of the speaking sequence.
- **Plug-and-Play Integration:** MADC is entirely orthogonal to existing multi-agent frameworks. It operates purely on the backend by altering the communication sequence, requiring zero modifications to the agents' context or profiling prompts.

#### Datasets and Benchmarks
To validate the robustness of MADC, the authors tested it across 9 different LLMs (including GPT-4o-mini, Qwen2.5, and DeepSeek-R1-Distill) using the following reasoning benchmarks:
- **MATH500:** A dataset consisting of 500 diverse mathematical reasoning problems.
- **Logical Deduction:** A complex logical reasoning subtask extracted from the Big-Bench Hard (BBH) benchmark.
- **Geometric Shapes:** A spatial reasoning subtask, also extracted from the Big-Bench Hard (BBH) benchmark.
