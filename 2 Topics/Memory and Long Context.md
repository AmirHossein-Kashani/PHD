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
- **My design:** local knowledge = **path to the root** + compressed shared knowledge ([[AR 1 - Problem and Method]]).
- **Attention steering:** mark important text and push attention to it ([[PASTA - Attention Steering|PASTA]]), keep attention on the prompt ([[SPA - Selective Prompt Anchoring|SPA]]), re-anchor to plan steps ([[Self-Anchor (ATAR)]]). Review: [[Review - Attention by Highlighting]].

## Where it is used

- [[AR 0 - Start Here|Auto Researcher]]
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

- [[AR 6 - Paper Draft (Introduction)]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Reports and experiments (1)

- [[Selective Forgetting Pilot]] - First runs of the forgetting experiment on Fir and Nibi (arms N, A-F after a fork). Hypothesis untested: every fork came when the task was already solved; the test tasks are too easy for Opus 5.5.

### Papers (15)

- [[AiScientist - File-as-Bus Long-Horizon Engineering]] - Orchestrator plus a shared File-as-Bus workspace with append-only logs; 81.82% on MLE-bench Lite. No code.
- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[DeepScientist - Findings Memory]] - Bayesian-optimization research loop with a Findings Memory of all ideas, failed ones too; month-long runs (ICLR 2026).
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[Failure Registry - Recovering Wasted Compute]] - Plug-in failure registry with banned patterns, proven fixes and backtracking; AIDE golds 22 to 38.
- [[Kosmos - AI Scientist with World Model]] - Long runs with a shared structured world model; keeps conclusions, not paths. Commercial, no code.
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[MARS - Modular Agent with Reflective Search]] - MCTS over modular solutions with a Comparative Reflective Memory of causal lessons; 56.0% any medal on MLE-Bench (ICML 2026). Closest to our failure records.
- [[ML-Master 2.0 - Hierarchical Cognitive Caching]] - Three memory levels (traces, phase summaries with reasons, cross-task wisdom); 56.44% medals on full MLE-bench. Strongest memory rival.
- [[MLEvolve - Monte Carlo Graph Search]] - UCT tree with cross-branch reference edges and a success/failure memory; 65.3% any medal on full MLE-bench in 12 h.
- [[PASTA - Attention Steering]] - Lets the user mark important text and steers model attention to it, like bold text for humans.
- [[Reflexion]] - Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.
- [[ScienceFlow - Re-Anchoring Long-Horizon Agent]] - Continue or jump back to archived checkpoints with their evidence; 70.22% on full MLE-bench. Closest to reopening old ideas.
- [[Self-Anchor (ATAR)]] - Interleaves plan and action steps and steers attention back to the question and the current plan step.
- [[SPA - Selective Prompt Anchoring]] - Fixes "attention dilution" in long code generation by keeping attention on the user prompt.

### Literature reviews and lists (1)

- [[Review - Attention by Highlighting]] - Papers about making the model pay attention to marked parts of long context (Self-Anchor, SPA, PASTA).

