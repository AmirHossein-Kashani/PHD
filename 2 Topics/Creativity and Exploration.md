---
type: topic
aliases: [creativity, exploration, novelty, diversity]
tags: [type/topic, topic/creativity-and-exploration]
---
# Creativity and Exploration

> [!summary] In short
> **Creative output = new and useful.** We can see creativity as **search**: make many variations and keep the good ones. Then the key question is how to split the budget between **exploration** (new ideas) and **exploitation** (refining known ideas). LLM agents tend to under-explore and to defend ideas they already have.

## Key ideas from my notes

- **Classic view:** problem solving is search (Newell and Simon); creative = new + useful (Runco and Jaeger); variation and selection (Campbell, Boden).
- **For LLMs:** more samples give more coverage; adaptive test-time compute is more efficient; LLMs do not explore well in bandit tasks without help.
- **Fixation:** confirmation bias, design fixation, escalation of commitment. Self-reflection can make it worse.
- **Remedies:** external pressure (debate, multiple working hypotheses) or a **budgeted exploration** (novelty search, quality-diversity archives).
- **My experiments** ([[Creativity in Multi-Agent Debate]]): persona diversity works; identical context collapses; originality-first ranking is unsafe for medicine.
- **My ideas:** quality-diversity on Arbor; ambitious short-lived sub-agents; agents that know when to move up, down, sideways, merge or return; combine "jumping ideas" with refinement. See [[03 Ideas Inbox]].
- **Measuring creativity:** [[Review - Creativity Measuring Benchmarks]], [[EscapeBench]], [[MUTATE - Divergent Thinking Benchmark|MUTATE]].

## Where it is used

- [[AR 0 - Start Here|Auto Researcher]] (exploration budget)
- [[Creativity in Multi-Agent Debate]]
- [[Early Exploration (May-June 2026)]] (the starting question)

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-25 One-on-One

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-08-25 One-on-One|AR - Meeting Log]], [[Creativity in Multi-Agent Debate#2026-08-25 One-on-One|Creativity in Multi-Agent Debate]], [[Benchmarks and Datasets#2026-08-25 One-on-One|Benchmarks and Datasets]], [[Agent Architectures and Frameworks#2026-08-25 One-on-One|Agent Architectures and Frameworks]], [[MS - Meeting Log#2026-08-25 One-on-One|MS - Meeting Log]]

Spliting the problem, finding good questions
The much more interesting agent is one that knows when it should move:
down, up, sideways, merge branches, or return to an old branch.
Then creativity becomes partly about asking better scientific questions, not only generating more ideas.

### 2026-06-02 One-on-One

*Original: [Notion page](https://app.notion.com/p/374d68da32f080f1a6f5c6b4bb9b81b0)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-02 One-on-One|Early Exploration (May-June 2026)]], [[MS - Meeting Log#2026-06-02 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-02 One-on-One|Skills and Tools]], [[Multi-Agent Debate#2026-06-02 One-on-One|Multi-Agent Debate]]

#### Next week plan
- [ ] continue the topic of creativity in models

### 2026-05-26 One-on-One

> [!summary] This meeting in short
> How creative are LLMs? They depend on training data. RL can be more creative because it explores the reward function.

*Original: [Notion page](https://app.notion.com/p/36cd68da32f080098284d67d9b811972)*

> [!attachment] File in Notion: **Report_26thMay.pptx** (2 MiB) - [open the Notion page](https://app.notion.com/p/36cd68da32f080098284d67d9b811972)

how creative are larg language model and limitation of llms, they are depends on training data RL is more creative
  it will explore the reward function. combine the llm and the reward function eplore more creatively.
SFT data se
  on policy learning

## Notes on this topic

### Reports and experiments (6)

- [[Creativity Experiments - Datasets]] - The 4 small datasets used in the creativity experiments: med_ddx (real MedQA), med_repurpose (synthetic), gen_aut (alternative uses), gen_problems (synthetic city...
- [[Creativity Experiments - Key Findings]] - Five main findings from both creativity experiments: identical-context debate fails, surface changes do nothing, persona is the best lever, the aggregator matters...
- [[Creativity Exp 1 - Index and Conclusion]] - Index of both creativity experiments and the full conclusion of Exp 1 (masking and shuffling of context between debating agents).
- [[Creativity Exp 1 - Full Results]] - Full statistics of Exp 1. Masking raised agent diversity; shuffling did nothing; plain debate was worse than one agent.
- [[Creativity Exp 2 - Conclusion]] - Conclusion of Exp 2 (asymmetry levers). Persona works best; representation is a null; the new aggregator helped creativity but lost the correct diagnosis.
- [[Creativity Exp 2 - Full Results]] - Full statistics of Exp 2 (persona, masking, representation, aggregator bottleneck, medical safety).

### Drafts (1)

- [[AR 6 - Paper Draft (Introduction)]] - Draft of the auto-researcher paper introduction: levels of scientific agents, our level, limits (long context, planning, memory, exploration), closest systems and our...

### Papers (10)

- [[AIRA-dojo - Search Policies and Operators]] - Meta's study on MLE-bench: an agent = search policy + operators. Better operators mattered more than greedy vs MCTS vs evolution. Agents overfit to validation.
- [[AlphaEvolve]] - Evolutionary coding agent: LLMs propose program changes, automatic evaluation selects survivors. Creativity comes from a pool of ideas that keeps improving.
- [[DeepScientist - Findings Memory]] - Bayesian-optimization research loop with a Findings Memory of all ideas, failed ones too; month-long runs (ICLR 2026).
- [[EscapeBench]] - Text-only room-escape games to measure creative intelligence of LLM agents. Top models reach only about 15% progress without hints.
- [[FML-bench - Search Dynamics of Research Agents]] - 18 ML research tasks to compare the search behavior of six agents. Early gains predict the final score; scattered search does not help. AdaptiveSearch (greedy, then branch when stuck) is close to our exploration idea.
- [[MLEvolve - Monte Carlo Graph Search]] - UCT tree with cross-branch reference edges and a success/failure memory; 65.3% any medal on full MLE-bench in 12 h.
- [[MOOSE-Chem - Chemistry Hypothesis Rediscovery]] - Chemistry hypothesis rediscovery from inspiration papers with evolution and beam search (ICLR 2025). No experiments.
- [[MUTATE - Divergent Thinking Benchmark]] - Text sandboxes where the obvious solution fails. Measures path-level and action-level divergence of agents. Close to EscapeBench.
- [[ResearchAgent - Iterative Idea Generation]] - Problem, method and experiment design from a core paper, refined by 15 reviewer agents. No experiments (NAACL 2025).
- [[Tree of Thoughts and Graph of Thoughts]] - Reasoning as a tree or a graph of thoughts with generate, refine and aggregate steps.

### Literature reviews and lists (1)

- [[Review - Creativity Measuring Benchmarks]] - Overview of benchmarks that measure creativity (CreativityBench, MacGyver, DeepMath-Creative, EscapeBench, MUTATE, bio tasks).

### Learning (3)

- [[Creativity in AI (folder)]] - Folder page for the question "is AI creative?".
- [[Is AI Really Creative]] - Open question page. Holds the Mila talk note.
- [[Rethinking Creativity in the Age of GenAI (Mila talk)]] - Talk note: test creativity with "give 10 unrelated words"; some models beat the median human.

