---
type: report
date: 2026-08-20
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Creativity and Exploration", "Multi-Agent Debate"]
tags: [type/report, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate]
notion: https://app.notion.com/p/3c2d68da32f0804aaba7f3386ab7e59a
created: 2026-08-20
updated: 2026-08-20
---
# Creativity Experiments - Key Findings

> [!summary] In short
> Five main findings from both creativity experiments: identical-context debate fails, surface changes do nothing, persona is the best lever, the aggregator matters most, and originality-first ranking broke the medical task.

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Creativity and Exploration]], [[Multi-Agent Debate]]  
**From meeting:** [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]]  
**Original:** [Notion page](https://app.notion.com/p/3c2d68da32f0804aaba7f3386ab7e59a)

---

Five things, ordered by how much I'd stake on them.
**1. Identical-context debate is the failure mode, not the baseline.**
  Three agents given the same context converge, and the aggregator then compresses an already-homogeneous set. In Exp 1 it scored *worse than a single agent* (−0.74 vs −0.34). Almost any perturbation beats it. This held across both runs and is the most robust pattern in the data.
**2. Surface perturbations do nothing. Semantic ones work.**
  Two independent nulls make this a real generalisation:

| Manipulation | Effect on agent diversity |  |
|---|---|---|
| Fact-order shuffling (Exp 1) | 0.116 → 0.111 | **null** |
| Representation: prose / table / list (Exp 2) | 0.089 → 0.084 | **null** |
| Fact masking | 0.089 → 0.110 | works |
| **Persona / reasoning stance** | 0.089 → **0.128** | **best** |

These models are invariant to the *order* and the *format* of the same facts. Only changing **what an agent knows** or **how it's told to reason** moves it. That retires a whole family of interventions cheaply.
**3. Persona is the best lever found — and it's free.**
  It beats masking on diversity (0.128 vs 0.110), rare ideas (14.5 vs 9.25, +57%) and usable novelty (7.9 vs 4.9, +62%) — while every agent still sees the *complete* context. Masking buys diversity by making agents less informed, which is what costs usefulness. Persona doesn't pay that price.
**4. The aggregator matters more than the context does.**
  Rewriting the merge step moved judged flexibility from 8.1 to 13.2 distinct mechanisms — a bigger change than any context manipulation in Exp 1, and enough to reverse finding #1. The output stage was throttling the whole system.
**5. And that same fix broke the medical task.**
  This is the one I'd lead with. Originality-first ranking moved the correct diagnosis from mean rank **1.2 → 9.9**, and lost it entirely in one cell. On the Bartter syndrome case, **all three debaters proposed it and two ranked it #1** — the aggregator was handed the right answer three times and discarded it, because a correct textbook diagnosis is by construction not original. It published Munchausen by proxy and heavy-metal poisoning instead.
That was *my* recommendation after Exp 1, and this run falsifies it. The creativity gain and the clinical failure came from the same edit. The fix isn't to revert — it's to stop making one ranked list do two jobs: emit a plausibility-ranked answer list and a separate speculative list.
**On the mechanism:** agent diversity never predicted composite creativity (ρ ≈ +0.1, n.s.) — but it does predict *usable novelty* at ρ ≈ +0.4, p ≈ .01. That's the only clear correlation in either experiment. The asymmetry hypothesis survives, narrowed: more divergent agents surface more rare-but-defensible ideas; it just doesn't reach the composite because usefulness moves the other way.
**Caveat that applies to all of it:** both experiments ran at 1 repeat per cell, so run-to-run variance is confounded with condition effect throughout. This is screening-grade evidence. The next run should be narrow and powered: persona vs identical context, ≥3 repeats, dual-output aggregator, gold rank as a primary outcome.
