---
type: paper
date: 2026-08-01
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/reversible-exploration, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3aed68da32f080509781c8ca2c97b533
created: 2026-07-31
updated: 2026-08-14
---
# GENEDISCO

> [!summary] In short
> Benchmark for choosing which genes to test next in costly experiments (active learning for drug target discovery).

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Benchmarks for MoA and Multi-Agent Systems]]  
**Original:** [Notion page](https://app.notion.com/p/3aed68da32f080509781c8ca2c97b533)

---

**The situation.** Scientists want to know which genes cause diseases. There are about 20,000 genes to check. Checking one means running a real lab experiment: disable that gene in some cells, see what happens. Experiments cost money and time, so you can only do a few thousand — nowhere near all of them.
**The question.** Can a computer look at the experiments you've done so far and guess which gene is worth testing next? If it's good at guessing, you find the important genes much faster.
**The obstacle.** Nobody could tell whether such a computer program was any good, because there was no fair test to compare programs against. Everyone made up their own.
**What the authors built.** A test. They found four big published studies where scientists had already, exhaustively, tested *every* gene — so all the answers are known and written down. Now you can pretend: hide the answers, let a program pick genes one batch at a time, and each time it picks, just look up what really happened. It's a flight simulator for lab experiments. Cheap, fast, and the outcomes are real rather than made up.
They also packaged three ways of describing a gene to the computer (so it can tell which genes resemble each other), plus nine off-the-shelf strategies for choosing what to test next, and ran every combination — 20,000+ hours of computing — to see how they stack up.
**What they learned.** The strategies split into two camps, and neither wins outright:
- *Spread out.* Pick genes that are different from each other, cover the whole map. This teaches the computer the most — its predictions get accurate fastest.
- *Chase uncertainty.* Pick genes the computer is most unsure about. This finds more of the genes that actually matter — the big-effect ones a biologist would care about.

The catch is that the strategy that makes the smartest computer isn't the one that finds the most interesting genes. Building a good map and finding treasure turn out to be different jobs. Figuring out how to do both at once is the problem they're handing to the field.
**The honest caveats.** Real lab experiments are noisy — sometimes you should repeat an experiment rather than run a new one, and the simulator ignores that entirely. And which strategy looks best shifts depending on which study and which gene-description you use, so don't over-trust any single ranking.

- We are selecting among tested 
- BioDiscoveryAgent (Roohani et al., Stanford/UCSF, 2024)
