---
type: paper
date: 2026-08-02
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)", "Auto Researcher"]
topics: ["Benchmarks and Datasets", "Autonomous Research Agents"]
tags: [type/paper, project/reversible-exploration, project/auto-researcher, topic/benchmarks-and-datasets, topic/autonomous-research-agents]
notion: https://app.notion.com/p/3b0d68da32f080a08f56d437caed1b49
created: 2026-08-02
updated: 2026-08-02
---
# RE-Bench

> [!summary] In short
> Seven open-ended ML research engineering environments with a scoring function, compared with human experts.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]], [[Auto Researcher]]  
**Topics:** [[Benchmarks and Datasets]], [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Benchmarks for MoA and Multi-Agent Systems]]  
**Original:** [Notion page](https://app.notion.com/p/3b0d68da32f080a08f56d437caed1b49)

---

**What the benchmark consists of**
It's a suite of seven hand-crafted, open-ended ML research engineering environments. Each one presents a realistic optimization problem — for example, speeding up a fine-tuning script (Optimize LLM Foundry), writing a fast custom GPU kernel in Triton (Optimize a Kernel), recovering a model whose embedding weights were scrambled (Fix Embedding), predicting optimal hyperparameters from small-scale experiments (Scaling Law Experiment), building a language model from a restricted set of PyTorch primitives, fine-tuning GPT-2 into a chatbot using RL, and scaffolding GPT-3.5 to solve competition programming problems in Rust.
Every environment gives the agent (human or AI) the same setup: a machine with up to 6 H100 GPUs, a simple but weak starting solution, and a scoring function it can run at any time. Scores are normalized so the starting solution counts as 0 and a strong reference solution written by the task author counts as 1, making results comparable across tasks.
**The introduced baseline: human experts**
This is the paper's key contribution and what distinguishes it from prior benchmarks. The baseline is 71 eight-hour attempts by 61 distinct human ML experts, working under conditions equivalent to those given to the AI agents (SSH into the same VMs, same instructions, same scoring). The experts came from three sources: METR's professional networks (people with 5+ years of relevant experience or from labs like DeepMind, Anthropic, and OpenAI), applicants to METR's research scientist/engineer hiring process, and graduate students from top ML PhD programs.
The baseline validates that the tasks are feasible — 82% of expert attempts achieved a non-zero score, and 24% matched or beat the reference solutions. Average scores varied a lot by source: professional-network experts averaged 0.96 while hiring applicants averaged 0.46, which is why the authors caution against comparing agents only to the *average* human and suggest comparing to top human attempts when assessing automation potential.
**The headline comparison**
Against this baseline, AI agents (Claude 3.5 Sonnet and o1-preview) scored about 4× higher than humans at a 2-hour budget, since they iterate over 10× faster. But humans show much better returns to time: they narrowly beat the best agents at 8 hours and achieved roughly 2× the top agent score at 32 hours. So the human baseline serves both as a feasibility check and as the grounding for interpreting what agent scores actually mean for AI R&D automation risk.
