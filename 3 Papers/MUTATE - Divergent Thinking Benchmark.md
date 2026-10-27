---
type: paper
date: 2026-08-14
status: Done
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Creativity and Exploration", "Benchmarks and Datasets"]
tags: [type/paper, project/creativity-debate, project/auto-researcher, topic/creativity-and-exploration, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bcd68da32f0803aab4bc62c4c537910
created: 2026-08-14
updated: 2026-08-18
---
# MUTATE - Divergent Thinking Benchmark

> [!summary] In short
> Text sandboxes where the obvious solution fails. Measures path-level and action-level divergence of agents. Close to EscapeBench.

**Project:** [[Creativity in Multi-Agent Debate]], [[Auto Researcher]]  
**Topics:** [[Creativity and Exploration]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Creativity Measuring Benchmarks]]  
**Original:** [Notion page](https://app.notion.com/p/3bcd68da32f0803aab4bc62c4c537910)

---

The tasks in the **MUTATE** benchmark are structured as **interactive, text-based sandboxes** that act almost like puzzle or escape-room scenarios.
close to [[EscapeBench]] 

**How MUTATE Measures Creativity**
  MUTATE places agents in text-based sandboxes where the obvious solution intentionally leads to a dead end. It evaluates the agent across two dimensions:  
  1. **Path-Level Divergence:** Can the agent discover completely different mechanisms to solve a problem? For example, in the benchmark's "beehive" scenario, if the direct approach fails, does the agent think to smoke the bees out with a newspaper, lure them to another tree, or find a phone number for an expert?  
  2. **Action-Level Divergence (Divergence Momentum):** The benchmark scores every single Thought-Action step—even the failed ones. It measures if the agent can break conventional object affordances (e.g., using a heavy book as a hammer). Most importantly, it rewards agents that try wild, off-path ideas rather than just tweaking a failed conventional approach.
