---
type: paper
date: 2026-08-06
status: Done
projects: ["Auto Researcher", "Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/auto-researcher, project/reversible-exploration, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3b4d68da32f08071a8ecd0e98967ddb8
created: 2026-08-06
updated: 2026-08-06
---
# Biomni - General Biomedical Agent

> [!summary] In short
> Biomni builds a large biomedical action space (tools, databases, software) from 2,500 papers. Good for medical baselines.

**Project:** [[Auto Researcher]], [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3b4d68da32f08071a8ecd0e98967ddb8)

---

### Biomni: A General-Purpose Biomedical AI Agent
Huang et al. (Stanford, Genentech, Arc, UW, Princeton, UCSF) — bioRxiv 2025, *Science* 2026 (`science.adz4351`)

### Thesis
An **action-space engineering** paper. Inverts the usual agent-research setup: instead of fixing the environment (WebArena, SWE-bench, τ-bench) and iterating on the policy, it fixes a standard policy and treats *environment construction* as the research problem. Justified because biomedicine has no pre-existing universal interface — no "the browser," no "the shell." The action space had to be discovered, not wrapped.

### Two components
**Biomni-E1 (the environment — the real contribution).**
  Pipeline: bioRxiv's 25 subject categories × 100 most recent papers = 2,500 → LLM extraction agent pulls tasks/tools/databases/software needed to reproduce each → dedupe → **human expert validation**. Output: 105 software packages, 150 specialized biological tools (wet-lab protocols, ML predictors, domain know-how), 59 databases.
- Sampling strategy *is* the design: uniform-over-categories × recency buys coverage and currency; frequency-weighting would collapse onto whatever's hot.
- Semi-automated, not autonomous. Expert validation is in the loop and is probably where the cost sits. The "AI discovers the action space" framing oversells it.

**Biomni-A1 (the agent).** CodeAct-based. Retrieve → plan → execute by **generating and running Python** (LangGraph state machine, ReAct-style graph, self-critique). Code-execution over function-calling is the one meaningful design choice: JSON tool calls force one action per turn, code gives loops, conditionals, and DataFrame passing between tools in a single action. No new algorithm claimed.

### The retriever
**No retrieval algorithm in the IR sense.** Paper: a *prompt-based retriever powered by a separate LLM*. Code: `ToolRegistry` + `biomni/model/retriever.py` → `ToolRetriever` prompts an LLM with the query + the full catalog, parses the selection. No embeddings, no index, no top-k, no training. Stated motivation is purely context budget.
