---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2609.19644)
link: https://arxiv.org/abs/2609.19644
code: no public framework code found
ar_role: related work (end-to-end, strong claims)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Evaluation and Metrics"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/evaluation-and-metrics]
created: 2026-10-27
updated: 2026-10-27
---
# ScientistTwo

> [!summary] In short
> Google Cloud AI Research system that improves the method of a published ML paper and writes a new paper. It claims to beat the original method on 86 of 107 recent problems, but it costs about $3,800 per task and is mostly scored by automated reviewers.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Evaluation and Metrics]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2609.19644](https://arxiv.org/abs/2609.19644) · [project page](https://scientist-two.github.io/)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the project page. Not read in full by me yet.

---

## Paper

- **Title:** *ScientistTwo: Pioneering the Human Knowledge Frontier with Autonomous AI.*
- **Authors:** Jaehyun Nam, Jinsung Yoon, Yanzhou Pan, Yubo Wang, Rui Meng, Parthasarathy Ranganathan, Tomas Pfister (Google Cloud AI Research). arXiv, 17 Sep 2026.
- **Code:** no framework code found. Uses Gemini 3.6 Flash and Claude Opus 4.8 (coding through Claude Code).

## Problem

Can an agent push past the state of the art on real, recent ML problems, not only reproduce papers?

## Method (main idea)

- Start from a problem plus its baseline paper and code. List the baseline's weaknesses and make seed ideas ranked by novelty.
- Test each idea on a **subset** of the benchmark and label it **Good, Bad or "Engineer"** (worth more tuning). Only Good ideas go to the full benchmark.
- An **evolver** agent sees the execution traces of all rounds plus the current best idea, its results and code.
- A selector picks the best idea; ablation agents refine it; a change is kept only if it beats the previous best.
- A rebuttal agent runs new experiments until the review score reaches 8 or the budget ends.

## Results

- 107 problems from NeurIPS 2025, ICLR 2026 and ICML 2026 papers.
- Beat the original method on 86/107 (80.4%); mean gain +25.2% (median 7.7%).
- Automated reviewers: 7.5 average (ScholarPeer, which is also used for drafting) and 5.7 (Stanford Agentic Reviewer, held out). 9 human reviewers on 33 papers: about equal to human papers, humans better on rigour.

## Limits

- Local improvements, not new theory. About $3.8k per task.
- Mostly automated reviewing; one reviewer is also used in drafting. ML only.

## How it relates to my work

- **Similar to:** staged testing (subset first, then full), like Arbor's dev vs held-out score. The "Engineer" label = an idea worth refining instead of dropping, close to our failure refinement.
- **Different from my work because:** the evolver sees all traces as one flat list, not a path. Bad ideas are not reopened.
- **Can I use it as a baseline?** No (no code, too expensive). But its task design (improve a recent paper's method, measure relative gain) is a good evaluation idea, close to MLRC-Bench.

## Ideas it gave me

- **Good / Bad / Engineer** triage for each tested idea.
