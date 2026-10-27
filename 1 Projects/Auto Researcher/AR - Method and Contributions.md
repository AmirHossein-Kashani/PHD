---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context, topic/creativity-and-exploration]
---
# AR - Method and Contributions

Part of [[Auto Researcher]]. Sources: [[AR Paper - Introduction Draft]], [[AR - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]], [[AR - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[AR - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]].

> [!summary] In short
> We keep the **same model and the same research loop** as the baselines. We only change the **research state** that the agent reasons over: a local path to the root, a trajectory of thinking, and failure records.

---

## 1. Local knowledge: the path to the root (main contribution)

- Each agent carries the chain of actions and results from the idea it works on now, back to the root of its reasoning.
- Each step on the path holds three things:
  1. the **decision** taken,
  2. the **evidence** it was based on,
  3. the **condition that would overturn it**.
- Why it helps:
  - **Long context:** the agent loads only its path, not the whole history. Context grows with *depth*, not with *time*.
  - **Fixation:** if the overturning condition of a past decision is now true, the system flags it in every cycle. The agent does not need to notice it alone.
- How it differs:
  - AI Scientist-v2 tree nodes keep code and scores, but **not the reasons** for choosing them.
  - The Kosmos world model keeps **conclusions, not paths**.
  - Arbor gives the **whole tree** to the decision step.

## 2. Shared knowledge (we use it, but do not claim it)

- A summary of important results, and of approaches that worked or failed.
- Kosmos-style summaries already do this, so it is not our contribution.
- In Phase 1 this was the **most useful feature**: "shared memory of findings and failures, given to every new idea".

## 3. Trajectory of thinking

- The local paths of all agents form an **append-only trajectory** of the run: plan, implementation, result, revision.
- Baselines log steps only for audit (for example Kosmos logs citations). We use the trajectory as a **control signal**. It tells the loop:
  - when an agent spent several cycles on one branch with no gain,
  - when two agents are converging on the same idea,
  - how far current candidates are from everything already tried.
- This distance is what the **exploration budget** of each cycle is spent against.

## 4. Failure refinement (preliminary)

- When an idea fails, the path records **why** and **what would need to change**.
- So the idea can be refined, not thrown away.
- The current rule is preliminary. We expect to change it the most.

## 5. Exploration vs exploitation

Creativity is a search problem: make many variations and keep the good ones. The budget must be split between **new ideas** and **refining old ideas**. LLM agents under-explore and defend ideas they already hold (confirmation bias, design fixation, escalation of commitment). Self-reflection makes this worse.

Ideas we have:
- Make exploration **part of the loop**, not an optional tool (optional tools were rarely used in our pilot).
- **Short-lived ambitious sub-agents** that ignore the parent hierarchy and try very novel ideas, so agents do not only defend old ideas.
- **Combine "jumping ideas" with refinement** and decide how much of each is needed ([[AR - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]]).
- An agent that knows **when to move**: down, up, sideways, merge branches, or return to an old branch ([[AR - Meeting Log#2026-08-25 One-on-One|2026-08-25 One-on-One]]).
- **Quality-diversity** as the main rule of the search (see [[03 Ideas Inbox]]).

## 6. Mechanisms tried in Phase 1 and what happened

| Mechanism | What happened in the Sep run |
|---|---|
| Shared memory of findings and failures | Most useful |
| Blocking an idea with a reopening condition | Partly useful (saved cycles once an honest test existed). In October logs: blocking made agents **less ambitious** |
| Evidence verdicts | Partly useful (clear record that matched the next choices) |
| Reactivation of ideas | Never used |
| Duplicate merging | Never used |
| Concurrent ideas, parallel workers | Never used (each round had one agent and a fresh tree) |
| Confidence-based selection | Never used |
| Automatic evidence router | No effect (0 candidate leaves in 10 tries) |
| Stop when no ideas left | Worked (stopped after 9 of 12 cycles) |

Lesson: **carry the Phase 1 tree and memory across rounds and add a leaf-selection tool**, so the unused parts can be tested.

## 7. Things we saw agents do wrong (motivation for the paper)

- An idea blocked with an explicit reopening condition **stayed blocked** after the condition was met.
- Agents kept trusting a **local validation** that had already misled them.
- When a merge tool refused a change, agents **bypassed it** instead of revising the idea.
- With web search, agents seemed to **copy public solutions** more than create new ideas.

## Related

- [[AR - Related Systems and Baselines]]
- [[Memory and Long Context]]
- [[Creativity and Exploration]]
- [[Reversible Exploration (ARIADNE and Retro-Fork)]] - earlier version of the rollback and failure ideas
