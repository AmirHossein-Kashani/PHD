---
type: project-hub
status: active
started: 2026-08
tags: [type/hub, project/auto-researcher]
---
# Auto Researcher

> [!summary] In short
> An autonomous research agent that runs the full research loop by itself for days: it makes ideas, writes code, tests the ideas with an automatic evaluator, keeps the good ones and drops the bad ones. We build it on top of [[Arbor - Hypothesis Tree Refinement|Arbor]]. Our change is **what the agent remembers and reasons over**: the path from its current idea back to the root, a trajectory of its thinking, and a record of why ideas failed.

**Status (23 Oct 2026):** writing the paper. The introduction is drafted ([[AR Paper - Introduction Draft]]) and is being made clearer. First experiments on a Kaggle task are done. Baselines and better evaluation tasks are still open.

**Main notes of this project**
- [[AR - Method and Contributions]] - what we change and why
- [[AR - Experiments and Results]] - what we ran and what we saw
- [[AR - Related Systems and Baselines]] - the closest systems and how we differ
- [[AR - Benchmarks]] - tasks we can test on
- [[AR - Open Questions]] - things to decide next
- [[AR Paper - Introduction Draft]] - the paper text so far
- [[AR - Meeting Log]] - all meeting notes about this project, by date

---

## 1. The problem in simple words

A research run is a very long chain of steps. This causes four problems for today's research agents:

1. **Long horizon means long context.** The agent must remember many steps. Models get worse when the context gets long. In our pilot, about **77% of all model usage was agents re-reading their own context**.
2. **Long-term planning is weak.** LLMs are not good planners. In our pilot, the version with optional planning stopped early with budget left. The version with forced plan reviews was the only one that revised blocked ideas.
3. **Memory loses the path.** Agents compress history (a best node, a summary). They keep results but lose *why* they are where they are, what they promised, and what evidence would make them go back.
4. **Agents do not explore enough.** They defend their own ideas (fixation). When exploration tools are optional, they are rarely used.

## 2. Our level

Our system is a **Scientist-level agent with a verifiable objective**. It gets a task, a dataset and an automatic evaluator. Humans only set the task and rules and choose the final submissions. This is narrower than Kosmos or The AI Scientist-v2, on purpose: with an external score we can measure if a change in the research machinery changed the outcome.

## 3. Our contributions (current version)

| # | Part | Is it our contribution? | Short idea |
|---|---|---|---|
| 1 | **Local knowledge (path to the root)** | **Yes - main one** | Each agent keeps the chain of decisions from its current idea back to the root. Each step stores the decision, the evidence, and the condition that would undo it. Context grows with depth, not with time. |
| 2 | **Trajectory of thinking** | Yes | All paths together form an append-only trajectory (plan, implement, result, revision). We use it as a *control signal*: stuck branches, two agents converging, distance from tried ideas. |
| 3 | **Failure refinement** | Yes, but weak for now | When an idea fails, record why and what must change, so the idea can be refined instead of dropped. We are not happy with the current rule. |
| 4 | Shared knowledge | No (Kosmos-style already exists) | A summary of important results and of approaches that worked or failed, given to every new idea. |
| 5 | Explicit research steps | Not decided | 3 of 4 closest works define research steps (and literature review). We do not have it yet, so we do not claim it. |

More detail: [[AR - Method and Contributions]].

## 4. Base system: Arbor

- [[Arbor - Hypothesis Tree Refinement|Arbor]] keeps the whole research state in a **hypothesis tree**. A long-lived **coordinator** owns the tree. Short-lived **executors** test one hypothesis each in an isolated git worktree.
- Loop: Observe, Ideate, Select, Dispatch, Backpropagate, Decide.
- **Arbor default (as we ran it):** only step planning and coding; each round has 12 cycles; each cycle adds a node; a winning cycle is merged into the trunk; the tree is deleted after each round.
- **What we think is missing:** Arbor gives the whole graph to the decision step. This makes it hard to go deep on one idea. We give only the parent history plus compressed shared knowledge.

## 5. Versions we built

- **Phase 1 (Sep 2026):** revoke ideas, merge duplicate ideas, leaf idea with parent history, better concurrency (many separate agents), full history, long-term memory, start a node from any point in the parent history, stop when no active ideas are left.
- **Phase 2 (end of Sep 2026):** loop of plan, implement, plan, implement.
- **Baselines run:** Arbor default and Claude Code.

## 6. Results so far (short)

