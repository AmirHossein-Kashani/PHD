---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/autonomous-research-agents]
---
# AR - Related Systems and Baselines

Part of [[Auto Researcher]]. Sources: [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]], [[AR - Meeting Log#2026-08-25 One-on-One|2026-08-25 One-on-One]], [[AR - Meeting Log#2026-09-01 One-on-One|2026-09-01 One-on-One]], [[AR - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[AR Paper - Introduction Draft]].

> [!summary] In short
> The three closest systems are **AI Scientist-v2, AI-Researcher and Kosmos**. All of them compress history (a best node, an advisor verdict, or a world-model summary) and generate candidate ideas once, early. None of them keeps the agent's own **reasoning path** as a first-class object. That is our gap.

---

## 1. The three closest systems (for the paper)

| System | What it does | Weak point (our view) |
|---|---|---|
| **AI Scientist-v2** (Yamada et al., 2025) | Agentic tree search run by an experiment manager. Four forced stages: preliminary investigation, hyperparameter tuning, research agenda, ablations. Each stage is a separate tree; the best node seeds the next stage. Ideas are made without thinking about implementation. | Alternatives from earlier stages are thrown away. Nodes keep code and scores, not reasons. Authors say it struggles with novel, high-impact ideas. |
| **AI-Researcher** (Tang et al., 2025) | Full pipeline from 15-20 input papers: literature review (atomic ideas, datasets, plan), 5 ideas but only the best is implemented, short runs, an "advisor" decides to continue, then writing. Introduces Scientist-Bench. | Idea refinement is thin. Benchmark has data contamination, LLM-only judging, and agents can find the original papers online. Best idea: start research from input papers. |
| **Kosmos** (Mitchener et al., 2025) | Up to about 200 agent rollouts over long runs. Shared structured world model (summary). Logs citations and steps. Main contribution: separation of context. | Keeps conclusions, not paths. Still limited by how long agents stay coherent. |

## 2. Our base system

- [[Arbor - Hypothesis Tree Refinement|Arbor]] - persistent hypothesis tree, coordinator plus executors, lessons propagated up the tree, failed attempts become constraints.
- Difference to AI Scientist-v2 (from [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]]): in AI Scientist-v2 a node is one code run; in Arbor a node is a *research unit* (hypothesis, code, evidence, insight). AI Scientist-v2 rebuilds the tree at each stage; Arbor keeps one tree for the whole run.

## 3. "Most dangerous" related papers (August ranking)

These were the papers to read before more implementation ([[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]]):

| Priority | Paper | Why it matters |
|---|---|---|
| 1 | Arbor / Hypothesis-Tree Refinement | Closest dynamic autonomous-research search system |
| 2 | [[Mage - Memory as Execution State\|Mage]] | Closest to gates, rollback and failed-branch hints |
| 3 | GCC (Git Context Controller) | Versioned branch memory, like git branch and merge |
| 4 | [[EviGraph - Evidence-Guided Research Agents\|EviGraph]] | Research graph, evidence repair, checkpoints, rollback |
| 5 | FiF: Failure is Feedback | Backtracking that learns from failure (retrieval tasks only) |
| 6 | DynaDebate | Dynamic paths, disagreement-triggered verification, early stop |
| 7 | MemTX | Makes "transactional shared memory" hard to claim |
| 8 | MRAgent | Makes on-demand KG/history retrieval hard to claim |
| 9 | MemSIF | Makes "core memory + on-demand memory" hard to claim |
| 10 | ChronoMem | Semantic rollback of memory |
| 11 | AgentGit | Multi-agent branching, revert, versioning |
| 12 | [[AIDE - ML Engineering as Code Search\|AIDE]] / AI Scientist-v2 | Search baselines for code and research |
| 13 | [[AlphaEvolve]] family (FunSearch) | Evolutionary search baseline |

Feature comparison from the same meeting (the last column was our planned contribution):

| System | Branch | Multi-agent | Failure memory | Evidence graph | Rollback | Decision-level rollback from valid failure |
|---|---|---|---|---|---|---|
| AIDE | yes | no | limited | no | tree revisit | no |
| LATS | yes | no | reflection | no | yes | no |
| AI Scientist-v2 | yes | limited | limited | no | search | no |
| AutoScientists | yes | yes | yes | shared state | adaptive | no / weak |
| Negative Knowledge | no | no | yes | structured records | no | no |
| ChronoMem | no | no | memory versions | no | yes | no |
| EviGraph | yes | components | some | yes | yes | not its central problem |
| **ARIADNE-Bio (ours, August plan)** | yes | yes | yes | yes | yes | **planned contribution** |

Other notes from that meeting:
- **Shepherd** paper: works like git versioning for ideas (rollback, branching, merges). It resets knowledge when restarting or forking. Merge only means using child findings in the parent stream.
- **Negative Knowledge:** stores failures in permanent storage for sequential thinking. Very close.
- **ChronoMem:** rollback that removes data.

## 4. Baselines by question (September list)

| Question | Best representative baseline |
|---|---|
| Quality and diversity of research ideas | IDEAgent |
| High-end biomedical hypothesis generation | Co-Scientist |
| Chemistry hypothesis rediscovery | MOOSE-Chem / MOOSE-Chem2 |
| Tree-search scientific creativity | MC-NEST |
| Drug repurposing agent | RepurAgent (very useful: directly drug repurposing) |
| End-to-end therapeutic discovery | Robin |
| General biomedical agent capability | [[Biomni - General Biomedical Agent\|Biomni]] |

## 5. Nearest neighbors for the quality-diversity idea (August)

- **Nova** - more novel and diverse research ideas through planning and search (3.4x more unique novel ideas). Not quality-diversity, not archive-based, not drug-specific.
- **Diverse molecular generation** - works at the molecule level, not the hypothesis level.
- **QADD** - multi-objective molecule design (diversity vs potency) as an RL generator.
- **Drug discovery agent systems** - MADD, Mozi, AgentD, CLADD. They run pipelines, but do not use "illumination of a hypothesis space" as the core mechanism.
- Gap written then: *quality-diversity as the governing principle of a long-horizon autonomous research agent, built on Arbor, aimed at a diverse portfolio of drug hypotheses.*

## 6. Other systems in the notes

- [[AlphaEvolve]] and FunSearch - evolutionary program search with automatic evaluation.
- [[Virtual Lab]] - PI, scientist and critic agents in meetings.
- Coscientist, Robin, Kosmos - end-to-end discovery.
- PharmAgents, MADD, DrugSAGE, ChatInvent, DrugAgent, DiscoVerse, Medea, Agentomics (11 of 20 SOTA), MolClaw - multi-agent drug discovery list in [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]].
- Practical baseline used in our runs: **Arbor default** and **Claude Code**.

