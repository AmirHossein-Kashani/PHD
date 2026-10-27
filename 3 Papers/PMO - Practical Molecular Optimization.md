---
type: paper
date: 2026-08-14
status: Done
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
notion-tags: ["bio"]
tags: [type/paper, project/creativity-debate, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bcd68da32f08004b793ed526dbf392d
created: 2026-08-14
updated: 2026-08-19
---
# PMO - Practical Molecular Optimization

> [!summary] In short
> Benchmark with 23 molecule objectives and a 10K oracle budget; scores the path too (AUC top-10). Idea: use intermediate rewards in long horizons.

**Project:** [[Creativity in Multi-Agent Debate]], [[Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Creativity Measuring Benchmarks]]  
**Original:** [Notion page](https://app.notion.com/p/3bcd68da32f08004b793ed526dbf392d)

---

score the final result and the path of solution 
it is not generally for multi agents  or llm but it is a good option
you have limitation in try and errors.
!!! potential thing in this approach is that we can use intermidate reward or criteria when we are approaching to a solution in long horizon

About the data sets
  • **23 Optimization Tasks (Oracles):** The benchmark includes 23 distinct objective functions. These range from simple property targets like QED (drug-likeness) and DRD2 (binding affinity) to complex multi-property objectives (MPOs) relevant to pharmaceutical design.  
  • **The 10K Oracle Budget:** This is the defining constraint. Algorithms are strictly limited to 10,000 queries to the oracle. This prevents methods from brute-forcing the chemical space over millions of iterations.  
  • **The AUC Top-10 Metric:** Instead of merely reporting the single highest-scoring molecule at the end of a run, PMO measures the Area Under the Curve (AUC) of the top-10 average scores plotted against the number of oracle calls. This evaluates how quickly and consistently a method finds good candidates.  
  • **Standardized Starting Point:** Whenever a method requires a database to initialize its search, PMO forces the use of the ZINC 250K dataset to ensure all algorithms start on a level playing field.  
  • **25 Built-in Baselines:** The repository includes 25 diverse algorithms categorized by optimization strategy (e.g., Genetic Algorithms, Reinforcement Learning, Bayesian Optimization, Score-Based Modeling, and Hill Climbing).  

**What You Can Do with PMO**
  • **Benchmark Agent Efficiency:** You can directly compare a custom agent's performance against strong historical baselines to see if dynamic decision-making outperforms static algorithmic loops.
  • **Test Multiple Representations:** PMO supports various molecular assembly strategies, including SMILES, SELFIES, atom-level graphs, and fragment-based assembly. You can evaluate how well your agent navigates these different chemical representations.  
  • **Analyze Optimization Landscapes:** By testing your agent across the 23 different oracles, you can analyze its behavior on different types of optimization landscapes—such as finding out if it excels at string-based mutations or graph-level fragment combinations.
