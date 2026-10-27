---
type: report
date: 2026-07-08
status: Not started
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate", "Neuro-Symbolic Reasoning and Argumentation"]
notion-tags: ["proposal"]
tags: [type/report, project/claim-conflict, topic/multi-agent-debate, topic/neuro-symbolic-reasoning-and-argumentation]
notion: https://app.notion.com/p/397d68da32f080f7a5c0c7b171b882ab
created: 2026-07-08
updated: 2026-07-21
---
# Gaps Protocol - Claim Conflict Proposal

> [!summary] In short
> Proposal: debate systems vote on whole answers and miss which claims conflict. Idea: extract claims, detect and classify conflicts, build a claim-conflict graph, resolve each conflict with the right method. Includes my own manager-agent idea.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]], [[Neuro-Symbolic Reasoning and Argumentation]]  
**From meeting:** [[Claim-Level Conflict in Debate#2026-07-07 One-on-One|2026-07-07 One-on-One]]  
**Original:** [Notion page](https://app.notion.com/p/397d68da32f080f7a5c0c7b171b882ab)

---

- **Current limitation:** Multi-agent debate systems usually rely on majority vote or answer-level aggregation.
- **Main gap:** They do not identify which specific claims conflict for non binary decision and also they would attach unnecessary information. 
- **Additional gap:** Different conflict types are treated the same, although they may require different resolution strategies.
- **Key conflict types:** Factual, evidential, definitional, logical, reasoning-path, and value-based conflicts.
- **Core bottleneck:** Reliable claim extraction and cross-agent conflict detection in long, messy debate transcripts.
- **Proposed direction:**
    - Extract claims from each agent.
    - Detect conflicting claims.
    - Classify the conflict type.
    - Build a claim-conflict graph.
    - Resolve each conflict using the appropriate method.
    - Compose the final answer from the resolved claims.
- **Possible resolution methods:**
    - Retrieval for factual conflicts.
    - Solvers or verification for logical conflicts.
    - Clarification for ambiguity.
    - Decision protocols for value conflicts.
- **Main contributions:**
    - Conflict taxonomy and annotated benchmark.
    - Reliable cross-agent conflict detector.
    - Claim-level conflict-aware aggregation framework.

---
my idea
- [ ] Agents will tag the claims or statement and sperately gave us their final statement
- [ ] we would have a manager that try to find conflicts and create a method for validity or conversation, it can creates sum tags and assign the agent to a local conversation,
  the most important section of this manager is that it will peak the related data to reduce the cost of computations.
- [ ] long context problem can be solved by
    - [ ] manager will peak a section by using tags when generate or when manager read the sections
    - [ ] we can work on attention mechanism 
        - [ ] bold regions 
        - [ ] reader bold regions

  - the final method would be
- [ ] output augmentation protocol to enhance the collaboration of agents
- [ ] architecture of conversation that match the best with system.
