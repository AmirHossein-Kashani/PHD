---
type: paper
date: 2026-08-06
status: Done
projects: ["Auto Researcher", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Autonomous Research Agents"]
tags: [type/paper, project/auto-researcher, project/reversible-exploration, topic/autonomous-research-agents]
notion: https://app.notion.com/p/3b4d68da32f0801fb7f7e30c74b55da3
created: 2026-08-06
updated: 2026-08-13
---
# AIDE - ML Engineering as Code Search

> [!summary] In short
> AIDE searches over whole Python scripts with a solution tree. No shared knowledge, no merging. Close to our idea and a search baseline.

**Project:** [[AR 0 - Start Here|Auto Researcher]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3b4d68da32f0801fb7f7e30c74b55da3)

---

![[aide-ml-engineering-as-code-search-1.webp]]

Paper initially start by branching to potential solutions of the model and then try to solve it graph abstraction framework. they don’t have share knowledge and only take information from their parrents. no communictation and no merging exist. in each step they decide which no is good enough to continue some node leaved some node decide to improve and based on validation score we will decide which final valid node is the best. 

#### AIDE in Brief
**The core idea:** Treat machine learning engineering not as a conversation with an agent, but as an **optimization problem over code**. The thing being searched isn't hyperparameters or architectures — it's the entire Python script. Find s* = argmax h(s), where h is a validation metric.
**How it works:** AIDE builds a **solution tree** where every node is a complete, runnable script and every edge is one modification attempt. Three components drive the loop:
1. **Search policy (π)** — a simple hard-coded rule deciding what to do each step: *draft* new solutions from scratch (early on, for diversity), *debug* a broken script (up to a depth limit), or *improve* the current best node (the default, greedy).
1. **Coding operator (f)** — the LLM, invoked with one of three specialized prompts matching those modes. Improvements are deliberately **atomic** (one change at a time, e.g., swap the optimizer) so each change's effect on the score is measurable.
1. **Summarization operator (Σ)** — instead of stuffing the whole history into context, it feeds the LLM a compact digest of past attempts: metrics, hyperparameters, debugging hints. Each LLM call is therefore **stateless and local** — small prompt, one focused job.
