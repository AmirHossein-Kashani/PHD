---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/autonomous-research-agents]
---
# AR 4 - Related Work and Baselines

Part of [[AR 0 - Start Here|Auto Researcher]]. Sources: [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]], [[AR 7 - Meeting Log#2026-08-25 One-on-One|2026-08-25 One-on-One]], [[AR 7 - Meeting Log#2026-09-01 One-on-One|2026-09-01 One-on-One]], [[AR 7 - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[AR 6 - Paper Draft (Introduction)]].

> [!summary] In short
> The newest and closest work is **long-horizon agents with memory** (ML-Master 2.0, ScienceFlow, AiScientist, MLEvolve). They already keep reasons, checkpoints or failure logs. What still looks new in our work: the **undo condition** stored with each decision, memory organised **by path** (not by time or by similarity), and the trajectory used as a **control signal**. The older framing in our paper introduction (AI Scientist-v2, AI-Researcher and Kosmos compress history and lose the reasoning path) is still true, but no longer enough.

> [!tip] Where the system notes are
> This note is the overview of the folder **AR 4 - Related Work**. Each system has its own note in the subfolders of the same folder: 4.0 Base system, 4.1 Baselines we can run, 4.2 Closest to our ideas, 4.3 Other research-agent systems, 4.4 Search studies. Benchmarks stay in `3 Papers` (see [[AR 3 - Benchmarks]]).

---

## 1. Closest work for each of our ideas

| Our idea | Closest work | What they already do | What we add |
|---|---|---|---|
| **Path to the root** with undo conditions | [[ScienceFlow - Re-Anchoring Long-Horizon Agent\|ScienceFlow]], [[ML-Master 2.0 - Hierarchical Cognitive Caching\|ML-Master 2.0]], [[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]] | jump back to archived checkpoints with their evidence; phase summaries that keep reasons; memory scoped to ancestors | memory organised by the path of the current idea; each decision carries an undo condition that says *when* to reopen it |
| **Trajectory** as a control signal | [[AiScientist - File-as-Bus Long-Horizon Engineering\|AiScientist]], [[FML-bench - Search Dynamics of Research Agents\|FML-bench]] (AdaptiveSearch), [[MLEvolve - Monte Carlo Graph Search\|MLEvolve]] | append-only logs; "greedy until stuck, then branch"; exploration that decays, plus expansion when stuck | read the whole trajectory to detect stuck branches, converging agents and novelty, and spend the exploration budget against it |
| **Failure records** and refinement | [[Failure Registry - Recovering Wasted Compute\|Failure registry]], [[MLEvolve - Monte Carlo Graph Search\|MLEvolve]], [[DeepScientist - Findings Memory\|DeepScientist]], [[Reflexion]] | banned patterns from crashes plus backtracking; retrieval of failed nodes; a findings memory that includes failures; verbal self-reflection | record *why the idea* failed and *what must change*, so it can be refined or reopened, not only avoided |
| Shared memory (we do not claim it) | [[Kosmos - AI Scientist with World Model\|Kosmos]] | a shared world model of findings | - |

## 2. Our base system

- [[Arbor - Hypothesis Tree Refinement|Arbor]] - persistent hypothesis tree, coordinator plus executors, lessons propagated up the tree, failed attempts become constraints.
- Difference to AI Scientist-v2 (from [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]]): in AI Scientist-v2 a node is one code run; in Arbor a node is a *research unit* (hypothesis, code, evidence, insight). AI Scientist-v2 rebuilds the tree at each stage; Arbor keeps one tree for the whole run.

## 3. Which baselines to run

**Option A - from the list (creativity and hypothesis revision):** core = Arbor, AI Scientist-v2, ResearchAgent; add Robin and MOOSE-Chem for biomedical experiments. Good if the paper is judged on idea quality.

**Option B - Claude's view for our current setup** (task + automatic evaluator + hidden test score): ResearchAgent and MOOSE-Chem never run experiments, and Co-Scientist, Kosmos, ScientistTwo and AiScientist have no runnable code, so none of them can run on MLE-bench-style tasks. A runnable set:
1. **Arbor default** and **Claude Code** (already run)
2. **ML-Master 2.0** - memory rival
3. **AIRA-dojo** (greedy / MCTS) - clean tree search; its memory scopes act as an ablation of path memory
4. **autoresearch** - cheap greedy floor
5. optional: **MLEvolve** or **ScienceFlow**, if "reopening" is a headline claim

Everything else goes in related work. **Fair comparison:** same LLM, same time and compute, same tools and web access, at least 3 seeds. Arbor itself reports 86.36% on MLE-bench Lite with GPT-5.5, so compare only at the same model and budget.

## 4. Systems in our paper introduction

The introduction draft compares against these three systems.

| System | What it does | Weak point (our view) |
|---|---|---|
| **[[The AI Scientist (v1 and v2)\|AI Scientist-v2]]** (Yamada et al., 2025) | Agentic tree search run by an experiment manager. Four forced stages: preliminary investigation, hyperparameter tuning, research agenda, ablations. Each stage is a separate tree; the best node seeds the next stage. Ideas are made without thinking about implementation. | Alternatives from earlier stages are thrown away. Nodes keep code and scores, not reasons. Authors say it struggles with novel, high-impact ideas. |
| **[[AI-Researcher and Scientist-Bench\|AI-Researcher]]** (Tang et al., 2025) | Full pipeline from 15-20 input papers: literature review (atomic ideas, datasets, plan), 5 ideas but only the best is implemented, short runs, an "advisor" decides to continue, then writing. Introduces Scientist-Bench. | Idea refinement is thin. Benchmark has data contamination, LLM-only judging, and agents can find the original papers online. Best idea: start research from input papers. |
| **[[Kosmos - AI Scientist with World Model\|Kosmos]]** (Mitchener et al., 2025) | Up to about 200 agent rollouts over long runs. Shared structured world model (summary). Logs citations and steps. Main contribution: separation of context. | Keeps conclusions, not paths. Still limited by how long agents stay coherent. |

## 5. Main research-agent systems

> [!note] Source
> A list of recent research-agent baselines I collected on 27 Oct 2026, checked by Claude against arXiv, GitHub and journal pages, plus extra systems Claude found. Each system has its own note. Corrections to the original list: Co-Scientist and Robin are now in **Nature** (2026); autoresearch has about 92.7k stars (not 98k).

A fair comparison should separate three abilities: **generate** ideas, **test** them, and **revise** them from evidence. Many systems cover only one or two.

| System | Year, venue | Main idea | Generate | Test | Revise | Code | Role for us |
|---|---|---|---|---|---|---|---|
| [[Arbor - Hypothesis Tree Refinement\|Arbor]] | 2026, arXiv | Persistent hypothesis tree, coordinator + executors | yes | yes (auto score) | yes (prune, insights go up the tree) | open | our base; "Arbor default" baseline |
| [[The AI Scientist (v1 and v2)\|AI Scientist-v2]] | 2025; Nature 2026 | Staged best-first tree search, idea to paper | yes | yes | partly (best node per stage) | open (custom license) | best-known end-to-end name |
| [[AI-Researcher and Scientist-Bench\|AI-Researcher]] | NeurIPS 2025 | Paper-to-paper pipeline with an advisor agent | yes | yes | weak (continue / stop) | open | end-to-end related work |
| [[Google AI Co-Scientist\|Co-Scientist]] | Nature 2026 | Debate, Elo tournament, evolution of hypotheses | yes | no (humans run the lab) | yes | closed | related work |
| [[Kosmos - AI Scientist with World Model\|Kosmos]] | 2025, arXiv | Data + literature agents sharing a world model | yes | yes (data analysis) | partly | commercial | closest long-run system |
| [[Robin - Drug Repurposing Agent\|Robin]] | Nature 2026 | Literature agents, tournament, data-analysis agent | yes | humans run the lab | yes (rounds) | open, needs paid Edison API | biomedical baseline only |
| [[ResearchAgent - Iterative Idea Generation\|ResearchAgent]] | NAACL 2025 | Citation graph + entity co-occurrence store + 15 reviewer agents | yes | no | yes (critique) | open | ideation only |
| [[MOOSE-Chem - Chemistry Hypothesis Rediscovery\|MOOSE-Chem]] | ICLR 2025 | Inspiration retrieval, evolution, ranking | yes | no | partly | open | domain ideation only |
| [[Autoresearch (Karpathy)\|autoresearch]] | 2026, repo | Greedy edit, train, keep-or-reset loop | yes | yes | no | open | simple baseline |
| [[AutoResearchClaw]] | 2026, arXiv | 23 stages, PROCEED / REFINE / PIVOT | yes | yes | yes | open | optional end-to-end |
| [[ScientistTwo]] | 2026, arXiv | Improve a published method; Good / Bad / Engineer triage | yes | yes | yes | no code (about $3.8k per task) | related work |

## 6. Long-horizon agents with memory (2026)

These are closer to **our** contribution (memory, failure records, reopening) than most systems above, and most report on MLE-bench (see [[AR 3 - Benchmarks]]).

| System | Memory / search idea | Result | Code | Why it matters |
|---|---|---|---|---|
| [[ML-Master 2.0 - Hierarchical Cognitive Caching\|ML-Master 2.0]] | 3 memory levels; phase summaries keep reasons | 56.44% medals, full MLE-bench, 24 h | open | strongest memory rival; run it |
| [[MLEvolve - Monte Carlo Graph Search\|MLEvolve]] | UCT + cross-branch edges + success/failure memory | 65.3% any medal, full, 12 h | open | open state of the art; already stores failures |
| [[ScienceFlow - Re-Anchoring Long-Horizon Agent\|ScienceFlow]] | Jump back to archived checkpoints with their evidence | 70.22% any medal, full, 24 h | open | closest to "reopen old ideas" |
| [[AiScientist - File-as-Bus Long-Horizon Engineering\|AiScientist]] | Shared workspace, append-only logs | 81.82% any medal, Lite | no | closest to our trajectory idea |
| [[DeepScientist - Findings Memory\|DeepScientist]] | Bayesian optimization + Findings Memory | month-long runs (not MLE-bench) | open | canonical long run with failure memory |
| [[Failure Registry - Recovering Wasted Compute\|Failure registry]] | Banned patterns, proven fixes, backtracking | AIDE golds 22 to 38 (9 tasks) | no | direct prior art for failure record + backtracking |
| [[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]] (section 7) | Ancestor vs sibling memory scopes | 47.7% medals, Lite | open (non-commercial) | clean tree-search baseline |

**Also seen (no note yet):** [R&D-Agent](https://arxiv.org/abs/2505.14738) (Microsoft, 35.1% on MLE-bench), [ShinkaEvolve](https://arxiv.org/abs/2509.19349) (Sakana, program evolution), [InternAgent-1.5](https://arxiv.org/abs/2602.08990), [AIDE²](https://arxiv.org/abs/2609.26457) (Weco, conditional failure memory), [BeliefWeaver](https://arxiv.org/abs/2610.18082) (a belief state that survives rollback; close to undo conditions), [ReTree](https://arxiv.org/abs/2608.10676) (traces a contradicted claim back to where it entered, then prunes), [EurekAgent](https://arxiv.org/abs/2606.13662) (Claude Code with git as memory).

> [!warning] Name collisions
> - AMD's *Arbor: Tree Search as a Cognition Layer for Autonomous Agents* ([arXiv 2606.12563](https://arxiv.org/abs/2606.12563)) is **not** our base Arbor ([arXiv 2606.11926](https://arxiv.org/abs/2606.11926)).
> - *Coscientist* (Boiko et al., Nature 2023, chemistry lab agent) is **not** Google's AI Co-Scientist.

## 7. Studies of search and exploration

These two papers do not build a new research loop. They study **which search behavior works**, which is our open question on exploration vs refinement ([[AR 5 - Open Questions]]).

| Paper | Main finding | Why it matters for us |
|---|---|---|
| [[FML-bench - Search Dynamics of Research Agents\|FML-bench]] (2026) | Six agents with the same tools and budget on 18 ML tasks. Early gains predict the final score; diversity alone does not; scattered search hurts. Greedy wins on "dense" tasks, branching wins on "sparse" ones. **AdaptiveSearch** (greedy, then fork 1-3 different branches when the score stops rising) was best. | Closest published version of "jumping ideas + refinement". Must cite, and a likely baseline. Their branches keep the same context and have no path to the root, failure memory or graft back. |
| [[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]] (NeurIPS 2025) | Agent = search policy + operators. With weak operators, MCTS and evolution were no better than greedy; better operators raised every policy (medal rate 39.6% to 47.7% on MLE-bench Lite). Large val-test gap; picking final nodes by validation carefully closes most of it. | Operators may matter more than the exploration ratio. Their ancestor-scoped memory is close to our path to the root, but without reasons or overturn conditions. Supports our Kaggle validation problem. |

## 8. What this means for our novelty claim

- "Keeping the path and the reasons" is **no longer unique**: ML-Master 2.0 keeps reasons in phase summaries, ScienceFlow jumps back to checkpoints with their evidence, AiScientist keeps append-only logs and revisits invalidated assumptions, MLEvolve and the failure registry store failures.
- What still looks new: the **undo condition** stored with each decision (it says *when* to reopen), memory organised **by path, not by time or similarity**, and the trajectory used as a **control signal**. The paper should say this precisely and show it with ablations.

## 9. Earlier lists (August-September 2026)

> [!note] Kept for history
> These lists come from earlier meetings. Sections 1-8 above replace them.

### 9.1 "Most dangerous" related papers (August ranking)

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

### 9.2 Baselines by question (September list)

| Question | Best representative baseline |
|---|---|
| Quality and diversity of research ideas | IDEAgent |
| High-end biomedical hypothesis generation | Co-Scientist |
| Chemistry hypothesis rediscovery | MOOSE-Chem / MOOSE-Chem2 |
| Tree-search scientific creativity | MC-NEST |
| Drug repurposing agent | RepurAgent (very useful: directly drug repurposing) |
| End-to-end therapeutic discovery | Robin |
| General biomedical agent capability | [[Biomni - General Biomedical Agent\|Biomni]] |

### 9.3 Nearest neighbors for the quality-diversity idea (August)

- **Nova** - more novel and diverse research ideas through planning and search (3.4x more unique novel ideas). Not quality-diversity, not archive-based, not drug-specific.
- **Diverse molecular generation** - works at the molecule level, not the hypothesis level.
- **QADD** - multi-objective molecule design (diversity vs potency) as an RL generator.
- **Drug discovery agent systems** - MADD, Mozi, AgentD, CLADD. They run pipelines, but do not use "illumination of a hypothesis space" as the core mechanism.
- Gap written then: *quality-diversity as the governing principle of a long-horizon autonomous research agent, built on Arbor, aimed at a diverse portfolio of drug hypotheses.*

### 9.4 Other systems in the notes

- [[AlphaEvolve]] and FunSearch - evolutionary program search with automatic evaluation.
- [[Virtual Lab]] - PI, scientist and critic agents in meetings.
- Coscientist (check: Boiko et al. 2023 chemistry agent, or [[Google AI Co-Scientist]]?), [[Robin - Drug Repurposing Agent|Robin]], [[Kosmos - AI Scientist with World Model|Kosmos]] - end-to-end discovery.
- PharmAgents, MADD, DrugSAGE, ChatInvent, DrugAgent, DiscoVerse, Medea, Agentomics (11 of 20 SOTA), MolClaw - multi-agent drug discovery list in [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]].
- Practical baseline used in our runs: **Arbor default** and **Claude Code**.

## Related

- [[Autonomous Research Agents]]
- [[AR 3 - Benchmarks]]