- **Phase 1 vs Arbor default** ([[AR - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]]): same final score (0.322 vs 0.325), but Phase 1 reached a good score much faster, used about half the hours, merged 62% of cycles (vs 23%), and went 3 to 5 levels deep (vs 2). Shared memory of findings and failures was the most useful feature.
- **Four scaffolds on a Kaggle task** ([[AR - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]]): our approach was best in the first 4 cycles (rank 17). At the end Claude Code won by about +0.03. Our blocking rules made agents less ambitious.
- Full details: [[AR - Experiments and Results]].

## 7. How we got here

```mermaid
flowchart LR
  A[May: Is AI creative?<br/>agent architectures] --> B[June: multi-agent debate<br/>and LLM judges]
  B --> C[July: claim-level conflicts<br/>Gaps protocol]
  C --> D[27 July: stay close to<br/>MAD and MoA]
  D --> E[Aug: research agents<br/>ARIADNE, Retro-Fork]
  E --> F[Aug: creativity experiments<br/>in debate]
  F --> G[25 Aug: quality-diversity<br/>on Arbor idea]
  G --> H[Sep: Phase 1 and 2<br/>on Arbor]
  H --> I[Oct: paper writing]
```

- [[Early Exploration (May-June 2026)]]
- [[Claim-Level Conflict in Debate]]
- [[Reversible Exploration (ARIADNE and Retro-Fork)]]
- [[Creativity in Multi-Agent Debate]]

## 8. Next steps

See [[01 Now and Next#Auto Researcher]].

## 9. All notes linked to this project

### Drafts (1)

- [[AR Paper - Introduction Draft]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Papers (21)

- [[AIDE - ML Engineering as Code Search]] - AIDE searches over whole Python scripts with a solution tree. No shared knowledge, no merging. Close to our idea and a search baseline.
- [[AIRA-dojo - Search Policies and Operators]] - Meta's study on MLE-bench: an agent = search policy + operators. Better operators mattered more than greedy vs MCTS vs evolution. Agents overfit to validation.
- [[ALE-Bench]] - Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.
- [[AlphaEvolve]] - Evolutionary coding agent: LLMs propose program changes, automatic evaluation selects survivors. Creativity comes from a pool of ideas that keeps improving.
- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[BioDiscoveryAgent]] - Stanford agent that designs gene perturbation experiments. Main metric: cumulative hit ratio over rounds.
- [[BioML-bench]] - 24 end-to-end biomedical ML tasks (drug discovery, imaging, protein engineering, single-cell). Tests full ML pipelines by agents.
- [[BioML-bench - Drug Discovery Tasks]] - The 9 drug discovery tasks of BioML-bench (potency, ADME, toxicity) from Polaris and TDC.
- [[Biomni - General Biomedical Agent]] - Biomni builds a large biomedical action space (tools, databases, software) from 2,500 papers. Good for medical baselines.
- [[EscapeBench]] - Text-only room-escape games to measure creative intelligence of LLM agents. Top models reach only about 15% progress without hints.
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[FML-bench - Search Dynamics of Research Agents]] - 18 ML research tasks to compare the search behavior of six agents. Early gains predict the final score; scattered search does not help. AdaptiveSearch (greedy, then branch when stuck) is close to our exploration idea.
- [[GENEDISCO]] - Benchmark for choosing which genes to test next in costly experiments (active learning for drug target discovery).
- [[GENEDISCO - Creativity View]] - GENEDISCO seen as a creativity test: maximize target discovery rate and model performance.
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[MUTATE - Divergent Thinking Benchmark]] - Text sandboxes where the obvious solution fails. Measures path-level and action-level divergence of agents. Close to EscapeBench.
- [[PMO - Practical Molecular Optimization]] - Benchmark with 23 molecule objectives and a 10K oracle budget; scores the path too (AUC top-10). Idea: use intermediate rewards in long horizons.
- [[RE-Bench]] - Seven open-ended ML research engineering environments with a scoring function, compared with human experts.
- [[Reflexion]] - Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.
- [[SMDD-Bench]] - 502 medicinal-chemistry tasks over 102 protein targets (pharmacophore, interaction points, scaffold hopping, ...).
- [[Tree of Thoughts and Graph of Thoughts]] - Reasoning as a tree or a graph of thoughts with generate, refine and aggregate steps.

### Literature reviews and lists (1)

- [[Review - Creativity Measuring Benchmarks]] - Overview of benchmarks that measure creativity (CreativityBench, MacGyver, DeepMath-Creative, EscapeBench, MUTATE, bio tasks).

