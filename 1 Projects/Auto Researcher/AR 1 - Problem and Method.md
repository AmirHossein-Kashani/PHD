---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context, topic/creativity-and-exploration]
---
# AR 1 - Problem and Method

Part of [[AR 0 - Start Here|Auto Researcher]]. Sources: [[AR 6 - Paper Draft (Introduction)]], [[AR 7 - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]], [[AR 7 - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[AR 7 - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]].

> [!summary] In short
> Research agents fail on long runs for four reasons: long context, weak planning, memory that loses the path, and too little exploration. We keep the **same model and the same research loop** (Arbor) as the baselines and change only the **research state** the agent reasons over: a path to the root with undo conditions, a trajectory used as a control signal, and failure records.

---

## 1. The problem in simple words

A research run is a very long chain of steps. This causes four problems for today's research agents:

1. **Long horizon means long context.** The agent must remember many steps. Models get worse when the context gets long. In our pilot, about **77% of all model usage was agents re-reading their own context**.
2. **Long-term planning is weak.** LLMs are not good planners. In our pilot, the version with optional planning stopped early with budget left. The version with forced plan reviews was the only one that revised blocked ideas.
3. **Memory loses the path.** Agents compress history (a best node, a summary). They keep results but lose *why* they are where they are, what they promised, and what evidence would make them go back.
4. **Agents do not explore enough.** They defend their own ideas (fixation). When exploration tools are optional, they are rarely used.

## 2. Our setting: a Scientist-level agent with a verifiable objective

Our system is a **Scientist-level agent with a verifiable objective**. It gets a task, a dataset and an automatic evaluator. Humans only set the task and rules and choose the final submissions. This is narrower than Kosmos or The AI Scientist-v2, on purpose: with an external score we can measure if a change in the research machinery changed the outcome.

## 3. What we add: overview

| # | Part | Is it our contribution? | Short idea |
|---|---|---|---|
| 1 | **Path to the root** (also called local knowledge) | **Yes - main one** | Each agent keeps the chain of decisions from its current idea back to the root. Each step stores the decision, the evidence, and its **undo condition**. Context grows with depth, not with time. |
| 2 | **Trajectory** as a control signal | Yes | All paths together form an append-only trajectory (plan, implement, result, revision). We use it as a *control signal*: stuck branches, two agents converging, distance from tried ideas. |
| 3 | **Failure records** and refinement | Yes, but weak for now | When an idea fails, record why and what must change, so the idea can be refined instead of dropped. We are not happy with the current rule. |
| 4 | Shared knowledge | No (Kosmos-style already exists) | A summary of important results and of approaches that worked or failed, given to every new idea. |
| 5 | Explicit research steps | Not decided | 3 of 4 closest works define research steps (and literature review). We do not have it yet, so we do not claim it. |

## 4. Idea 1 - Path to the root (main contribution)

- Each agent carries the chain of actions and results from the idea it works on now, back to the root of its reasoning.
- Each step on the path holds three things:
  1. the **decision** taken,
  2. the **evidence** it was based on,
  3. the **undo condition**: the evidence that would show the decision was wrong.
- Why it helps:
  - **Long context:** the agent loads only its path, not the whole history. Context grows with *depth*, not with *time*.
  - **Fixation:** if the undo condition of a past decision is now true, the system flags it in every cycle. The agent does not need to notice it alone.
- How it differs:
  - AI Scientist-v2 tree nodes keep code and scores, but **not the reasons** for choosing them.
  - The Kosmos world model keeps **conclusions, not paths**.
  - Arbor gives the **whole tree** to the decision step.

### Worked example (simplified from our September Kaggle run)

> [!example] One path with three steps
> Task: predict labels from spectra. The agent is now working on step C. Its path to the root:
>
> | Step | Decision | Evidence | Undo condition |
> |---|---|---|---|
> | A | Use a neural network on the raw spectra | it beat the simple baseline on local validation | a simpler model matches it on the held-out score |
> | B | Trust the local validation split | it agreed with the first leaderboard submission | the local score goes up while the leaderboard score goes down |
> | C (now) | Use the fact that test spectra are copies of training rows with wrong labels | +0.028 on the leaderboard (a real Phase 1 finding) | the gain disappears on a fresh held-out split |
>
> **What happens next.** Suppose the local score keeps rising but the leaderboard falls. The undo condition of step B is now true, so the system flags step B in every cycle until the agent revisits it. The agent loads only A, B and C, not every node of the run, so its context grows with depth, not with time.
>
> Without the path, the agent keeps trusting a misleading validation, which is exactly what we saw in our runs (section 11).
>
> *Steps A and B are simplified to show the idea; they are not an exact log.*

## 5. Idea 2 - Trajectory as a control signal

- The local paths of all agents form an **append-only trajectory** of the run: plan, implementation, result, revision.
- Baselines log steps only for audit (for example Kosmos logs citations). We use the trajectory as a **control signal**. It tells the loop:
  - when an agent spent several cycles on one branch with no gain,
  - when two agents are converging on the same idea,
  - how far current candidates are from everything already tried.
- This distance is what the **exploration budget** of each cycle is spent against.

## 6. Idea 3 - Failure records (preliminary)

- When an idea fails, the path records **why** and **what would need to change**.
- So the idea can be refined, not thrown away.
- The current rule is preliminary. We expect to change it the most.

## 7. Shared memory (we use it, but do not claim it)

- A summary of important results, and of approaches that worked or failed.
- Kosmos-style summaries already do this, so it is not our contribution.
- In Phase 1 this was the **most useful feature**: "shared memory of findings and failures, given to every new idea".

## 8. Exploration vs refinement

Creativity is a search problem: make many variations and keep the good ones. The budget must be split between **new ideas** and **refining old ideas**. LLM agents under-explore and defend ideas they already hold (confirmation bias, design fixation, escalation of commitment). Self-reflection makes this worse.

Ideas we have:
- Make exploration **part of the loop**, not an optional tool (optional tools were rarely used in our pilot).
- **Short-lived ambitious sub-agents** that ignore the parent hierarchy and try very novel ideas, so agents do not only defend old ideas.
- **Combine "jumping ideas" with refinement** and decide how much of each is needed ([[AR 7 - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]]).
- An agent that knows **when to move**: down, up, sideways, merge branches, or return to an old branch ([[AR 7 - Meeting Log#2026-08-25 One-on-One|2026-08-25 One-on-One]]).
- **Quality-diversity** as the main rule of the search (see [[03 Ideas Inbox]]).

## 9. Base system (Arbor) and the versions we built

- [[Arbor - Hypothesis Tree Refinement|Arbor]] keeps the whole research state in a **hypothesis tree**. A long-lived **coordinator** owns the tree. Short-lived **executors** test one hypothesis each in an isolated git worktree.
- Loop: Observe, Ideate, Select, Dispatch, Backpropagate, Decide.
- **Arbor default (as we ran it):** only step planning and coding; each round has 12 cycles; each cycle adds a node; a winning cycle is merged into the trunk; the tree is deleted after each round.
- **What we think is missing:** Arbor gives the whole graph to the decision step. This makes it hard to go deep on one idea. We give only the parent history plus compressed shared knowledge.

### Versions we built

- **Phase 1 (Sep 2026):** revoke ideas, merge duplicate ideas, leaf idea with parent history, better concurrency (many separate agents), full history, long-term memory, start a node from any point in the parent history, stop when no active ideas are left.
- **Phase 2 (end of Sep 2026):** loop of plan, implement, plan, implement.
- **Baselines run:** Arbor default and Claude Code.

## 10. Mechanisms tried in Phase 1 and what happened

| Mechanism | What happened in the Sep run |
|---|---|
| Shared memory of findings and failures | Most useful |
| Blocking an idea until its undo condition is met | Partly useful (saved cycles once an honest test existed). In October logs: blocking made agents **less ambitious** |
| Evidence verdicts | Partly useful (clear record that matched the next choices) |
| Reactivation of ideas | Never used |
| Duplicate merging | Never used |
| Concurrent ideas, parallel workers | Never used (each round had one agent and a fresh tree) |
| Confidence-based selection | Never used |
| Automatic evidence router | No effect (0 candidate leaves in 10 tries) |
| Stop when no ideas left | Worked (stopped after 9 of 12 cycles) |

Lesson: **carry the Phase 1 tree and memory across rounds and add a leaf-selection tool**, so the unused parts can be tested.

## 11. Mistakes we saw agents make (motivation for the paper)

- An idea blocked with an explicit undo (reopening) condition **stayed blocked** after the condition was met.
- Agents kept trusting a **local validation** that had already misled them.
- When a merge tool refused a change, agents **bypassed it** instead of revising the idea.
- With web search, agents seemed to **copy public solutions** more than create new ideas.

## Related

- [[AR 4 - Related Work and Baselines]]
- [[Memory and Long Context]]
- [[Creativity and Exploration]]
- [[Reversible Exploration (ARIADNE and Retro-Fork)]] - earlier version of the rollback and failure ideas
