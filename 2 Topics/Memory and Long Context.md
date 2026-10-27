---
type: topic
aliases: [memory, long context, attention steering, long horizon]
tags: [type/topic, topic/memory-and-long-context]
---
# Memory and Long Context

> [!summary] In short
> Long tasks need a lot of context, but models get worse as context grows (they "lose the middle"). Agents therefore **compress** their history (summaries, best nodes, reflections). The risk: they lose *why* they made past decisions. Two ways to help: **better memory structure** (what to keep and load) and **attention steering** (make the model look at the important parts).

## Key ideas from my notes

- **Numbers to remember:** in long-context tests without literal cues, 11 of 13 models fell below half of their short-context accuracy at 32K tokens (NoLiMa). In our pilot, about 77% of model usage was re-reading context.
- **Memory designs:** summaries and retrieval (Generative Agents, MemGPT), reflections on failures ([[Reflexion]]), two-layer raw + summary memory ([[Mage - Memory as Execution State|Mage]]), git-like branch memory (GCC, AgentGit), rollback (ChronoMem), evidence graphs ([[EviGraph - Evidence-Guided Research Agents|EviGraph]]), persistent hypothesis tree ([[Arbor - Hypothesis Tree Refinement|Arbor]]).
- **My design:** local knowledge = **path to the root** + compressed shared knowledge ([[AR - Method and Contributions]]).
- **Attention steering:** mark important text and push attention to it ([[PASTA - Attention Steering|PASTA]]), keep attention on the prompt ([[SPA - Selective Prompt Anchoring|SPA]]), re-anchor to plan steps ([[Self-Anchor (ATAR)]]). Review: [[Review - Attention by Highlighting]].

## Where it is used

- [[Auto Researcher]]
- [[Claim-Level Conflict in Debate]] (bold regions idea)
- [[Reversible Exploration (ARIADNE and Retro-Fork)]]

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-06-30 One-on-One

> [!summary] This meeting in short
> LangGraph course done. Read PASTA and Self-Anchor (attention steering). Notes on course enrolment and "contribution before everything".

*Original: [Notion page](https://app.notion.com/p/394d68da32f080448531f6569d9d1634)*  
*Other parts of this meeting:* [[Skills and Tools#2026-06-30 One-on-One|Skills and Tools]]

- [x] [[PASTA - Attention Steering]] 
- [x] [[Self-Anchor (ATAR)]] 
- [x] [[PASTA - Attention Steering]]

## Notes on this topic

### Drafts (1)

- [[AR Paper - Introduction Draft]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Papers (7)

- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[PASTA - Attention Steering]] - Lets the user mark important text and steers model attention to it, like bold text for humans.
- [[Reflexion]] - Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.
- [[Self-Anchor (ATAR)]] - Interleaves plan and action steps and steers attention back to the question and the current plan step.
- [[SPA - Selective Prompt Anchoring]] - Fixes "attention dilution" in long code generation by keeping attention on the user prompt.

### Literature reviews and lists (1)

- [[Review - Attention by Highlighting]] - Papers about making the model pay attention to marked parts of long context (Self-Anchor, SPA, PASTA).

