---
type: project-hub
status: merged into Auto Researcher
period: 2026-07 to 2026-09
aliases: [ARIADNE, ARIADNE-Bio, Retro-Fork]
tags: [type/hub, project/reversible-exploration, topic/autonomous-research-agents, topic/memory-and-long-context]
---
# Reversible Exploration (ARIADNE and Retro-Fork)

> [!summary] In short
> The step between debate work and the [[Auto Researcher]] (late July to September 2026). After deciding to stay close to **MAD and MoA** (27 July), the plan became a multi-agent research system for biology (**ARIADNE-Bio**) whose key idea was **reversible exploration**: an agent can go back to an earlier decision when a *valid* failure shows that the decision was wrong (**decision-level rollback from a valid failure**). Later versions were called **Retro-Fork**. These ideas became Phase 1 of our Arbor-based system (revoke ideas, blocking with reopening conditions, parent history, shared failure memory).

Status: **merged** into [[Auto Researcher]].

---

## 1. Timeline

- **27 Jul** - decision: stay close to MAD and MoA. Files: `proposal_ariadne_2pager.docx`, `moa_baselines.docx`, `ariadne_benchmark_options (3).docx`, `five_benchmarks.docx` ([[Reversible Exploration (ARIADNE and Retro-Fork)#2026-07-27 One-on-One|2026-07-27 One-on-One]]).
- **31 Jul - 6 Aug** - benchmark study: [[GENEDISCO]], [[RE-Bench]], [[ALE-Bench]], [[BioML-bench]], [[BixBench]], [[Biomni - General Biomedical Agent|Biomni]], [[AIDE - ML Engineering as Code Search|AIDE]].
- **4 Aug** - big related-work review: Arbor, Mage, GCC, EviGraph, FiF, DynaDebate, ChronoMem, AI Scientist-v2, Shepherd. Gap table with "decision-level rollback from valid failure" ([[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|2026-08-04 One-on-One]]).
- **11 Aug** - `Reversible_Exploration_Plain_Language_v2.docx` and `Retro-Fork_Proposal_v3.docx` ([[Reversible Exploration (Plain Language v2)]], [[Retro-Fork Proposal v3]]).
- **12 Aug** - read [[Arbor - Hypothesis Tree Refinement|Arbor]], which became the base system.
- **Sep** - Phase 1 on Arbor ([[AR - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]]).

## 2. Key ideas

- **Rollback at the decision level**, not only at the code or memory level.
- **Failure is information:** keep failed branches and why they failed (negative knowledge).
- **Blocking with reopening conditions:** an idea is paused with a clear condition that would reopen it.
- **Branch, merge and revert ideas like git** (compare GCC, AgentGit, Shepherd).
- **Evidence graph** to justify each step (compare EviGraph).

## 3. Closest work and how it differs

See the full tables in [[AR - Related Systems and Baselines#3. "Most dangerous" related papers (August ranking)]].

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-04 One-on-One

> [!summary] This meeting in short
> Key meeting for the research-agent direction. Picked benchmarks (Biomni, BioML, AIDE). Compared AI Scientist-v2 and Arbor. Ranked the "most dangerous" related papers. Claimed gap: decision-level rollback from a valid failure (ARIADNE-Bio).

*Original: [Notion page](https://app.notion.com/p/3b3d68da32f08075814ed771e3e9af2b)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-08-04 One-on-One|MS - Meeting Log]]

> [!attachment] File in Notion: **weekly_research_update (1).html** - [open the Notion page](https://app.notion.com/p/3b3d68da32f08075814ed771e3e9af2b)

#### Meeting
- [ ] prepare and select exact benchmark from the two selected papers
    - [[Biomni - General Biomedical Agent]]  (good for medical baselines)
    - [[BioML-bench]] 
        - [[BioML-bench - Drug Discovery Tasks]] 
    - [[BixBench]] 
        - [[BixBench - Tasks and Baselines]] 
        - As it uses only specific environment i think it is better to ignore
    - [[AIDE - ML Engineering as Code Search]] (close to our idea)
- [x] report your final answer to compare

- [ ] Bad failure 
- Evi Graph [EviGraph: Evidence-Guided Autonomous Research Agents](https://arxiv.org/html/2608.04738v2)
    - [[EviGraph - Evidence-Guided Research Agents]] 
- ChronoMem 
    - They introduced rollback and they will remove the the data 
- Ngative Knowledge
    - very close , for sequential thinking , it will store the failures in permanent data storage
- AI Scientist-v2
    - approximately old version of arbor
        **2. What a node means: code attempt vs. hypothesis**
        In AI Scientist-v2, a node is basically *one experiment run*: a Python script, its execution result, plots, and a buggy/non-buggy label. The tree is a search over code implementations.
        In Arbor, each node is a *semantic research unit* — it binds a hypothesis, the artifact version that realizes it, the experimental evidence it produces, and the distilled insight that should shape later decisions. Nodes near the root are broad research directions; deeper nodes are concrete testable interventions. The tree operates one level of abstraction above code. [Hugging Face](https://huggingface.co/papers/2606.11926)
        **3. The tree's lifetime: transient vs. persistent memory**
        This is arguably the biggest conceptual difference. In AI Scientist-v2, the tree is rebuilt fresh at each of the four stages (preliminary → tuning → agenda → ablations); only the single best node survives to seed the next stage. Everything else — including *why* failed branches failed — is effectively thrown away.
        Arbor's tree is a persistent, cumulative research state for the whole run. When an experiment finishes, its lessons get **backpropagated upward**: the insight is written to the leaf, then abstracted into the parent direction's understanding, all the way to the root. Failed attempts become negative constraints on future ideation instead of dead ends. As the paper puts it, this turns autonomous research from a sequence of local attempts into a cumulative process in which strategy, execution, and evidence are carried across time. Their ablations found the tree is only useful *because* evidence accumulates over it. [Ruc-nlpir](https://ruc-nlpir.github.io/Arbor/)
        **4. Architecture: manager + workers vs. coordinator + isolated executors**
        AI Scientist-v2 has an experiment manager that walks a fixed four-stage pipeline, expanding nodes in parallel within each stage. Arbor separates roles more sharply: the coordinator manages global research strategy over the tree, while executors implement and test individual hypotheses in isolated worktrees (git worktrees, so exploratory code changes never touch the main artifact). The coordinator runs a repeating Observe → Ideate → Select → Dispatch → Backpropagate → Decide loop rather than fixed stages, and executors are strictly hypothesis-bound — they can fix their own code but can't quietly change what hypothesis they're testing.
- Arbor
    - the other paper [[Arbor - Hypothesis Tree Refinement]]  it is good to see
- Mage
    - [[Mage - Memory as Execution State]] 
- GCC (Git Context Controller)
    - only talks about the memory , completly look like the git branch and merge and merge only return to where seperated
- FiF : Failure is Feedback
    - used the concept of holding data when looping back in retrival task only, not agentic researcher
- **DynaDebate**
    - **Dynamic Path Generation and Allocation ⇒ force the model to create independent solution rather than blind replication of same idea and getting false majority of same thinking**
    - debate mechanism is not a new thing
    - early stop if converge 
- Data sets
    - ARC-Bench-ML andNanoResearch-20
#### The papers that are now most dangerous to you

These are the ones I would read before doing more implementation:

| Priority | Paper | Why it matters |
|---|---|---|
| **1** | **Arbor / Hypothesis-Tree Refinement** | Closest overall dynamic autonomous-research search system |
| **2** | **Mage** | Closest to your gates + rollback + failed-branch hints |
| **3** | **GCC** | Closest to versioned branch memory + on-demand historical context |
| **4** | **EviGraph** | Research graph + evidence repair + checkpoints + rollback |
| **5** | **FiF: Failure is Feedback** | Closest to backtracking that explicitly learns from failure |
| **6** | **DynaDebate** | Closest to dynamic paths + disagreement-triggered verification |
| **7** | **MemTX** | Makes “transactional shared memory” hard to claim |
| **8** | **MRAgent** | Makes agent-driven on-demand KG/history retrieval hard to claim |
| **9** | **MemSIF** | Makes “core memory + on-demand memory” hard to claim |
| **10** | **ChronoMem** | Semantic rollback of memory |
| **11** | **AgentGit** | Multi-agent branching/revert/versioning |
| **12** | **AIDE / AI Scientist-v2** | Search baselines for code/research |
| **13** | **AlphaEvolve-family** | Evolutionary-search baseline |

| System | Branch | Multi-agent | Failure memory | Evidence graph | Rollback | **Decision-level rollback from valid failure** |
|---|---|---|---|---|---|---|
| AIDE | ✓ | – | limited | – | tree revisit | ✗ |
| LATS | ✓ | – | reflection | – | ✓ | ✗ |
| AI Scientist-v2 | ✓ | limited | limited | – | search | ✗ |
| AutoScientists | **✓** | **✓** | **✓** | shared state | adaptive | ✗ / weak |
| Negative Knowledge | – | – | **✓** | structured records | – | ✗ |
| ChronoMem | – | – | memory versions | – | **✓** | ✗ |
| EviGraph | ✓ | components | some | **✓** | **✓** | **not its central problem** |
| **ARIADNE-Bio** | **✓** | **✓** | **✓** | **✓** | **✓** | **THIS SHOULD BE THE CONTRIBUTION** |
|  |  |  |  |  |  |  |

> [!attachment] File in Notion: **Reversible_Exploration_Plain_Language_v2.docx** (371.2 KiB) - [open the Notion page](https://app.notion.com/p/3b3d68da32f08075814ed771e3e9af2b)

> [!attachment] File in Notion: **Retro-Fork_Proposal_v3.docx** (14.3 KiB) - [open the Notion page](https://app.notion.com/p/3b3d68da32f08075814ed771e3e9af2b)

- [x] Read Shepherd paper
    - Shepherd paper [https://arxiv.org/html/2605.10913v3](https://arxiv.org/html/2605.10913v3)
    - In appendix D, they mentioned a method that write a hint for the forked idea from the past
    - They behave look like a git versioning for idea which have roling back and branching and even merges
    - They reset the knowledge when restarting or forking (They hope for randomness in this style)
    - The merge here is only about using the finding of child process to the main stream o fathers no specific way to merge ideas

### 2026-07-27 One-on-One

> [!summary] This meeting in short
> Decision: stay close to MAD and MoA. Files: ARIADNE 2-page proposal, MoA baselines, benchmark options.

*Original: [Notion page](https://app.notion.com/p/3add68da32f080569881dd768e204202)*

#### we dicided to stay close to the MAD and MOA

> [!attachment] File in Notion: **proposal_ariadne_2pager.docx** (15.5 KiB) - [open the Notion page](https://app.notion.com/p/3add68da32f080569881dd768e204202)

> [!attachment] File in Notion: **moa_baselines.docx** (14.6 KiB) - [open the Notion page](https://app.notion.com/p/3add68da32f080569881dd768e204202)

> [!attachment] File in Notion: **ariadne_benchmark_options (3).docx** (20.9 KiB) - [open the Notion page](https://app.notion.com/p/3add68da32f080569881dd768e204202)

> [!attachment] File in Notion: **five_benchmarks.docx** (16.3 KiB) - [open the Notion page](https://app.notion.com/p/3add68da32f080569881dd768e204202)

## 4. All notes linked to this direction

### Reports and experiments (2)

- [[Retro-Fork Proposal v3]] - File only: Retro-Fork proposal (version 3).
- [[Reversible Exploration (Plain Language v2)]] - File only: plain-language description of reversible exploration for research agents.

### Papers (13)

- [[AIDE - ML Engineering as Code Search]] - AIDE searches over whole Python scripts with a solution tree. No shared knowledge, no merging. Close to our idea and a search baseline.
- [[ALE-Bench]] - Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.
- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[BioML-bench]] - 24 end-to-end biomedical ML tasks (drug discovery, imaging, protein engineering, single-cell). Tests full ML pipelines by agents.
- [[BioML-bench - Drug Discovery Tasks]] - The 9 drug discovery tasks of BioML-bench (potency, ADME, toxicity) from Polaris and TDC.
- [[Biomni - General Biomedical Agent]] - Biomni builds a large biomedical action space (tools, databases, software) from 2,500 papers. Good for medical baselines.
- [[BixBench]] - 61 expert bioinformatics capsules with 205 questions. Decided to ignore it because it needs a specific environment.
- [[BixBench - Tasks and Baselines]] - Details of BixBench tasks (bioinformatics capsules) and baselines.
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[GENEDISCO]] - Benchmark for choosing which genes to test next in costly experiments (active learning for drug target discovery).
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[MoA - Mixture-of-Agents]] - Layers of proposer LLMs and aggregator LLMs. Agents work separately and aggregators control the flow.
- [[RE-Bench]] - Seven open-ended ML research engineering environments with a scoring function, compared with human experts.

### Literature reviews and lists (2)

- [[Review - Baselines for Gaps Protocol]] - Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.
- [[Review - Benchmarks for MoA and Multi-Agent Systems]] - Folder page for benchmark notes (GENEDISCO, RE-Bench, ALE-Bench, BioML, BixBench).

