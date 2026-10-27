---
type: paper
date: 2026-05-17
status: Done
projects: ["Early Exploration (May-June 2026)"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/early-exploration, topic/multi-agent-debate]
notion: https://app.notion.com/p/363d68da32f08067ae59d893a3b8f5c0
created: 2026-05-17
updated: 2026-06-11
---
# Multi-Agent Debate - First Notes

> [!summary] In short
> First short note on multi-agent debate (same idea as Du et al.).

**Project:** [[Early Exploration (May-June 2026)]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Protein Generation]]  
**Original:** [Notion page](https://app.notion.com/p/363d68da32f08067ae59d893a3b8f5c0)

---

> [!attachment] File in Notion: **Multiagent Debate.pdf** (1.6 MiB) - [open the Notion page](https://app.notion.com/p/363d68da32f08067ae59d893a3b8f5c0)

  Instead of relying on a single LLM call, run **multiple instances of an LLM in parallel**, have each generate an answer, then feed every agent the others' responses and ask it to revise. Repeat for several rounds. The agents effectively "debate" until they converge on a shared answer. Inspired by Minsky's *Society of Mind*.
