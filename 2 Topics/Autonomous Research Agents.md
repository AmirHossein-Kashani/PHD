---
type: topic
aliases: [AI scientist, research agent, auto researcher]
tags: [type/topic, topic/autonomous-research-agents]
---
# Autonomous Research Agents

> [!summary] In short
> AI systems that do research by themselves: make hypotheses, write and run experiments, read results, and change the plan. A survey groups them into three levels: **Tool** (one task), **Analyst** (multi-step analysis of given data) and **Scientist** (runs the whole discovery loop). Main open problems: long context, weak long-term planning, memory that loses the path, and too little exploration.

## A map of the field (from the Notion category page)

- **What is searched:** procedures (code, prompts, configs), concepts (hypotheses, explanations), actions (protocols, tool sequences).
- **How the search is controlled:** evolutionary / population-based, tournament / critique-based, gradient-like feedback, planner-driven.
- **Three families:** algorithm discovery ([[AlphaEvolve]]), hypothesis and answer generation (Kosmos, AI co-scientist), automating scientific workflows ([[Virtual Lab]]).
- See [[Notion Category - Agentic AI Researchers]] for the full text.

## Key systems in my notes

- **Tree search over research:** [[Arbor - Hypothesis Tree Refinement|Arbor]], AI Scientist-v2, [[AIDE - ML Engineering as Code Search|AIDE]].
- **Studies of search strategy (exploration vs refinement):** [[FML-bench - Search Dynamics of Research Agents|FML-bench]], [[AIRA-dojo - Search Policies and Operators|AIRA-dojo]].
- **Pipelines and long runs:** AI-Researcher, Kosmos, Robin, Coscientist.
- **Memory and rollback:** [[Mage - Memory as Execution State|Mage]], [[EviGraph - Evidence-Guided Research Agents|EviGraph]], GCC, ChronoMem, AgentGit, Shepherd.
- **Biomedical agents:** [[Biomni - General Biomedical Agent|Biomni]], [[BioDiscoveryAgent]], [[SAGA]], [[Virtual Lab]].

Full comparison: [[AR - Related Systems and Baselines]].

## Where it is used

- [[Auto Researcher]] (main topic)
- [[Reversible Exploration (ARIADNE and Retro-Fork)]]

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-07-21 One-on-One

*Original: [Notion page](https://app.notion.com/p/3a4d68da32f080caaa03d4375e78da67)*  
*Other parts of this meeting:* [[Claim-Level Conflict in Debate#2026-07-21 One-on-One|Claim-Level Conflict in Debate]], [[MS - Meeting Log#2026-07-21 One-on-One|MS - Meeting Log]]

**1. FunSearch**
Romera-Paredes, B., Barekatain, M., Novikov, A., Balog, M., Kumar, M. P., Dupont, E., Ruiz, F. J. R., Ellenberg, J. S., Wang, P., Fawzi, O., Kohli, P., & Fawzi, A. (2024). *Mathematical discoveries from program search with large language models*. **Nature**, 625(7995), 468–475. DOI: 10.1038/s41586-023-06924-6 [arxiv](https://arxiv.org/pdf/2604.20622)
- Paper: [https://www.nature.com/articles/s41586-023-06924-6](https://www.nature.com/articles/s41586-023-06924-6) (open access)
- Code: [https://github.com/google-deepmind/funsearch](https://github.com/google-deepmind/funsearch)

**2. AlphaEvolve**
Novikov, A., Vũ, N., Eisenberger, M., Dupont, E., Huang, P.-S., et al. (2025). *AlphaEvolve: A coding agent for scientific and algorithmic discovery*. **arXiv preprint arXiv:2506.13131**. DOI: 10.48550/arXiv.2506.13131 [arxiv](https://arxiv.org/pdf/2605.11359)
- Paper: [https://arxiv.org/abs/2506.13131](https://arxiv.org/abs/2506.13131)
- Note: AlphaEvolve is a white paper/preprint from Google DeepMind rather than a peer-reviewed journal article (at least as of now), so cite it as an arXiv preprint.

## Notes on this topic

### Reports and experiments (2)

- [[Retro-Fork Proposal v3]] - File only: Retro-Fork proposal (version 3).
- [[Reversible Exploration (Plain Language v2)]] - File only: plain-language description of reversible exploration for research agents.

### Drafts (1)

- [[AR Paper - Introduction Draft]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Papers (11)

- [[AIDE - ML Engineering as Code Search]] - AIDE searches over whole Python scripts with a solution tree. No shared knowledge, no merging. Close to our idea and a search baseline.
- [[AIRA-dojo - Search Policies and Operators]] - Meta's study on MLE-bench: an agent = search policy + operators. Better operators mattered more than greedy vs MCTS vs evolution. Agents overfit to validation.
- [[ALE-Bench]] - Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.
- [[AlphaEvolve]] - Evolutionary coding agent: LLMs propose program changes, automatic evaluation selects survivors. Creativity comes from a pool of ideas that keeps improving.
- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[FML-bench - Search Dynamics of Research Agents]] - 18 ML research tasks to compare the search behavior of six agents. Early gains predict the final score; scattered search does not help. AdaptiveSearch (greedy, then branch when stuck) is close to our exploration idea.
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[RE-Bench]] - Seven open-ended ML research engineering environments with a scoring function, compared with human experts.
- [[SAGA]] - SAGA paper files. We tried to run SAGA (Compute Canada, then cloud with gpt-mini).
- [[Virtual Lab]] - AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

### Literature reviews and lists (1)

- [[Review - Baselines for Gaps Protocol]] - Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.

