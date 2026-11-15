---
type: review
date: 2026-08-13
status: In Progress
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Creativity and Exploration", "Benchmarks and Datasets"]
tags: [type/review, project/creativity-debate, project/auto-researcher, topic/creativity-and-exploration, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bbd68da32f080768070e83351042619
created: 2026-08-13
updated: 2026-08-14
---
# Review - Creativity Measuring Benchmarks

> [!summary] In short
> Overview of benchmarks that measure creativity (CreativityBench, MacGyver, DeepMath-Creative, EscapeBench, MUTATE, bio tasks).

**Project:** [[Creativity in Multi-Agent Debate]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Creativity and Exploration]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Sub-notes:** [[EscapeBench]], [[GENEDISCO - Creativity View]], [[SMDD-Bench]], [[PMO - Practical Molecular Optimization]], [[BioDiscoveryAgent]], [[MUTATE - Divergent Thinking Benchmark]]  
**Original:** [Notion page](https://app.notion.com/p/3bbd68da32f080768070e83351042619)

---

### Remaining things that not explored yet
- **CreativityBench (2026)** — evaluates agent creative reasoning via affordance-based tool repurposing, targeting the discovery of unobvious functional connections grounded in physical reality — divergent thinking anchored to physical constraints
- **VisEscape / VillagerBench** — multimodal and embodied extensions of creativity evaluation to perception-grounded tasks

**Creative problem-solving:**
- **MacGyver** — evaluates whether models can solve everyday problems by repurposing available objects in unconventional ways
- **DeepMath-Creative** — a benchmark of constructive math problems across algebra, geometry, and analysis; even the best model achieved only ~70% accuracy on basic undergraduate-level constructive tasks, suggesting performance reflects recombination of memorized patterns rather than authentic creative insight [arxiv](https://arxiv.org/pdf/2505.08744)

**Divergent-thinking / psychometric tests adapted from psychology:**
- Standardized psychological assessments like the Alternate Uses Test and the Torrance Tests of Creative Thinking, evaluating sub-dimensions like fluency, originality, and elaboration
- **DAT (Divergent Association Task)** — participants produce 10 nouns as different from one another as possible, scored by average pairwise semantic distance; a conditional variant (CDAT) was recently shown to be the best predictor of divergent thinking, correlating strongly with NoveltyBench [arXiv](https://arxiv.org/html/2601.20546v1)[arXiv](https://arxiv.org/pdf/2605.13450)
- **NoveltyBench** — evaluates output diversity on real-world user interactions and open-ended prompts across randomness, factual information, creative writing, and subjectivity [arXiv](https://arxiv.org/html/2605.13450)
- **Hivemind** — measures output diversity across open-ended queries drawn from real-world user-ChatGPT interactions [arXiv](https://arxiv.org/pdf/2605.13450)

**Domain-specific and human-judged:**
- **Creativity Benchmark (marketing)** — covers 100 brands and three prompt types, judged via 11,012 pairwise comparisons from 678 practising creatives; notably, LLM-as-judge setups correlated weakly with human rankings, underscoring that automated judges can't substitute for human evaluation [arXiv](https://arxiv.org/abs/2509.09702)
- Creative-writing leaderboards like Chatbot Arena Creative Writing, EQBench CW, and Mazur's creative writing benchmark, plus **LiveIdeaBench** for scientific ideation

**Meta-benchmark (a good starting point for a literature survey):**
- **AGC-Bench (2026)** — a meta-benchmark built from a PRISMA-compliant systematic review that identified 497 unique creativity benchmarks, with a first release covering 78 datasets spanning brainstorming, problem solving, STEM, narrative, figurative language, and humor [arXiv](https://arxiv.org/html/2607.01152)

If you're designing your own experiment, a useful framing from this literature: creativity is usually decomposed into **novelty/diversity** (divergent thinking, often measured by semantic distance or entropy — one recent paper uses semantic entropy as a reference-free automated measure of divergent creativity, validated against human annotations) and **appropriateness/usefulness** (convergent thinking, usually needing human or multi-agent LLM judges). Good evaluations measure both, since maximizing one alone is trivial.