## 7. Studies of search and exploration in research agents (added 27 Oct 2026)

These two papers do not build a new research loop. They study **which search behavior works**, which is our open question on exploration vs refinement ([[AR - Open Questions]]).

| Paper | Main finding | Why it matters for us |
|---|---|---|
| [[FML-bench - Search Dynamics of Research Agents\|FML-bench]] (2026) | Six agents with the same tools and budget on 18 ML tasks. Early gains predict the final score; diversity alone does not; scattered search hurts. Greedy wins on "dense" tasks, branching wins on "sparse" ones. **AdaptiveSearch** (greedy, then fork 1-3 different branches when the score stops rising) was best. | Closest published version of "jumping ideas + refinement". Must cite, and a likely baseline. Their branches keep the same context and have no path to the root, failure memory or graft back. |
| [[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]] (NeurIPS 2025) | Agent = search policy + operators. With weak operators, MCTS and evolution were no better than greedy; better operators raised every policy (medal rate 39.6% to 47.7% on MLE-bench Lite). Large val-test gap; picking final nodes by validation carefully closes most of it. | Operators may matter more than the exploration ratio. Their ancestor-scoped memory is close to our path to the root, but without reasons or overturn conditions. Supports our Kaggle validation problem. |

## Related

- [[Autonomous Research Agents]]
- [[AR - Benchmarks]]
