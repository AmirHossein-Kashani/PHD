---
type: report
date: 2026-08-20
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Creativity and Exploration", "Multi-Agent Debate", "Evaluation and Metrics"]
tags: [type/report, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/3c2d68da32f08081b7fccec0f97da620
created: 2026-08-20
updated: 2026-08-20
---
# Creativity Exp 2 - Conclusion

> [!summary] In short
> Conclusion of Exp 2 (asymmetry levers). Persona works best; representation is a null; the new aggregator helped creativity but lost the correct diagnosis.

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Creativity and Exploration]], [[Multi-Agent Debate]], [[Evaluation and Metrics]]  
**From meeting:** [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]]  
**Sub-notes:** [[Creativity Exp 2 - Full Results]]  
**Original:** [Notion page](https://app.notion.com/p/3c2d68da32f08081b7fccec0f97da620)

---

## Conclusion — Experiment 2
**Design:** [`README.md`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/README.md) (frozen before any model call) **Full numbers:** [`results/report2.md`](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/results/report2.md) **Run:** 48 cells, 336 subagent calls, 0 failures, same 8 items and seed as Experiment 1.

---

### 1. The short answer
Four levers were screened against the identical-context baseline. Two work, one is a null, one overshoots — and the pipeline change I recommended after Experiment 1 turned out to be **actively unsafe** for medical use.

| Condition | Composite | Usable novelty | Agent diversity | Verdict |
|---|---|---|---|---|
| E0 single agent | −1.05 | 5.9 | — | worst overall |
| E1 identical debate | −0.23 | 4.9 | 0.089 | the baseline |
| E2 **masked** | +0.28 | 6.1 | 0.110 | works; safest |
| **E3 persona** | **+0.40** | **7.9** | 0.128 | **best on creativity** |
| E4 representation | +0.28 | 5.0 | 0.084 ↓ | **null manipulation** |
| E5 persona+masked | +0.32 | 7.5 | **0.145** | overshoots |

---

### 2. What worked

#### 2.1 Persona asymmetry beats masking — without withholding anything
Giving three agents different *reasoning stances* (mechanism-first, precedent-first, skeptic) over the **complete** context produced more agent divergence than hiding half the facts from each of them: **0.128 vs 0.110** text-level. It also produced:
- **+57% rare ideas** vs identical context (14.5 vs 9.25)
- **+62% usable novelty** (7.9 vs 4.9; mean diff +3.00, CI [+0.50, +6.12])
- the best composite of any arm

This is the cleanest positive result of the whole project. It matters practically because masking buys diversity by making agents *less informed* — which is what costs usefulness — whereas persona buys it for free.

#### 2.2 The aggregator was a real bottleneck
Experiment 1's headline was that identical-context debate scored *below* a single agent. **That reverses here**: E1 (−0.23) now beats E0 (−1.05). E0 is the only arm with no aggregator; every arm that passes through the rewritten aggregator gained, and the one that does not, did not.
On the same items across pipelines, identical-context debate moved: novelty 3.00 → 3.75, **flexibility 8.1 → 13.2 categories**, usefulness 3.38 → 2.88. That is a larger movement than any context manipulation produced in Experiment 1. The output stage was throttling the system.

#### 2.3 The mechanism partly survives — in a specific form
Experiment 1 found diversity did not predict creativity, and I suspected the whole-text cosine metric was too blunt. Adding the idea-level Jaccard metric did **not** rescue the composite correlation (ρ = +0.10, p = .55). But both metrics predict **usable novelty**:

| diversity metric | vs composite | vs usable novelty |
|---|---|---|
| text cosine | +0.13 (p=.42) | **+0.42 (p=.008)** |
| idea Jaccard | +0.10 (p=.55) | **+0.40 (p=.011)** |

These are the only clear correlations across both experiments. The asymmetry hypothesis survives in a narrowed form: **more divergent agents do surface more rare-but-defensible ideas.** It fails to show up in the composite because usefulness moves the other way, cancelling it.

---

### 3. What did not work

#### 3.1 Representation asymmetry is a null — the second surface manipulation to fail
Same facts as prose, as a numbered list, and as thematically grouped bullets. Diversity: **0.084, below the 0.089 identical-context baseline.** Usable novelty +0.12 (d = −0.02, negligible).
Combined with Experiment 1's fact-order shuffling result, a clear pattern emerges and it generalises:
> **Surface perturbations of the context do nothing. The model is invariant to both the order and the format of the same facts. Only changing *what* an agent knows (masking) or *how it is told to think* (persona) moves it.**
That is a more useful finding than another positive lever would have been, because it rules out an entire family of interventions cheaply.

#### 3.2 Stacking asymmetries overshoots
E5 has the highest diversity on both metrics (0.145 / 0.919) and the highest raw novelty (4.13) — but the **lowest usefulness of any arm (2.50)** and a composite *below* persona alone. Two levers pull the agents past the point where their output stays defensible. Persona alone is the better operating point.

---

### 4. The finding that overrides the rest
**Ranking by originality instead of plausibility broke the medical task.**

| pipeline | mean rank of the correct diagnosis | recall |
|---|---|---|
| Exp 1 — cap 12, plausibility-first | **1.2** | 1.00 |
| Exp 2 — cap 20, originality-first | **9.9** | 0.92 |

On `med_ddx_002` (Bartter syndrome), E3 — the best condition on every creativity metric — scored **zero recall**. The gold diagnosis appears nowhere in its twenty ideas.
And it is not the debaters' fault. **All three E3 agents proposed Bartter syndrome in round 2; two ranked it #1.** The aggregator was handed the right answer three times and threw it away, because a correct textbook diagnosis is by construction not original. What it published instead led with Munchausen by proxy, laxative abuse and heavy-metal poisoning — creative, superficially defensible, and wrong.
This was **my** recommendation from Experiment 1's conclusion, and this run falsifies it. The honest statement is: unshackling the aggregator raises every creativity metric and, on a task with a right answer, destroys the thing you actually needed. The 12-idea cap and the plausibility-first ordering were not just throttles — they were also the safety mechanism.
The fix is not to revert. It is to stop asking one ranked list to do two jobs: **emit two lists** — a plausibility-ranked shortlist and a separately-ranked speculative list — so novelty never competes with the correct answer for a slot.

---

### 5. Cap behaviour (incidental but useful)
Idea counts saturate at exactly whatever cap is set: 12 → 12.0, 20 → 20.0, in essentially every cell of both experiments. The cap is not a creativity ceiling the model strains against; it is an output-length instruction it obeys mechanically, and it will pad to fill. Raw fluency is therefore uninformative at any cap, and "raise the cap" is not by itself an intervention — what changed the results here was the *ranking rule*, not the number of slots.

---

### 6. Recommendation
1. **Adopt persona asymmetry.** Best creativity of any lever, no information withheld, and it composes with the existing harness at zero extra cost.
1. **Revert originality-first ranking; replace it with dual output.** A plausibility-ranked clinical shortlist plus a clearly-labelled speculative list. Never let one ranking serve both.
1. **Drop representation asymmetry**, and with it the whole surface-perturbation family. Two independent nulls is enough.
1. **Do not stack levers** without re-checking usefulness; E5 shows the diversity/defensibility trade-off has a turning point.
1. **Now spend the quota on repeats.** Both experiments have run at 1 repeat per cell, so every effect reported here still has run-to-run variance baked into it. The screening phase is done — the next run should be persona vs identical context, 3+ repeats, dual-output aggregator, and gold rank as a primary outcome rather than an afterthought.

---

### 7. One paragraph
Persona asymmetry is the best creativity lever found across both experiments: it beats fact-masking on agent diversity, rare-idea count and usable novelty while leaving every agent fully informed. Representation asymmetry is a null, confirming a general rule that surface perturbations of context — order, format — do nothing to these models, while changes to information or to instructed reasoning stance do. The aggregator was genuinely throttling output, and unshackling it produced the largest single improvement in the project. But the same change moved the correct diagnosis from rank 1.2 to rank 9.9 and lost it entirely in one cell, after the debaters had ranked it first — which means **the creativity gain and the clinical failure came from the same edit**, and any deployment has to separate the speculative list from the answer list rather than choosing between them.

---
*Screening run, 1 repeat per cell. Effect sizes are indicative, not confirmatory. Single LLM judge from the debaters' model family; no clinician review; 2 of 4 datasets synthetic. See* [*`results/report2.md`*](https://file+.vscode-resource.vscode-cdn.net/f%3A/PHD/SFU/Creativity%20Test/experiments/exp2-asymmetry-levers/results/report2.md) *§Limitations.*
