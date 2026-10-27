---
type: report
date: 2026-08-20
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Creativity and Exploration", "Multi-Agent Debate", "Evaluation and Metrics"]
tags: [type/report, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/3c2d68da32f08010bbf2db70b601d597
created: 2026-08-20
updated: 2026-08-20
---
# Creativity Exp 2 - Full Results

> [!summary] In short
> Full statistics of Exp 2 (persona, masking, representation, aggregator bottleneck, medical safety).

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Creativity and Exploration]], [[Multi-Agent Debate]], [[Evaluation and Metrics]]  
**Part of:** [[Creativity Exp 2 - Conclusion]]  
**Original:** [Notion page](https://app.notion.com/p/3c2d68da32f08010bbf2db70b601d597)

---

## Experiment 2 Results — Asymmetry Levers & the Aggregation Bottleneck
Design and pre-registered questions: [`../README.md`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/README.md). 48 cells, 336 subagent calls, 0 failures. Same 8 items and seed as Experiment 1.

### Headline
Composite creativity: **E3 persona (+0.40) > E5 persona+masked (+0.32) > E2 masked (+0.28) > E4 representation (+0.28) > E1 identical debate (-0.23) > E0 single agent (-1.05)**

| lever | round-1 diversity (text) | (idea-level) | rare ideas | **usable novelty** | judge novelty | judge usefulness |
|---|---|---|---|---|---|---|
| E0 single agent | - | - | 10.6 | **5.88** | 3.00 | 2.75 |
| E1 identical debate | 0.089 | 0.823 | 9.2 | **4.88** | 3.75 | 2.88 |
| E2 masked | 0.110 | 0.881 | 10.1 | **6.12** | 3.88 | 3.00 |
| E3 persona | 0.128 | 0.880 | 14.5 | **7.88** | 4.00 | 2.62 |
| E4 representation | 0.084 | 0.799 | 9.1 | **5.00** | 3.62 | 3.00 |
| E5 persona+masked | 0.145 | 0.919 | 13.5 | **7.50** | 4.12 | 2.50 |

### Q1 — Was the aggregator the bottleneck? Yes.
Experiment 1's most striking result was that identical-context debate (C1) scored *below* the single-agent baseline (C0). Under the new pipeline that reverses: E1 (-0.23) now beats E0 (-1.05).
E0 is the only arm with no aggregator — it is one agent's raw list. Every arm that passes through the aggregator gained; the one that does not, did not. Comparing the same conditions on the same items across pipelines:

| condition | judge novelty | judge flexibility | judge usefulness | flag rate /idea |
|---|---|---|---|---|
| identical context | 3.00 -> 3.75 (+0.75) | 8.1 -> 13.2 (+5.1) | 3.38 -> 2.88 (-0.50) | 0.20 -> 0.38 |
| masked | 3.25 -> 3.88 (+0.62) | 8.0 -> 11.8 (+3.8) | 3.12 -> 3.00 (-0.12) | 0.24 -> 0.31 |

Unshackling the output stage bought a large gain in **flexibility** (distinct mechanisms represented: +5.1 categories for identical-context debate) and a solid gain in **novelty**, at a small cost in usefulness and a modestly higher per-idea flag rate. This is a bigger movement than any context manipulation produced in Experiment 1.
**Caveat:** this contrast is across runs, so it carries run-to-run sampling noise as well as the pipeline change. It is indicative.

![[creativity-exp-2-full-results-1.webp]]

### Q2 — Can asymmetry be induced without withholding information? Yes — persona.

| condition | n | diversity_text_r1 | diversity_idea_r1 |
|---|---|---|---|
| E1 (identical debate) | 8 | 0.09 [0.08, 0.10] | 0.82 [0.77, 0.87] |
| E2 (masked) | 8 | 0.11 [0.09, 0.13] | 0.88 [0.83, 0.93] |
| E3 (persona) | 8 | 0.13 [0.11, 0.15] | 0.88 [0.83, 0.92] |
| E4 (representation) | 8 | 0.08 [0.07, 0.11] | 0.80 [0.73, 0.86] |
| E5 (persona+masked) | 8 | 0.14 [0.12, 0.17] | 0.92 [0.87, 0.96] |

| contrast | lever | n | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| E2 vs E1 | masking | 8 | +0.02 [+0.00, +0.04] | +0.47 (medium) | 0.078 |
| E3 vs E1 | persona | 8 | +0.04 [+0.02, +0.06] | +0.78 (large) | 0.008 |
| E4 vs E1 | representation | 8 | -0.00 [-0.02, +0.01] | -0.31 (small) | 0.250 |
| E5 vs E1 | persona+masked | 8 | +0.06 [+0.03, +0.08] | +0.88 (large) | 0.008 |
| E3 vs E2 | persona vs masking | 8 | +0.02 [-0.01, +0.05] | +0.34 (medium) | 0.547 |
| E5 vs E3 | does masking add to persona? | 8 | +0.02 [-0.02, +0.05] | +0.25 (small) | 0.461 |

**Persona (E3) produced more agent diversity than masking (E2)** (0.128 vs 0.110 text-level) while every agent still saw the complete context. It also produced the most rare ideas of any single lever (14.5 vs 9.25 for identical context, +57%) and the highest usable novelty (7.88 vs 4.88, +62%).
**Representation (E4) is a null.** Prose vs numbered list vs thematically grouped bullets moved diversity to 0.084 — *below* the identical-context baseline of 0.089. This is the second surface-level manipulation to fail, after fact-order shuffling in Experiment 1.

![[creativity-exp-2-full-results-2.webp]]

### Q3 — Do the levers compose? Partly, and with a cost.
E5 (persona + masking) has the highest diversity of any arm on both metrics (0.145 text, 0.919 idea) and the highest judged novelty (4.13). But it also has the **lowest usefulness (2.50) and the most flagged implausible ideas (7.9)**, and its composite (0.32) sits *below* persona alone (0.40). Stacking asymmetries overshoots.

| contrast | lever | n | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| E2 vs E1 | masking | 8 | +0.51 [-0.69, +1.68] | +0.00 (negligible) | 0.477 |
| E3 vs E1 | persona | 8 | +0.64 [-0.37, +2.09] | +0.25 (small) | 0.750 |
| E4 vs E1 | representation | 8 | +0.51 [-0.87, +1.75] | +0.14 (negligible) | 0.438 |
| E5 vs E1 | persona+masked | 8 | +0.56 [-0.78, +1.81] | +0.22 (small) | 0.688 |
| E1 vs E0 | debate vs solo | 8 | +0.82 [-1.18, +2.39] | +0.42 (medium) | 0.297 |
| E3 vs E2 | persona vs masking | 8 | +0.12 [-1.16, +1.32] | +0.11 (negligible) | 1.000 |
| E5 vs E3 | does masking add to persona? | 8 | -0.08 [-1.15, +0.99] | -0.08 (negligible) | 0.844 |

#### Usable novelty — the metric that refuses to reward junk

| contrast | lever | n | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| E2 vs E1 | masking | 8 | +1.25 [-0.88, +3.75] | +0.11 (negligible) | 0.562 |
| E3 vs E1 | persona | 8 | +3.00 [+0.50, +6.12] | +0.28 (small) | 0.125 |
| E4 vs E1 | representation | 8 | +0.12 [-2.62, +3.12] | -0.02 (negligible) | 1.000 |
| E5 vs E1 | persona+masked | 8 | +2.62 [+0.12, +5.25] | +0.28 (small) | 0.188 |
| E1 vs E0 | debate vs solo | 8 | -1.00 [-4.12, +1.75] | -0.03 (negligible) | 0.625 |
| E3 vs E2 | persona vs masking | 8 | +1.75 [+0.38, +3.50] | +0.12 (negligible) | 0.094 |
| E5 vs E3 | does masking add to persona? | 8 | -0.38 [-2.12, +1.25] | -0.03 (negligible) | 0.875 |

![[creativity-exp-2-full-results-3.webp]]

### Q4 — Was the null mechanism a metric artifact? No.

| diversity metric | vs composite | vs usable novelty | vs judge novelty |
|---|---|---|---|
| text-level cosine | +0.13 (p=0.420) | +0.42 (p=0.008) | +0.14 (p=0.386) |
| idea-level Jaccard | +0.10 (p=0.547) | +0.40 (p=0.011) | +0.34 (p=0.034) |

Switching to the idea-level metric did **not** rescue the mechanism. Diversity still fails to predict composite creativity within datasets. It does show a positive association with *usable novelty*, which is the one place the asymmetry story survives — more divergent agents do surface more rare-but-defensible ideas — but it does not translate into the composite, because usefulness moves the other way.

### Medical safety — the originality-first aggregator is dangerous

| condition | flagged_per_run | ideas | flag_rate_per_idea | gold_recall | mean_gold_rank |
|---|---|---|---|---|---|
| E0 | 11.75 | 17.5 | 0.671 | 1.0 | 1.0 |
| E1 | 9.5 | 16.75 | 0.567 | 1.0 | 10.0 |
| E2 | 9.0 | 18.0 | 0.5 | 1.0 | 15.5 |
| E3 | 9.25 | 18.5 | 0.5 | 0.5 | 10.0 |
| E4 | 10.75 | 16.75 | 0.642 | 1.0 | 10.0 |
| E5 | 10.75 | 18.5 | 0.581 | 1.0 | 13.0 |

**This is the most important result in the experiment, and it is a negative one.**

| pipeline | mean rank of the correct diagnosis | recall |
|---|---|---|
| Experiment 1 (cap 12, plausibility-first) | **1.2** | 1.00 |
| Experiment 2 (cap 20, originality-first) | **9.9** | 0.92 |

Ranking by originality instead of plausibility pushed the correct diagnosis from the top of the list into the middle of it, and in one cell out of the list altogether. On `med_ddx_002` (Bartter syndrome) condition E3 scored **0.00 recall** — the gold diagnosis appears nowhere in its 20 ideas.
That failure is not the debaters' fault. Inspecting their round-2 outputs directly: **all three E3 agents proposed Bartter syndrome, and two of them ranked it #1.** The originality-first aggregator received the right answer three times over and discarded it, because a correct textbook diagnosis is by construction not original. The list it produced instead led with Munchausen by proxy, laxative abuse and heavy-metal poisoning.
Flag rates tell a milder version of the same story. Ranked cleanest to least clean: E3 0.500, E2 0.500, E1 0.567, E5 0.581, E4 0.642, E0 0.671. Masking (E2) is the cleanest lever; the single-agent baseline is the least clean.

### Per-dataset

#### gen_aut

| condition | n | creativity_composite | usable_novelty | judge_novelty | judge_usefulness | diversity_idea_r1 |
|---|---|---|---|---|---|---|
| E0 (single agent) | 2 | -1.89 [-2.90, -0.88] | 10.00 [9.00, 11.00] | 2.50 [2.00, 3.00] | 3.00 [3.00, 3.00] | n/a |
| E1 (identical debate) | 2 | 0.18 [-0.88, 1.25] | 7.00 [3.00, 11.00] | 3.00 [3.00, 3.00] | 3.50 [3.00, 4.00] | 0.86 [0.80, 0.92] |
| E2 (masked) | 2 | 0.18 [-0.88, 1.25] | 10.50 [10.00, 11.00] | 3.00 [3.00, 3.00] | 3.50 [3.00, 4.00] | 0.95 [0.92, 0.98] |
| E3 (persona) | 2 | 0.14 [-0.88, 1.15] | 10.00 [10.00, 10.00] | 3.50 [3.00, 4.00] | 2.50 [2.00, 3.00] | 0.92 [0.91, 0.93] |
| E4 (representation) | 2 | 1.25 [1.25, 1.25] | 10.00 [9.00, 11.00] | 3.00 [3.00, 3.00] | 4.00 [4.00, 4.00] | 0.87 [0.84, 0.90] |
| E5 (persona+masked) | 2 | 0.14 [-0.88, 1.15] | 9.00 [8.00, 10.00] | 3.50 [3.00, 4.00] | 2.50 [2.00, 3.00] | 0.95 [0.92, 0.99] |

#### gen_problems

| condition | n | creativity_composite | usable_novelty | judge_novelty | judge_usefulness | diversity_idea_r1 |
|---|---|---|---|---|---|---|
| E0 (single agent) | 2 | -1.56 [-2.58, -0.55] | 13.50 [11.00, 16.00] | 2.00 [2.00, 2.00] | 3.50 [3.00, 4.00] | n/a |
| E1 (identical debate) | 2 | 0.75 [-0.27, 1.76] | 10.00 [7.00, 13.00] | 4.00 [4.00, 4.00] | 3.50 [3.00, 4.00] | 0.91 [0.90, 0.91] |
| E2 (masked) | 2 | 0.75 [-1.42, 2.92] | 11.50 [11.00, 12.00] | 4.00 [3.00, 5.00] | 3.50 [3.00, 4.00] | 0.92 [0.92, 0.93] |
| E3 (persona) | 2 | 0.75 [-0.27, 1.76] | 16.00 [13.00, 19.00] | 4.00 [4.00, 4.00] | 3.50 [3.00, 4.00] | 0.96 [0.96, 0.96] |
| E4 (representation) | 2 | -0.41 [-1.42, 0.61] | 10.00 [8.00, 12.00] | 3.00 [3.00, 3.00] | 3.50 [3.00, 4.00] | 0.87 [0.86, 0.88] |
| E5 (persona+masked) | 2 | -0.27 [-0.27, -0.27] | 15.00 [14.00, 16.00] | 4.00 [4.00, 4.00] | 3.00 [3.00, 3.00] | 0.98 [0.97, 0.99] |

#### med_ddx

| condition | n | creativity_composite | usable_novelty | judge_novelty | judge_usefulness | diversity_idea_r1 |
|---|---|---|---|---|---|---|
| E0 (single agent) | 2 | 0.29 [-1.27, 1.85] | 0.00 [0.00, 0.00] | 4.00 [3.00, 5.00] | 2.00 [2.00, 2.00] | n/a |
| E1 (identical debate) | 2 | -1.52 [-3.33, 0.29] | 0.00 [0.00, 0.00] | 4.00 [4.00, 4.00] | 2.00 [2.00, 2.00] | 0.76 [0.72, 0.80] |
| E2 (masked) | 2 | -0.85 [-1.27, -0.43] | 0.00 [0.00, 0.00] | 3.50 [3.00, 4.00] | 2.50 [2.00, 3.00] | 0.77 [0.77, 0.77] |
| E3 (persona) | 2 | 1.07 [0.29, 1.85] | 0.50 [0.00, 1.00] | 4.50 [4.00, 5.00] | 2.00 [2.00, 2.00] | 0.80 [0.78, 0.82] |
| E4 (representation) | 2 | -0.07 [-0.43, 0.29] | 0.00 [0.00, 0.00] | 4.00 [4.00, 4.00] | 2.00 [2.00, 2.00] | 0.63 [0.60, 0.66] |
| E5 (persona+masked) | 2 | 1.07 [0.29, 1.85] | 0.00 [0.00, 0.00] | 4.50 [4.00, 5.00] | 2.00 [2.00, 2.00] | 0.83 [0.81, 0.86] |

#### med_repurpose

| condition | n | creativity_composite | usable_novelty | judge_novelty | judge_usefulness | diversity_idea_r1 |
|---|---|---|---|---|---|---|
| E0 (single agent) | 2 | -1.04 [-1.73, -0.35] | 0.00 [0.00, 0.00] | 3.50 [3.00, 4.00] | 2.50 [2.00, 3.00] | n/a |
| E1 (identical debate) | 2 | -0.35 [-0.35, -0.35] | 2.50 [0.00, 5.00] | 4.00 [4.00, 4.00] | 2.50 [2.00, 3.00] | 0.77 [0.75, 0.78] |
| E2 (masked) | 2 | 1.04 [1.04, 1.04] | 2.50 [2.00, 3.00] | 5.00 [5.00, 5.00] | 2.50 [2.00, 3.00] | 0.88 [0.86, 0.89] |
| E3 (persona) | 2 | -0.35 [-1.73, 1.04] | 5.00 [4.00, 6.00] | 4.00 [3.00, 5.00] | 2.50 [2.00, 3.00] | 0.84 [0.83, 0.86] |
| E4 (representation) | 2 | 0.35 [-0.35, 1.04] | 0.00 [0.00, 0.00] | 4.50 [4.00, 5.00] | 2.50 [2.00, 3.00] | 0.83 [0.81, 0.84] |
| E5 (persona+masked) | 2 | 0.35 [-0.35, 1.04] | 6.00 [5.00, 7.00] | 4.50 [4.00, 5.00] | 2.50 [2.00, 3.00] | 0.91 [0.84, 0.97] |

### All cells

| dataset | item | condition | fluency | n_rare_ideas | usable_novelty | diversity_text_r1 | diversity_idea_r1 | judge_novelty | judge_usefulness | n_flagged_implausible | creativity_composite |
|---|---|---|---|---|---|---|---|---|---|---|---|
| gen_aut | gen_aut_001 | E0 | 20 | 12 | 9 | nan | nan | 2 | 3 | 3 | -2.905 |
| gen_aut | gen_aut_001 | E1 | 20 | 8 | 3 | 0.09 | 0.799 | 3 | 3 | 5 | -0.876 |
| gen_aut | gen_aut_001 | E2 | 18 | 12 | 11 | 0.163 | 0.916 | 3 | 4 | 1 | 1.245 |
| gen_aut | gen_aut_001 | E3 | 19 | 14 | 10 | 0.113 | 0.925 | 3 | 3 | 4 | -0.876 |
| gen_aut | gen_aut_001 | E4 | 19 | 13 | 11 | 0.086 | 0.836 | 3 | 4 | 2 | 1.245 |
| gen_aut | gen_aut_001 | E5 | 20 | 14 | 10 | 0.124 | 0.916 | 4 | 3 | 4 | 1.152 |
| gen_aut | gen_aut_002 | E0 | 20 | 15 | 11 | nan | nan | 3 | 3 | 4 | -0.876 |
| gen_aut | gen_aut_002 | E1 | 19 | 14 | 11 | 0.125 | 0.916 | 3 | 4 | 3 | 1.245 |
| gen_aut | gen_aut_002 | E2 | 20 | 14 | 10 | 0.138 | 0.981 | 3 | 3 | 4 | -0.876 |
| gen_aut | gen_aut_002 | E3 | 20 | 17 | 10 | 0.136 | 0.907 | 4 | 2 | 7 | 1.152 |
| gen_aut | gen_aut_002 | E4 | 20 | 11 | 9 | 0.153 | 0.899 | 3 | 4 | 2 | 1.245 |
| gen_aut | gen_aut_002 | E5 | 19 | 17 | 8 | 0.23 | 0.991 | 3 | 2 | 9 | -0.876 |
| gen_problems | gen_problems_001 | E0 | 20 | 15 | 11 | nan | nan | 2 | 3 | 4 | -2.577 |
| gen_problems | gen_problems_001 | E1 | 20 | 15 | 13 | 0.1 | 0.908 | 4 | 4 | 2 | 1.761 |
| gen_problems | gen_problems_001 | E2 | 20 | 13 | 11 | 0.073 | 0.931 | 5 | 4 | 2 | 2.915 |
| gen_problems | gen_problems_001 | E3 | 19 | 16 | 13 | 0.134 | 0.964 | 4 | 4 | 3 | 1.761 |
| gen_problems | gen_problems_001 | E4 | 18 | 14 | 8 | 0.084 | 0.883 | 3 | 3 | 6 | -1.423 |
| gen_problems | gen_problems_001 | E5 | 20 | 19 | 16 | 0.146 | 0.991 | 4 | 3 | 3 | -0.268 |
| gen_problems | gen_problems_002 | E0 | 20 | 17 | 16 | nan | nan | 2 | 4 | 1 | -0.549 |
| gen_problems | gen_problems_002 | E1 | 20 | 14 | 7 | 0.085 | 0.904 | 4 | 3 | 7 | -0.268 |
| gen_problems | gen_problems_002 | E2 | 20 | 15 | 12 | 0.098 | 0.918 | 3 | 3 | 3 | -1.423 |
| gen_problems | gen_problems_002 | E3 | 29 | 24 | 19 | 0.193 | 0.965 | 4 | 3 | 5 | -0.268 |
| gen_problems | gen_problems_002 | E4 | 19 | 15 | 12 | 0.077 | 0.857 | 3 | 4 | 3 | 0.606 |
| gen_problems | gen_problems_002 | E5 | 20 | 18 | 14 | 0.11 | 0.974 | 4 | 3 | 4 | -0.268 |
| med_ddx | med_ddx_001 | E0 | 19 | 5 | 0 | nan | nan | 5 | 2 | 11 | 1.854 |
| med_ddx | med_ddx_001 | E1 | 14 | 0 | 0 | 0.055 | 0.724 | 4 | 2 | 9 | -3.326 |
| med_ddx | med_ddx_001 | E2 | 16 | 2 | 0 | 0.103 | 0.775 | 4 | 3 | 8 | -0.432 |
| med_ddx | med_ddx_001 | E3 | 19 | 10 | 0 | 0.123 | 0.778 | 5 | 2 | 13 | 1.854 |
| med_ddx | med_ddx_001 | E4 | 18 | 4 | 0 | 0.056 | 0.605 | 4 | 2 | 13 | 0.292 |
| med_ddx | med_ddx_001 | E5 | 18 | 6 | 0 | 0.152 | 0.861 | 4 | 2 | 13 | 0.292 |
| med_ddx | med_ddx_002 | E0 | 14 | 8 | 0 | nan | nan | 3 | 2 | 15 | -1.27 |
| med_ddx | med_ddx_002 | E1 | 16 | 4 | 0 | 0.098 | 0.797 | 4 | 2 | 15 | 0.292 |
| med_ddx | med_ddx_002 | E2 | 17 | 8 | 0 | 0.108 | 0.772 | 3 | 2 | 16 | -1.27 |
| med_ddx | med_ddx_002 | E3 | 16 | 8 | 1 | 0.106 | 0.82 | 4 | 2 | 7 | 0.292 |
| med_ddx | med_ddx_002 | E4 | 13 | 2 | 0 | 0.068 | 0.659 | 4 | 2 | 14 | -0.432 |
| med_ddx | med_ddx_002 | E5 | 16 | 7 | 0 | 0.133 | 0.808 | 5 | 2 | 15 | 1.854 |
| med_repurpose | med_repurpose_001 | E0 | 18 | 6 | 0 | nan | nan | 4 | 3 | 6 | -0.346 |
| med_repurpose | med_repurpose_001 | E1 | 19 | 6 | 0 | 0.074 | 0.753 | 4 | 3 | 6 | -0.346 |
| med_repurpose | med_repurpose_001 | E2 | 20 | 7 | 2 | 0.107 | 0.893 | 5 | 3 | 5 | 1.039 |
| med_repurpose | med_repurpose_001 | E3 | 20 | 13 | 4 | 0.086 | 0.827 | 5 | 2 | 9 | 1.039 |
| med_repurpose | med_repurpose_001 | E4 | 19 | 7 | 0 | 0.064 | 0.809 | 5 | 3 | 8 | 1.039 |
| med_repurpose | med_repurpose_001 | E5 | 20 | 13 | 7 | 0.165 | 0.972 | 5 | 3 | 6 | 1.039 |
| med_repurpose | med_repurpose_002 | E0 | 19 | 7 | 0 | nan | nan | 3 | 2 | 15 | -1.732 |
| med_repurpose | med_repurpose_002 | E1 | 18 | 13 | 5 | 0.084 | 0.781 | 4 | 2 | 8 | -0.346 |
| med_repurpose | med_repurpose_002 | E2 | 19 | 10 | 3 | 0.087 | 0.862 | 5 | 2 | 7 | 1.039 |
| med_repurpose | med_repurpose_002 | E3 | 19 | 14 | 6 | 0.134 | 0.856 | 3 | 3 | 8 | -1.732 |
| med_repurpose | med_repurpose_002 | E4 | 17 | 7 | 0 | 0.084 | 0.841 | 4 | 2 | 8 | -0.346 |
| med_repurpose | med_repurpose_002 | E5 | 20 | 14 | 5 | 0.099 | 0.841 | 4 | 2 | 9 | -0.346 |

### Limitations
- **1 repeat per cell.** Unchanged from Experiment 1: run-to-run variance is still confounded with condition effect. This is a screening run.
- **The pipeline contrast is across runs**, not a within-run factor.
- Single Sonnet judge, same model family as the debaters; no clinician review; 2 of 4 datasets synthetic; `med_ddx` recall at ceiling with 2 items.
- The judge saw longer lists in Exp 2 (20 vs 12 ideas). Absolute flag *counts* are therefore not comparable across experiments; flag *rates* are.
