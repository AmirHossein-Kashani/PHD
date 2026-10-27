---
type: report
date: 2026-08-20
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Creativity and Exploration", "Multi-Agent Debate", "Evaluation and Metrics"]
tags: [type/report, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/3c2d68da32f08000a548fe3b1dac379e
created: 2026-08-20
updated: 2026-08-20
---
# Creativity Exp 1 - Full Results

> [!summary] In short
> Full statistics of Exp 1. Masking raised agent diversity; shuffling did nothing; plain debate was worse than one agent.

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Creativity and Exploration]], [[Multi-Agent Debate]], [[Evaluation and Metrics]]  
**Part of:** [[Creativity Exp 1 - Index and Conclusion]]  
**Original:** [Notion page](https://app.notion.com/p/3c2d68da32f08000a548fe3b1dac379e)

---

## Context Masking & Shuffling in Multi-Agent Debate — Pilot Report
Research question: does information asymmetry (**masking**) or information ordering (**shuffling**) across debating agents increase the creativity of the debate's output, relative to agents that all see identical, fully-ordered context?

### Headline
Composite creativity ranking (higher = better): **C4 (+0.79) > C3 (+0.27) > C2 (+0.02) > C0 (-0.34) > C1 (-0.74)**
- **Masking manipulation: WORKED.** Round-1 inter-agent diversity C2=0.152, C4=0.155 vs C1=0.116.
- **Shuffling manipulation: FAILED.** C3=0.111 vs C1=0.116 — reordering the same facts did NOT produce more divergent round-1 agents.
- **Mechanism: NOT SUPPORTED.** Within datasets, round-1 diversity does not predict final creativity, and C3 gained composite creativity while producing no diversity gain - so the C4 advantage is not (yet) attributable to information asymmetry. See Sections 6 and 8.
- **Safety: the cost is visible.** Judge-flagged implausible ideas per medical run are highest in C4 (4.0) and lowest in the single-agent baseline C0 (2.5).

### 1. Configuration actually used

```yaml
# config.yaml — creativity experiment
budget_tier: pilot          # pilot | small | full
n_agents: 3                 # debaters per multi-agent condition
mask_rate: 0.5              # fraction of facts each masked agent sees
union_coverage: true        # masks must jointly cover 100% of facts
debate_rounds: 1            # rounds AFTER round-1 independent outputs
items_per_dataset:
  pilot: 2
  small: 5
  full: 10
repeats:
  pilot: 1
  small: 3
  full: 5
# DEVIATION FROM SPEC DEFAULT: pilot runs all 5 conditions, not [C0,C1,C2].
# Rationale: the research question is about masking AND shuffling; trimming to
# C0/C1/C2 would yield zero data on shuffling (C3) and the combination (C4).
# Cost: 232 generation + 40 judge calls vs the ~145 in the Section-8 pilot row.
conditions: [C0, C1, C2, C3, C4]
random_seed: 42
debater_model: haiku
judge_model: sonnet
```

### 2. Execution accounting

| round1_debater_outputs | round1_retries_agent_did_not_write_file | round1_integrity_probe_calls | round2_debater_outputs | round2_retries | aggregator_outputs | aggregator_retries_concurrency_cap | judge_outputs | judge_retries_malformed_json | case_writer_calls | total_subagent_invocations | failed_cells | debater_model | judge_model |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 104 | 3 | 1 | 96 | 0 | 32 | 1 | 40 | 0 | 1 | 278 | 0 | haiku | sonnet |

### 3. Manipulation check (round-1 inter-agent diversity)
Mean pairwise cosine distance between the three agents' *independent* round-1 outputs, before any debate. This measures whether the intervention actually made the agents think differently.

| condition | n | interagent_diversity_r1 |
|---|---|---|
| C1 | 8 | 0.12 [0.08, 0.16] |
| C2 | 8 | 0.15 [0.12, 0.18] |
| C3 | 8 | 0.11 [0.08, 0.14] |
| C4 | 8 | 0.15 [0.12, 0.20] |

| contrast | meaning | n_items | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| C2 vs C1 | masking vs plain debate | 8 | +0.036 [+0.003, +0.071] | +0.53 (large) | 0.148 |
| C3 vs C1 | shuffling vs plain debate | 8 | -0.005 [-0.035, +0.021] | -0.09 (negligible) | 0.641 |
| C4 vs C1 | masking+shuffling vs plain debate | 8 | +0.039 [-0.007, +0.094] | +0.56 (large) | 0.312 |
| C4 vs C2 | adding shuffling on top of masking | 8 | +0.003 [-0.022, +0.032] | -0.12 (negligible) | 0.945 |
| C4 vs C3 | adding masking on top of shuffling | 8 | +0.044 [+0.002, +0.095] | +0.47 (medium) | 0.109 |

![[creativity-exp-1-full-results-1.webp]]

### 4. Pooled results (all 4 datasets, item-level pairing)

| condition | n | creativity_composite | judge_novelty | judge_usefulness | judge_flexibility | judge_elaboration | originality_rarity |
|---|---|---|---|---|---|---|---|
| C0 | 8 | -0.34 [-2.14, 1.20] | 2.88 [2.38, 3.38] | 3.88 [3.38, 4.38] | 7.25 [6.38, 8.12] | 3.38 [2.88, 3.88] | 0.74 [0.66, 0.79] |
| C1 | 8 | -0.74 [-1.47, 0.11] | 3.00 [2.25, 3.75] | 3.38 [2.75, 4.00] | 8.12 [7.38, 8.88] | 3.00 [3.00, 3.00] | 0.71 [0.58, 0.79] |
| C2 | 8 | 0.02 [-0.68, 0.60] | 3.25 [2.75, 3.75] | 3.12 [2.75, 3.50] | 8.00 [7.12, 9.00] | 3.12 [3.00, 3.38] | 0.76 [0.70, 0.80] |
| C3 | 8 | 0.27 [-0.53, 1.20] | 3.00 [2.38, 3.62] | 3.75 [3.25, 4.25] | 8.25 [7.12, 9.25] | 3.38 [3.12, 3.75] | 0.75 [0.68, 0.80] |
| C4 | 8 | 0.79 [0.24, 1.30] | 3.38 [2.75, 3.88] | 3.62 [3.25, 4.12] | 8.12 [7.12, 9.00] | 3.25 [3.00, 3.62] | 0.78 [0.76, 0.80] |

#### Key contrasts on composite creativity

| contrast | meaning | n_items | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| C2 vs C1 | masking vs plain debate | 8 | +0.758 [-0.397, +1.929] | +0.28 (small) | 0.219 |
| C3 vs C1 | shuffling vs plain debate | 8 | +1.001 [+0.306, +1.713] | +0.41 (medium) | 0.062 |
| C4 vs C1 | masking+shuffling vs plain debate | 8 | +1.523 [+0.667, +2.405] | +0.61 (large) | 0.031 |
| C1 vs C0 | debate vs single agent | 8 | -0.396 [-2.250, +1.616] | -0.23 (small) | 0.688 |
| C4 vs C0 | masking+shuffling vs single agent | 8 | +1.127 [-0.545, +3.045] | +0.17 (small) | 0.562 |
| C4 vs C2 | adding shuffling on top of masking | 8 | +0.766 [-0.300, +1.912] | +0.44 (medium) | 0.438 |
| C4 vs C3 | adding masking on top of shuffling | 8 | +0.522 [-0.625, +1.669] | +0.33 (small) | 0.625 |

#### Judge novelty

| contrast | meaning | n_items | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| C2 vs C1 | masking vs plain debate | 8 | +0.250 [-0.250, +0.625] | +0.12 (negligible) | 0.625 |
| C3 vs C1 | shuffling vs plain debate | 8 | +0.000 [-0.375, +0.375] | +0.00 (negligible) | 1.000 |
| C4 vs C1 | masking+shuffling vs plain debate | 8 | +0.375 [+0.000, +0.875] | +0.19 (small) | 0.500 |
| C1 vs C0 | debate vs single agent | 8 | +0.125 [-0.500, +0.750] | +0.06 (negligible) | 1.000 |
| C4 vs C0 | masking+shuffling vs single agent | 8 | +0.500 [+0.122, +1.000] | +0.33 (small) | 0.250 |
| C4 vs C2 | adding shuffling on top of masking | 8 | +0.125 [-0.250, +0.500] | +0.14 (negligible) | 1.000 |
| C4 vs C3 | adding masking on top of shuffling | 8 | +0.375 [+0.000, +0.875] | +0.23 (small) | 0.500 |

#### Judge usefulness (the cost side of the ledger)

| contrast | meaning | n_items | mean diff [95% CI] | Cliff's d | Wilcoxon p |
|---|---|---|---|---|---|
| C2 vs C1 | masking vs plain debate | 8 | -0.250 [-0.750, +0.250] | -0.14 (negligible) | 0.625 |
| C3 vs C1 | shuffling vs plain debate | 8 | +0.375 [+0.000, +0.750] | +0.27 (small) | 0.250 |
| C4 vs C1 | masking+shuffling vs plain debate | 8 | +0.250 [+0.000, +0.625] | +0.17 (small) | 0.500 |
| C1 vs C0 | debate vs single agent | 8 | -0.500 [-0.875, -0.125] | -0.31 (small) | 0.125 |
| C4 vs C0 | masking+shuffling vs single agent | 8 | -0.250 [-0.625, +0.000] | -0.17 (small) | 0.500 |
| C4 vs C2 | adding shuffling on top of masking | 8 | +0.500 [+0.125, +0.875] | +0.34 (medium) | 0.125 |
| C4 vs C3 | adding masking on top of shuffling | 8 | -0.125 [-0.375, +0.000] | -0.11 (negligible) | 1.000 |

![[creativity-exp-1-full-results-2.webp]]

### 5. Per-dataset breakdown

#### gen_aut

| condition | n | creativity_composite | judge_novelty | judge_usefulness | originality_rarity | fluency | interagent_diversity_r1 |
|---|---|---|---|---|---|---|---|
| C0 | 2 | 0.07 [-0.60, 0.74] | 2.00 [2.00, 2.00] | 4.50 [4.00, 5.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | n/a |
| C1 | 2 | -0.60 [-1.94, 0.74] | 2.00 [2.00, 2.00] | 4.00 [3.00, 5.00] | 0.80 [0.80, 0.80] | 11.50 [11.00, 12.00] | 0.18 [0.12, 0.24] |
| C2 | 2 | 0.40 [-0.60, 1.40] | 2.50 [2.00, 3.00] | 3.50 [3.00, 4.00] | 0.80 [0.80, 0.80] | 11.50 [11.00, 12.00] | 0.20 [0.18, 0.22] |
| C3 | 2 | 0.07 [-0.60, 0.74] | 2.00 [2.00, 2.00] | 4.50 [4.00, 5.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.17 [0.15, 0.18] |
| C4 | 2 | 0.07 [-0.60, 0.74] | 2.00 [2.00, 2.00] | 4.50 [4.00, 5.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.18 [0.16, 0.20] |

#### gen_problems

| condition | n | creativity_composite | judge_novelty | judge_usefulness | originality_rarity | fluency | interagent_diversity_r1 |
|---|---|---|---|---|---|---|---|
| C0 | 2 | 1.45 [0.33, 2.57] | 3.00 [3.00, 3.00] | 4.50 [4.00, 5.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | n/a |
| C1 | 2 | -1.33 [-1.33, -1.33] | 2.00 [2.00, 2.00] | 4.00 [4.00, 4.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.11 [0.11, 0.11] |
| C2 | 2 | -0.78 [-1.90, 0.33] | 3.00 [3.00, 3.00] | 3.50 [3.00, 4.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.13 [0.12, 0.14] |
| C3 | 2 | -0.50 [-1.33, 0.33] | 2.50 [2.00, 3.00] | 4.00 [4.00, 4.00] | 0.80 [0.80, 0.80] | 11.50 [11.00, 12.00] | 0.10 [0.10, 0.11] |
| C4 | 2 | 1.17 [0.33, 2.00] | 3.50 [3.00, 4.00] | 4.00 [4.00, 4.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.11 [0.08, 0.13] |

#### med_ddx

| condition | n | creativity_composite | judge_novelty | judge_usefulness | originality_rarity | fluency | interagent_diversity_r1 |
|---|---|---|---|---|---|---|---|
| C0 | 2 | -0.77 [-3.42, 1.89] | 3.00 [2.00, 4.00] | 3.00 [3.00, 3.00] | 0.63 [0.47, 0.80] | 8.00 [7.00, 9.00] | n/a |
| C1 | 2 | -0.41 [-1.78, 0.97] | 4.00 [4.00, 4.00] | 2.50 [2.00, 3.00] | 0.47 [0.27, 0.67] | 9.50 [8.00, 11.00] | 0.06 [0.05, 0.08] |
| C2 | 2 | -0.01 [-0.54, 0.51] | 3.50 [3.00, 4.00] | 3.00 [3.00, 3.00] | 0.63 [0.60, 0.67] | 9.00 [9.00, 9.00] | 0.17 [0.13, 0.20] |
| C3 | 2 | -0.24 [-1.00, 0.51] | 3.50 [3.00, 4.00] | 3.00 [3.00, 3.00] | 0.60 [0.60, 0.60] | 9.00 [9.00, 9.00] | 0.07 [0.05, 0.09] |
| C4 | 2 | 1.43 [1.43, 1.43] | 4.00 [4.00, 4.00] | 3.00 [3.00, 3.00] | 0.73 [0.73, 0.73] | 11.50 [11.00, 12.00] | 0.21 [0.13, 0.29] |

#### med_repurpose

| condition | n | creativity_composite | judge_novelty | judge_usefulness | originality_rarity | fluency | interagent_diversity_r1 |
|---|---|---|---|---|---|---|---|
| C0 | 2 | -2.11 [-5.03, 0.81] | 3.50 [3.00, 4.00] | 3.50 [3.00, 4.00] | 0.73 [0.73, 0.73] | 10.50 [9.00, 12.00] | n/a |
| C1 | 2 | -0.60 [-1.69, 0.49] | 4.00 [4.00, 4.00] | 3.00 [3.00, 3.00] | 0.77 [0.73, 0.80] | 11.00 [10.00, 12.00] | 0.11 [0.10, 0.12] |
| C2 | 2 | 0.49 [0.49, 0.49] | 4.00 [4.00, 4.00] | 2.50 [2.00, 3.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.12 [0.09, 0.14] |
| C3 | 2 | 1.74 [0.49, 2.99] | 4.00 [4.00, 4.00] | 3.50 [3.00, 4.00] | 0.80 [0.80, 0.80] | 12.00 [12.00, 12.00] | 0.11 [0.09, 0.12] |
| C4 | 2 | 0.49 [0.49, 0.49] | 4.00 [4.00, 4.00] | 3.00 [3.00, 3.00] | 0.80 [0.80, 0.80] | 11.50 [11.00, 12.00] | 0.12 [0.12, 0.13] |

### 6. Mechanism check
Across all 32 multi-agent runs:

| relationship | pooled Spearman rho | p | dataset-residualised rho | p |
|---|---|---|---|---|
| diversity vs composite creativity | -0.01 | 0.939 | **-0.12** | 0.530 |
| diversity vs judge novelty | -0.39 | 0.026 | **+0.10** | 0.576 |
| diversity vs judge usefulness | +0.14 | 0.443 | **-0.32** | 0.076 |

The pooled column is confounded: `gen_aut` runs have high inter-agent diversity but low judged novelty, and the medical runs the reverse, so pooling across datasets manufactures a spurious slope. The dataset-residualised column is the one to read.

![[creativity-exp-1-full-results-3.webp]]

### 7. Medical safety lens
Gold-diagnosis recall on `med_ddx` (2 real MedQA-USMLE vignettes: hemolytic uremic syndrome; Bartter syndrome):

| condition | recall | hits | items | mean_rank_when_found |
|---|---|---|---|---|
| C0 | 1.0 | 2.0 | 2 | 1.0 |
| C1 | 1.0 | 2.0 | 2 | 1.5 |
| C2 | 1.0 | 2.0 | 2 | 1.0 |
| C3 | 1.0 | 2.0 | 2 | 1.0 |
| C4 | 1.0 | 2.0 | 2 | 1.5 |

Judge-flagged clinically implausible ideas across both medical datasets:

| condition | mean flagged/run | total flagged |
|---|---|---|
| C0 | 2.5 | 10 |
| C1 | 3.75 | 15 |
| C2 | 3.75 | 15 |
| C3 | 3.0 | 12 |
| C4 | 4.0 | 16 |

![[creativity-exp-1-full-results-4.webp]]

### 8. Verdict on the design
**Answer to the research question, as far as a pilot can answer it:**
1. **Masking is a real manipulation; shuffling is not.** Withholding half the facts from each agent raised round-1 inter-agent diversity by ~31% (Cliff's d = +0.53 vs C1, large). Reordering the *same* facts changed nothing (d = -0.09, negligible) - these debaters are essentially order-invariant over a numbered fact list, so C3 amounts to a re-run of C1 with different sampling noise.
1. **Composite creativity ranks C4 > C3 > C2 > C0 > C1.** C4 vs C1 is the only contrast whose bootstrap CI excludes zero and whose nominal p &lt; .05 (d = +0.61, large, p = .031). C2 vs C1 is d = +0.28 (small); C3 vs C1 is d = +0.41 (medium).
1. **The proposed mechanism does not hold up.** If masking helped *because* it diversified the agents, round-1 diversity should predict final creativity. Within datasets it does not (Section 6). Worse, C3 gained a medium-sized composite advantage while producing **no** diversity gain at all. Diversity and creativity move independently in this data, so the C4 result is not yet explained by information asymmetry.
1. **Plain debate (C1) was the worst condition of all five, below the single-agent baseline** (d = -0.23). Three agents given identical context converge, and the aggregator then compresses an already-homogeneous set. Anything that breaks that symmetry - even ineffectively, as in C3 - beats it. This is the most consistent pattern in the data, and it reframes the headline: the story looks less like *masking creates creativity* and more like *identical-context debate destroys it, and masking avoids the destruction.*
1. **The safety cost is visible and points the wrong way.** Judge-flagged clinically implausible ideas per medical run: C0 2.5, C1 3.8, C2 3.8, C3 3.0, C4 4.0. The best-scoring condition also produces the most flagged content. Gold-diagnosis recall was 1.00 in every condition, but with 2 `med_ddx` items that is a ceiling, not evidence of safety.

**Recommendation before spending `small` / `full` quota:**
The design works *mechanically* - masks cover the union exactly, shuffles are independent and deterministic, context isolation held across all 200 debater calls (verified: 840/840 fact-presence assertions correct, no r1 prompt contained peer material), 0 cells failed, and the judge saw no condition metadata. It is safe to scale. But scale it with three changes, or the extra quota mostly buys noise:
- **Raise repeats to >= 3.** At 1 repeat per cell, run-to-run sampling variance is not separable from condition effects - the single most likely explanation for C3's unearned gain.
- **Drop or redesign C3.** Fact-order shuffling over a flat numbered list is a null manipulation here. Either shuffle something the model is demonstrably order-sensitive to (narrative prose, or interleaved evidence of differing salience), or spend those calls on repeats.
- **Add a C1-with-different-seeds arm.** That is the live confound: the data cannot currently distinguish *masking specifically* from *breaking agent symmetry by any means*.
- Also raise or remove the 12-idea cap: raw fluency sits at the ceiling in nearly every cell and contributes nothing to the composite.

### 9. Limitations
- **Tiny N.** 8 items x 1 repeat. Wilcoxon on n&lt;=8 pairs cannot reach p&lt;0.05 for most effects regardless of magnitude; p-values here are indicative only, and effect sizes/CIs carry the argument.
- **LLM judge bias.** A single Sonnet judge scored every output. Judges of this kind are known to reward surface novelty and fluent rationales. No human rater validated any score.
- **No clinician validation.** Medical plausibility and the implausible-idea flags are model judgements, not clinical review.
- **Single model family.** Debaters, aggregator, judge and the `med_repurpose` case-writer are all Claude models. Generator/judge correlation is a real confound; `med_repurpose` items are additionally synthetic and share a model family with the debaters.
- **Fluency ceiling.** The prompt caps output at 12 ideas, so fluency is near-saturated in every condition and cannot discriminate; only post-dedup fluency carries any signal.
- **Embedding backend substituted.** The spec asks for `sentence-transformers/all-MiniLM-L6-v2`, which was absent and would have required pulling a full torch install; a static distillation of the same family (`model2vec/potion-base-8M`) was used instead. It was spot-checked to separate paraphrases (cosine distance 0.11-0.31) from distinct ideas (0.75+), so the spec's 0.35 dedup threshold transfers. The TF-IDF fallback the spec permits was run first and produced degenerate, fully-saturated dedup and rarity metrics; those numbers were discarded rather than reported.
- **Subagent definitions.** `.claude/agents/*.md` were written as specified but the session could not be restarted, so each role prompt was passed verbatim in the delegation message instead. Context isolation -- the property the experiment actually depends on -- was preserved: every debater ran in a fresh subagent that read exactly one prompt file and saw no peer contexts.
