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

- **Tree search over research:** [[Arbor - Hypothesis Tree Refinement|Arbor]], [[The AI Scientist (v1 and v2)|AI Scientist-v2]], [[AIDE - ML Engineering as Code Search|AIDE]], [[MLEvolve - Monte Carlo Graph Search|MLEvolve]].
- **Studies of search strategy (exploration vs refinement):** [[FML-bench - Search Dynamics of Research Agents|FML-bench]], [[AIRA-dojo - Search Policies and Operators|AIRA-dojo]].
- **Pipelines and long runs:** [[AI-Researcher and Scientist-Bench|AI-Researcher]], [[Kosmos - AI Scientist with World Model|Kosmos]], [[Robin - Drug Repurposing Agent|Robin]], [[AutoResearchClaw]], [[ScientistTwo]], Coscientist.
- **Long-horizon agents with memory (2026):** [[ML-Master 2.0 - Hierarchical Cognitive Caching|ML-Master 2.0]], [[ScienceFlow - Re-Anchoring Long-Horizon Agent|ScienceFlow]], [[AiScientist - File-as-Bus Long-Horizon Engineering|AiScientist]], [[DeepScientist - Findings Memory|DeepScientist]], [[Failure Registry - Recovering Wasted Compute|failure registry]].
- **Hypothesis generation (no experiments):** [[Google AI Co-Scientist|Co-Scientist]], [[ResearchAgent - Iterative Idea Generation|ResearchAgent]], [[MOOSE-Chem - Chemistry Hypothesis Rediscovery|MOOSE-Chem]].
- **Simple experiment loops:** [[Autoresearch (Karpathy)|autoresearch]].
- **Memory and rollback:** [[Mage - Memory as Execution State|Mage]], [[EviGraph - Evidence-Guided Research Agents|EviGraph]], GCC, ChronoMem, AgentGit, Shepherd.
- **Biomedical agents:** [[Biomni - General Biomedical Agent|Biomni]], [[BioDiscoveryAgent]], [[SAGA]], [[Virtual Lab]].

Full comparison: [[AR 4 - Related Work and Baselines]].

## Where it is used

- [[AR 0 - Start Here|Auto Researcher]] (main topic)
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

- [[AR 6 - Paper Draft (Introduction)]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Papers (27)

- [[AI-Researcher and Scientist-Bench]] - Paper-to-paper pipeline with an advisor agent; Scientist-Bench of 22 papers, judged by LLMs (NeurIPS 2025).
- [[AIDE - ML Engineering as Code Search]] - AIDE searches over whole Python scripts with a solution tree. No shared knowledge, no merging. Close to our idea and a search baseline.
- [[AIRA-dojo - Search Policies and Operators]] - Meta's study on MLE-bench: an agent = search policy + operators. Better operators mattered more than greedy vs MCTS vs evolution. Agents overfit to validation.
- [[AiScientist - File-as-Bus Long-Horizon Engineering]] - Orchestrator plus a shared File-as-Bus workspace with append-only logs; 81.82% on MLE-bench Lite. No code.
- [[ALE-Bench]] - Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.
- [[AlphaEvolve]] - Evolutionary coding agent: LLMs propose program changes, automatic evaluation selects survivors. Creativity comes from a pool of ideas that keeps improving.
- [[Arbor - Hypothesis Tree Refinement]] - Arbor keeps the whole research state in a persistent hypothesis tree. A long-lived coordinator manages the tree; short-lived executors test one hypothesis each in a...
- [[Autoresearch (Karpathy)]] - Greedy edit, train, keep-or-reset loop with git as memory. Simple and popular baseline.
- [[AutoResearchClaw]] - 23-stage idea-to-paper pipeline with PROCEED / REFINE / PIVOT and a lesson store. Judged by an LLM.
- [[DeepScientist - Findings Memory]] - Bayesian-optimization research loop with a Findings Memory of all ideas, failed ones too; month-long runs (ICLR 2026).
- [[EviGraph - Evidence-Guided Research Agents]] - Graph of evidence with long and short memory and rollback to a better earlier state. Uses the graph for evidence, not for thinking steps. Not very similar to our work.
- [[Failure Registry - Recovering Wasted Compute]] - Plug-in failure registry with banned patterns, proven fixes and backtracking; AIDE golds 22 to 38.
- [[FML-bench - Search Dynamics of Research Agents]] - 18 ML research tasks to compare the search behavior of six agents. Early gains predict the final score; scattered search does not help. AdaptiveSearch (greedy, then branch when stuck) is close to our exploration idea.
- [[Google AI Co-Scientist]] - Generate, debate, Elo-rank and evolve biomedical hypotheses (Nature 2026). No public code; does not run its own experiments.
- [[Kosmos - AI Scientist with World Model]] - Long runs with a shared structured world model; keeps conclusions, not paths. Commercial, no code.
- [[Mage - Memory as Execution State]] - Memory for long-horizon agents as execution state: one layer for raw step data, one layer for summaries. Critical of plain RAG memory.
- [[ML-Master 2.0 - Hierarchical Cognitive Caching]] - Three memory levels (traces, phase summaries with reasons, cross-task wisdom); 56.44% medals on full MLE-bench. Strongest memory rival.
- [[MLEvolve - Monte Carlo Graph Search]] - UCT tree with cross-branch reference edges and a success/failure memory; 65.3% any medal on full MLE-bench in 12 h.
- [[MOOSE-Chem - Chemistry Hypothesis Rediscovery]] - Chemistry hypothesis rediscovery from inspiration papers with evolution and beam search (ICLR 2025). No experiments.
- [[RE-Bench]] - Seven open-ended ML research engineering environments with a scoring function, compared with human experts.
- [[ResearchAgent - Iterative Idea Generation]] - Problem, method and experiment design from a core paper, refined by 15 reviewer agents. No experiments (NAACL 2025).
- [[Robin - Drug Repurposing Agent]] - FutureHouse literature and data-analysis agents for drug repurposing; found ripasudil for dry AMD (Nature 2026). Needs the paid Edison API.
- [[SAGA]] - SAGA paper files. We tried to run SAGA (Compute Canada, then cloud with gpt-mini).
- [[ScienceFlow - Re-Anchoring Long-Horizon Agent]] - Continue or jump back to archived checkpoints with their evidence; 70.22% on full MLE-bench. Closest to reopening old ideas.
- [[ScientistTwo]] - Google system that improves published ML methods (86 of 107 wins); Good / Bad / Engineer triage; about $3.8k per task, no code.
- [[The AI Scientist (v1 and v2)]] - Sakana's idea-to-paper agent; v2 uses a staged best-first tree search and keeps only the best node per stage. Nature 2026.
- [[Virtual Lab]] - AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

### Literature reviews and lists (1)

- [[Review - Baselines for Gaps Protocol]] - Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.

