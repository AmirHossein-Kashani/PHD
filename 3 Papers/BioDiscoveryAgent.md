---
type: paper
date: 2026-08-14
status: Done
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
notion-tags: ["bio"]
tags: [type/paper, project/creativity-debate, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bcd68da32f080538690ea608802d8cf
created: 2026-08-14
updated: 2026-08-14
---
# BioDiscoveryAgent

> [!summary] In short
> Stanford agent that designs gene perturbation experiments. Main metric: cumulative hit ratio over rounds.

**Project:** [[Creativity in Multi-Agent Debate]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Creativity Measuring Benchmarks]]  
**Original:** [Notion page](https://app.notion.com/p/3bcd68da32f080538690ea608802d8cf)

---

introduced by standford
designed for LLMs

  The ultimate goal of the agent is to find genes that cause a specific biological reaction when altered. To evaluate this, the framework relies heavily on a metric called the **Cumulative Hit Ratio**.

The ultimate goal of the agent is to find genes that cause a specific biological reaction when altered. To evaluate this, the framework relies heavily on a metric called the **Cumulative Hit Ratio**.  
  • **Cumulative Hit Ratio:** This is the primary metric. The evaluation does not just look at the single best gene found; it counts the *total cumulative number* of relevant genes (or "hits") the agent successfully identifies across a set number of experimental rounds (e.g., 5 rounds of testing batches of genes).  
  • **Performance on "Hard" Targets:** The evaluation specifically isolates how well the agent discovers "non-essential genes". (Essential genes are obvious targets that affect almost everything; non-essential genes require much deeper biological reasoning to link to a specific phenotype).
  • **Combinatorial Accuracy:** For more complex tasks, the benchmark measures the agent's accuracy in predicting the effects of perturbing *combinations* of genes at the same time, usually comparing its hit rate against a random baseline or standard Bayesian Optimization models.

  • **Unpublished Data Validation:** To prove the LLM is actually reasoning and not just regurgitating its training data, the benchmark evaluates the agent's hit ratio on recently generated, *unpublished* wet-lab datasets (like new CAR-T cell experiments). 

**2. Evaluating the Process (The Execution and Reasoning)**
  Because BioDiscoveryAgent acts as an autonomous scientist, *how* it arrives at its gene selections is just as important as the selections themselves. The framework evaluates the intermediate agentic process through several behavioral metrics:  
  • **Evidence Alignment and Citation:** Does the agent's proposed biological mechanism align with actual scientific literature? The system evaluates whether the reasoning the agent provides before making a choice matches the published findings it retrieved.
  • **Tool Execution Rates:** When the agent decides to write Python code to analyze the historical dataset from previous rounds, the framework evaluates the **code executability rate**—how often the code actually runs without syntax or logic errors.
  • **Hypothesis Accuracy (Type I / Type II Errors):** The system tracks the precision and recall of the agent's hypotheses. Does it confidently propose hypotheses that are actually non-verifiable, or does it accurately assess the uncertainty of the biological landscape?  
  • **Context and Memory Management:** A major point of failure for LLMs in long-horizon loops is forgetting. The process is evaluated on whether the agent avoids basic experimental errors, such as mistakenly requesting to test the exact same genes it already tested in a previous round.
  • **Critic Responsiveness:** If you employ the "AI Critic" tool node, the evaluation looks at whether the main actor agent actually updates its beliefs and explores a more diverse set of genes when challenged, rather than just stubbornly sticking to its initial obvious choices.
