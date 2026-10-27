---
type: report
date: 2026-08-20
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Creativity and Exploration", "Multi-Agent Debate", "Evaluation and Metrics"]
tags: [type/report, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/3c2d68da32f08011b980ea9c586ea88b
created: 2026-08-20
updated: 2026-08-20
---
# Creativity Exp 1 - Index and Conclusion

> [!summary] In short
> Index of both creativity experiments and the full conclusion of Exp 1 (masking and shuffling of context between debating agents).

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Creativity and Exploration]], [[Multi-Agent Debate]], [[Evaluation and Metrics]]  
**From meeting:** [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]]  
**Sub-notes:** [[Creativity Exp 1 - Full Results]]  
**Original:** [Notion page](https://app.notion.com/p/3c2d68da32f08011b980ea9c586ea88b)

---

## Creativity in Multi-Agent Debate — Experiment Index
Two completed experiments on the same 8 items (4 datasets × 2), same seed 42, same models (Haiku debaters, blinded Sonnet judge), same 3-agent / 1-debate-round structure. Each is self-contained and its design was frozen before any model call.

| # | Experiment | Question | Location | Status |
|---|---|---|---|---|
| 1 | Masking & shuffling | Does information **asymmetry** or **ordering** raise creativity? | [`../creativity-experiment/`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/) | complete — 40 cells, 278 calls |
| 2 | Asymmetry levers & the aggregation bottleneck | Can asymmetry be induced **without withholding information**? Was the **aggregator** throttling output? | [`exp2-asymmetry-levers/`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/) | complete — 48 cells, 336 calls |

### Read in this order
1. [Exp 1 conclusion](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/CONCLUSION.md) — the interpretive bottom line
1. [Exp 1 report](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/report.md) — full statistics
1. [Exp 2 design](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/README.md) — what Exp 1 forced us to test next
1. [Exp 2 conclusion](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/CONCLUSION2.md) — **start here if reading only one file**
1. [Exp 2 report](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/results/report2.md) — full statistics

Supporting: [Exp 1 verbatim samples](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/SAMPLES.md), [compute cost](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/COST.md) (⚠ orchestrator figures in that file are inflated ~18×; subagent figures are correct).

### What is settled after both runs
**Levers that move agent diversity**

| Lever | Exp | Effect on round-1 diversity | Verdict |
|---|---|---|---|
| Persona / reasoning stance | 2 | 0.089 → **0.128** | best lever found |
| Fact masking (50%, union-covering) | 1, 2 | 0.089 → 0.110 | works; safest on flags |
| Persona + masking | 2 | 0.089 → **0.145** | most diverse, but overshoots on usefulness |
| Fact-order shuffling | 1 | 0.116 → 0.111 | **null** |
| Representation (prose/table/list) | 2 | 0.089 → 0.084 | **null** |

**The generalisation.** Surface perturbations of context — order, format — do nothing; these models are invariant to both. Only changing *what* an agent knows or *how it is instructed to reason* produces divergence. Two independent nulls support this, which retires a whole family of candidate interventions.
**The mechanism, narrowed.** Agent diversity does **not** predict composite creativity (ρ ≈ +0.1, n.s., both metrics). It **does** predict *usable novelty* — rare ideas minus judge-flagged ones — at ρ ≈ +0.4, p ≈ .01. That is the only clear correlation in either experiment.
**The aggregator matters more than the context.** Rewriting the merge step (cap 12 → 20, plausibility-first → originality-first) moved judged flexibility from 8.1 to 13.2 categories — a bigger change than any context manipulation in Exp 1, and enough to reverse Exp 1's headline that debate underperforms a single agent.
**⚠ And it is dangerous.** The same edit moved the correct diagnosis from mean rank 1.2 to 9.9, and lost it entirely in one cell *after all three debaters had proposed it and two had ranked it first*. Creativity gain and clinical failure came from the same change. Any deployment must emit a plausibility-ranked answer list and a speculative list separately, never one ranking serving both.

### What is still open
Both experiments ran at **1 repeat per cell**, so run-to-run variance remains confounded with condition effect throughout. Everything above is screening-grade. The next run should be narrow and properly powered: persona vs identical context, ≥3 repeats, dual-output aggregator, with gold rank as a primary outcome.

## Conclusion
**Experiment:** Does random context **masking** or context **shuffling** across debating agents increase the creativity of multi-agent debate output?
**Run:** pilot tier, all 5 conditions, 4 datasets × 2 items × 1 repeat = 40 cells, 278 subagent invocations, 0 failed cells. Debaters Haiku, blinded judge Sonnet, seed 42.
Full numbers in [`results/report.md`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/report.md); verbatim run artifacts in [`results/SAMPLES.md`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/SAMPLES.md). This file is the interpretive bottom line.

---

### 1. The short answer
**Masking: yes, probably — but not for the reason the hypothesis proposed.** **Shuffling: no. It is a null manipulation as implemented.**

| Condition | Composite creativity | Round-1 agent diversity |
|---|---|---|
| C0 — single agent, full context | −0.34 | — |
| C1 — 3 agents, identical context | **−0.74** (worst) | 0.116 |
| C2 — masked (50%, union-covering) | +0.02 | 0.152 |
| C3 — shuffled | +0.27 | 0.111 |
| C4 — masked + shuffled | **+0.79** (best) | 0.155 |

C4 vs C1 is the only contrast whose bootstrap CI excludes zero (Cliff's d = +0.61, large, nominal p = .031).

---

### 2. What the experiment actually established

#### 2.1 Masking changes agent behaviour; shuffling does not
This is the cleanest result, and it is a manipulation-check finding rather than an outcome finding — which is why I trust it most.
Withholding half the facts from each agent raised round-1 inter-agent distance from 0.116 to 0.152 (**d = +0.53, large**; mean difference +0.036, CI [+0.003, +0.071]). Reordering the *same* facts moved it to 0.111 — **d = −0.09, negligible, and in the wrong direction**.
The interpretation is straightforward: these debaters are effectively order-invariant over a flat numbered fact list. Presented with `FACT 1..25` in a permuted order, the model reconstructs the same clinical picture. Shuffling only produces information asymmetry if the model is order-sensitive to begin with, and over a bulleted list it is not. **C3 is therefore not "the shuffling condition" — it is C1 re-run with different sampling noise.**

#### 2.2 Identical-context debate is actively harmful
C1 scored worst of all five conditions, below the single-agent baseline (d = −0.23 on composite), and it also had the *lowest* judged usefulness of any multi-agent arm relative to C0 (3.375 vs 3.875, d = −0.31).
Section 3 of `SAMPLES.md` shows what this looks like: three agents given identical context open with near-identical hypotheses, the debate round reinforces the consensus rather than challenging it, and the aggregator then compresses an already-homogeneous set into something narrower than what one agent produced alone.
This is the most consistent pattern in the data and, in my judgement, the most useful finding in the whole run.

#### 2.3 The system-level accuracy cost of masking was zero here
Gold-diagnosis recall on `med_ddx` was 1.00 in every condition including C2 and C4, at ranks 1–2. Union coverage plus one debate round was sufficient to reassemble the diagnosis from fragments — no masked agent's blind spot survived contact with its peers.
I would not lean on this. Two items is a ceiling, not a result.

---

### 3. What the experiment failed to establish — and this matters
**The proposed mechanism is not supported.**
The hypothesis was causal and specific: masking → more diverse agents → more creative output. The first link holds (§2.1). The second does not.

| Relationship (dataset-residualised) | Spearman ρ | p |
|---|---|---|
| round-1 diversity → composite creativity | −0.12 | .530 |
| round-1 diversity → judge novelty | +0.10 | .576 |
| round-1 diversity → judge usefulness | −0.32 | .076 |

Within datasets, agent diversity does not predict final creativity. And the decisive counter-evidence is C3: it gained a **medium**-sized composite advantage over C1 (d = +0.41) while producing **no diversity gain whatsoever**.
If a condition that demonstrably failed to diversify the agents nonetheless improved the outcome, then diversity is not what is driving the outcome.
Two candidate explanations, and the pilot cannot separate them:
- **(a) Symmetry-breaking of any kind helps.** What hurts C1 may simply be that its three agents are near-duplicates of one another. Any perturbation — masking, or in C3's case merely a different sampling trajectory — avoids that trap. Under this reading masking is *sufficient* but not *special*, and the mechanism is about escaping degenerate consensus rather than about information asymmetry.
- **(b) C3's gain is noise.** With 1 repeat per cell, run-to-run variance is completely confounded with condition effects. C3 may simply have drawn a lucky sample.

Distinguishing (a) from (b) is the single most important thing the next run must do, and neither requires more datasets — both require **repeats**.

---

### 4. The safety picture points the wrong way
Judge-flagged clinically implausible ideas, per medical run:

| C0 | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| 2.5 | 3.75 | 3.75 | 3.0 | **4.0** |

**The best-scoring condition on creativity also produces the most flagged content.** C4 flags 60% more implausible ideas than the single-agent baseline. Judged usefulness under masking alone (C2, 3.125) is the lowest of all five conditions.
This is exactly the degradation story the spec warned about, and it should not be buried under the composite score. Whatever C4 is buying, part of the price is paid in clinical defensibility. The flags themselves are real and specific — the judge caught geographic errors (Bartonella bacilliformis placed in Mexico), mechanism inversions (augmenting an anticoagulant state in a patient with recurrent haemorrhage), and drugs that do not exist (MAVS antagonists) — see `SAMPLES.md` §4.
For any medical deployment, the honest summary is: **masking made the idea pool broader and less reliable at the same time.**

---

### 5. Verdict on the design itself
**The apparatus works.** Masks cover the union exactly (840/840 fact-presence assertions verified correct). Shuffles are independent and deterministic under seed. Context isolation held across all 200 debater calls — 0 of 104 round-1 prompts contained peer material, and no agent ever saw another's context. The judge saw no condition metadata. Zero cells failed. The grid is idempotent and resumable.
**The statistics are underpowered by construction.** 8 items × 1 repeat. Wilcoxon on n ≤ 8 pairs cannot reach p &lt; .05 for most real effects regardless of magnitude, so effect sizes and CIs carry the argument and the p-values are decoration.
**So: scale it — but not as-is.** Running `small` with the current design would mostly buy precision on the wrong quantities.

---

### 6. Recommendation
Before spending `small`/`full` quota, make four changes:
1. **Repeats to ≥ 3.** Non-negotiable. Everything ambiguous in this pilot — above all C3's unearned gain — is ambiguous because run variance and condition effect are the same number right now.
1. **Add a C1′ arm: identical context, different sampling seeds per agent.** This is the decisive experiment. If C1′ recovers most of C4's advantage, the finding is "break agent symmetry" and masking is one cheap way to do it. If C1′ stays at C1's level, masking is doing something specific and the asymmetry hypothesis survives. As it stands the data cannot tell these apart, and that ambiguity undercuts the headline.
1. **Drop C3, or redesign it.** Shuffling a numbered fact list is a null. If order effects are still of interest, shuffle something the model is demonstrably order-sensitive to — narrative prose, or evidence streams of differing salience where primacy/recency can bite. Otherwise reallocate those calls to repeats, which is where they are worth more.
1. **Raise or remove the 12-idea cap.** Raw fluency sits at ceiling in nearly every cell and contributes nothing; the cap also forces the aggregator to discard exactly the long-tail ideas the study is trying to detect.

Two smaller ones: report judged usefulness and flag counts *beside* the composite everywhere rather than in a separate section, since the composite alone flatters C4; and add a second judge (or a different model family) before drawing conclusions about novelty, because generator and judge currently share a lineage.

---

### 7. One-paragraph summary
Masking half the context from each debater, with union coverage across the team, produced the highest-creativity outputs in this pilot and did so without losing the gold diagnosis. But the mechanism proposed for *why* it works does not survive contact with the data: agent diversity, the supposed mediator, does not predict creativity within datasets, and a condition that produced no diversity gain improved the outcome anyway. The most defensible reading is not "asymmetry makes agents creative" but "**three agents given identical context collapse into consensus and produce worse output than one agent alone, and almost any perturbation avoids that collapse**" — with masking being one effective perturbation that also carries a measurable cost in clinical plausibility. That is a narrower and more interesting claim than the one the experiment set out to test, and it is directly testable with one more arm and three repeats.

---
*Generated from the pilot run of 2026-08-19. Config, exact call counts, per-cell scores, failed cells (none), and full limitations are in* [*`results/report.md`*](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/creativity-experiment/results/report.md)*.*
