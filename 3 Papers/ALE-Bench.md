---
type: paper
date: 2026-08-02
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)", "Auto Researcher"]
topics: ["Benchmarks and Datasets", "Autonomous Research Agents"]
tags: [type/paper, project/reversible-exploration, project/auto-researcher, topic/benchmarks-and-datasets, topic/autonomous-research-agents]
notion: https://app.notion.com/p/3b1d68da32f08047afc7fb9bcab7fe2e
created: 2026-08-03
updated: 2026-08-03
---
# ALE-Bench

> [!summary] In short
> Score-based long-horizon algorithm engineering from 40 AtCoder heuristic contests. Results map to human rankings.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Benchmarks and Datasets]], [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Benchmarks for MoA and Multi-Agent Systems]]  
**Original:** [Notion page](https://app.notion.com/p/3b1d68da32f08047afc7fb9bcab7fe2e)

---

### ALE-Bench — Research Notes (Potential Benchmark / Baseline)
**Paper:** *ALE-Bench: A Benchmark for Long-Horizon Objective-Driven Algorithm Engineering*
  Imajuku et al. (Sakana AI, Univ. of Tokyo, AtCoder) — NeurIPS 2025, Datasets & Benchmarks Track. arXiv:2506.09050
  **Code:** github.com/SakanaAI/ALE-Bench · **Data:** hf.co/datasets/SakanaAI/ALE-Bench

### What it is
The first benchmark for **score-based** (not pass/fail) competitive programming, built from 40 real AtCoder Heuristic Contest (AHC) problems. Tasks are NP-hard optimization problems (routing, scheduling, packing, planning, Bayesian inference) with no known optimum — the goal is to maximize a score through **long-horizon iterative refinement**, mirroring contests where humans improve solutions over 4 hours to 2 weeks. Because it reuses real contest leaderboards (~1,000 participants per contest), AI results map directly to human ranks. Open-ended: remains meaningful even after AI surpasses top humans.

### Dataset
- **Full version:** 40 problems (contests through April 2025); **Lite version:** 10 harder, genre-diverse problems for cheap AI-vs-AI comparison (full↔lite correlation r ≈ 0.92, but lite is harder — don't compare across versions).
- **Each problem package:** Markdown problem statement (EN/JP), Rust scorer, visualizer (local + interactive web), and original leaderboard data.
- **Framework:** Python library with a timed `Session` (view problem, test run, visualize, submit once), Docker sandbox replicating the AtCoder judge (C++/Python/Rust), Terraform scripts for standardized AWS C6i hardware. CPU-only → cheap to run (vs. MLE-Bench needing GPUs).
- **Metrics:** per-problem score/rank/**performance** (Elo-like, 0–3500); aggregate via **average performance + performance distribution** (recommended) — AtCoder-style rating overstates AI because it rewards spikes without penalizing failures.

### Baselines and results

| Setting | Best system | Avg. perf. | Notes |
|---|---|---|---|
| One-shot (≤5 tries) | o3-high | 1044 | Only model >1000; human avg ≈ 1260; no model hit 2000 on any problem |
| Iterative refinement (4h, Self-Refine style) | o4-mini-high | 1520 | Rating = top 11.8% of humans (inflated); +400 pts for all models vs. one-shot |
| **ALE-Agent** (their scaffold, Gemini 2.5 Pro, lite) | Method 1+2 | 1879 | Top ~6.8%; ≥2000 perf on 30% of problems; one solution = 5th place in real AHC039 |
| OpenHands (general agent) | — | ~600–900 | Barely better than one-shot; often exited early |

**ALE-Agent** = strong purpose-built baseline: (1) domain-knowledge prompting (simulated annealing, beam search, neighborhood design, speedups) + (2) diversity-oriented best-first search expanding ~30 LLM-generated children per node in parallel, tabu-style, 3-turn refinement per candidate. A live version placed 154th (perf 1915) in real contest AHC046.

### Key findings
- AI ≈ human novice-to-intermediate on average but **inconsistent**: spikes on SA-friendly short contests, weak on planning problems needing custom evaluation functions; short-contest perf > long-contest perf (wins via volume: 100–1000 generated programs).
- **Iterative ≠ parallel guessing:** 150 independent one-shot runs peak ~300+ points below one iterative run → benchmark genuinely measures long-horizon refinement.
- Contamination check (no jump at knowledge cutoffs) and manual plagiarism review both clean.

### Why useful for my work
- Ready-made **baselines to beat**: one-shot, 4h Self-Refine, ALE-Agent (all with published prompts/configs in appendix).
- Lite subset = affordable ablation testbed; full = human-comparable evaluation.
- Good target for agent-architecture research (search strategies, multi-agent roles, visualizer/tool use — largely unexplored in the paper's own experiments).

**Limitations:** only 40 problems; mostly single-run results (API cost); image/visualizer inputs not exercised in experiments.
