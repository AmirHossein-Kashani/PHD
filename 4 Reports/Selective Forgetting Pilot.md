---
type: report
date: 2026-11-16
status: Pilot complete, hypothesis untested
clusters: [Fir, Nibi]
model: claude-opus-5-5
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context", "Evaluation and Metrics"]
tags: [type/report, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context, topic/evaluation-and-metrics, selective-forgetting, pilot]
created: 2026-11-16
updated: 2026-11-16
---
# Selective Forgetting Pilot

> [!summary] In short
> In every finished batch the fork happened when the task was already solved, so no approach could gain after the
> fork. For that reason the pilot gives no evidence for or against selective forgetting. It does show that the
> pipeline works, that the agents beat the task baselines on hidden data, and that the two test tasks are too easy
> for Opus 5.5.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]], [[Evaluation and Metrics]]  
**Part of:** [[AR 2 - Experiments and Results]] · **Method:** [[AR 1 - Problem and Method]] · **Questions:** [[AR 5 - Open Questions]] · **Log:** [[AR 7 - Meeting Log]]  
**Date:** 16 Nov 2026 · **Clusters:** Fir, Nibi · **Model:** `claude-opus-5-5`

---

## 1. Setup

- **Agent:** Arbor, variant `forget` (Phase 1 + Phase 2 + hard cycle budget), Claude Code headless on the Max plan.
- **Model:** `claude-opus-5-5`, effort xhigh. On Fir also three `adme_h` trunks at medium effort (batch name `e2m`).
- **Tasks:** `tsp_h` (TSP heuristic, score = nearest-neighbour length / tour length) and `adme_h` (8 TDC ADMET
  endpoints, score = 100 x mean of Spearman / AUROC). Each has a dev split for the agent and a hidden holdout.
- **Design:** a trunk agent works until it stalls (no dev gain above `eps` in 3 cycles, checked from cycle 4) or
  ends its own run. At that point the harness builds a record (ideas, observations, interpretations), selects one
  interpretation with the rule `dependents / (1 + support)`, and starts one new run per approach with 5 cycles.
- **Runs:** Fir, night and day of 16 Nov (batches `e2`, `e2b`, `e2m`). Nibi, 09:45 to 16:26 EST (batches `pilot`).
  All agents are stopped on both clusters.

## 2. The approaches that were compared

| Arm | Approach | What the new run receives | Role of an approach from the literature |
|---|---|---|---|
| N | Continue | nothing new: the trunk goes on | default agent without restart |
| A | New direction, full record | all ideas, observations, interpretations | branch on stagnation with the full history ([[FML-bench - Search Dynamics of Research Agents\|FML-bench]] AdaptiveSearch) |
| **B** | **Selective forgetting (ours)** | **all observations; the selected interpretation is removed** | — |
| C | Random forgetting | all observations; one random interpretation is removed | control: "any removal helps" |
| D | Doubt | full record plus "assume X is false" for the selected interpretation | "challenge, do not withhold" |
| E | Reset to best | only the best solution, no record | full reset that keeps the best candidate (Tan et al., arXiv 2610.20563) |
| F | Restart | the plain task baseline, no record | random restart |
| BP, DP | Planted-belief test | as B and D, but for a false belief that I planted | — |

> [!warning]
> Arms A, D and E only play the role of the cited approaches inside my harness. The original systems were not run.

## 3. Comparison on `tsp_h` (five finished batches, 35 runs)

Hidden holdout at the end of each run, with the cycles the run completed (budget 5). First row: score at the fork.

| Arm | Fir s1 | Fir s2 | Fir s3 | Nibi s1 | Nibi s3 | Mean change from the fork |
|---|---|---|---|---|---|---|
| fork | 1.2798 | 1.2796 | 1.2796 | 1.2781 | 1.2786 | |
| N | 1.2798 (5) | 1.2796 (5) | 1.2796 (3) | 1.2781 (2) | 1.2786 (2) | 0.0000 |
| A | 1.2798 (1) | 1.2801 (2) | 1.2801 (3) | 1.2781 (1) | 1.2786 (5) | +0.0002 |
| **B** | 1.2801 (1) | 1.2801 (2) | 1.2799 (4) | 1.2781 (5) | 1.2786 (1) | **+0.0002** |
| C | 1.2798 (2) | 1.2800 (3) | 1.2801 (2) | 1.2781 (2) | 1.2786 (1) | +0.0002 |
| D | 1.2798 (2) | 1.2800 (3) | 1.2800 (5) | 1.2799 (2) | 1.2786 (5) | +0.0005 |
| E | 1.2797 (0) | 1.2799 (4) | 1.2799 (4) | 1.2781 (5) | 1.2795 (5) | +0.0003 |
| F | 1.2799 (3) | 1.2799 (2) | 1.2798 (2) | 1.2770 (2) | 1.2782 (2) | -0.0002 |

- The smallest step that the task counts as an improvement is `eps` = 0.002. Every change in the table is below it.
- Paired difference B minus A in the five batches: +0.0003, 0, -0.0002, 0, 0. Mean +0.00002.
- Arm F reaches the same level from the plain baseline in 1 to 3 cycles, so the record has no measurable value here.
- The cycle counts are unequal (usage limit, and runs that ended themselves). The comparison is not at equal budget.
- For N the number in brackets is the total number of trunk cycles.

> [!important]
> Ranking the approaches from this table is not allowed. All differences are inside the noise of a solved task.

## 4. Planted false belief (`e2b`, Fir)

A false belief was written into the starting record: "random kicks with repair do not improve the tours". The arms
BP (belief dropped) and DP (belief doubted) test whether the selection rule finds it and whether removal helps.

| Seed | Fork | Trunk used the forbidden direction | Rank of the planted belief | Arms |
|---|---|---|---|---|
| 1 | cycle 2, trunk ended itself | yes, in cycle 1 | none: record extraction failed | none |
| 2 | cycle 1, trunk ended itself | yes, in cycle 1 | 11 of 16 | A 1.2800, B 1.2787, BP 1.2801, DP 1.2787 (fork 1.2787) |
| 3 | cycle 3, trunk ended itself | yes, in the code at the fork | 16 of 20 | stopped at 0 cycles |

The plant was invalid in 3 of 3 seeds: Opus tested the belief itself and refuted it before the fork. The ranks and
the four arm scores therefore carry no evidence. The same happened in the earlier Sonnet test.

## 5. `adme_h`: trunks only, no arm ran

Dev score after each cycle. Baseline: dev 77.60, hidden holdout 78.37. Stronger reference model: dev 81.94, holdout
81.70. Bootstrap standard error: 0.62 (dev), 0.53 (holdout).

| Trunk | Cluster, effort | Dev by cycle | Hidden holdout of the last state |
|---|---|---|---|
| `e2_adme_h_s1` | Fir, xhigh | 80.19, 83.42 | not computed |
| `e2_adme_h_s2` | Fir, xhigh | 82.55, 83.47 | not computed |
| `e2_adme_h_s3` | Fir, xhigh | 82.84, 84.06 | not computed |
| `e2m_adme_h_s1` | Fir, medium | 83.13, 83.59 | not computed |
| `e2m_adme_h_s2` | Fir, medium | 83.00, 83.61, 83.61, 84.04 | not computed |
| `e2m_adme_h_s3` | Fir, medium | 79.94, 83.59, 83.83, 83.83, 83.96, 84.06 | not computed |
| `pilot_adme_h_s1` | Nibi, xhigh | 83.87, 83.87, 83.87 | 83.58 |
| `pilot_adme_h_s2` | Nibi, xhigh | 80.50, 83.77, 83.77 | 83.27 |
| `pilot_adme_h_s3` | Nibi, xhigh | 82.77, 83.98, 84.27, 84.27 | 83.77 |

- All nine trunks sit between 83.4 and 84.3 from cycle 2 on. Later steps are 0.0 to 0.4, below the standard error.
- Medium and xhigh effort reach the same level. Medium cost about a third per cycle ($1.90 against $5.24, one pair).
- On the hidden holdout the three Nibi trunks gain 4.9 to 5.4 points over the baseline. The gain is 1.1 to 1.3
  points smaller than on dev.
- No batch reached a fork, so there is no comparison of approaches on this task.

## 6. Against the task baseline

| Task | Baseline | Agents | Change |
|---|---|---|---|
| `tsp_h`, hidden holdout | 1.0000 | 1.2770 to 1.2801 | tours about 22% shorter |
| `adme_h`, hidden holdout | 78.37 | 83.27 to 83.77 | +4.9 to +5.4 points, 9 to 10 standard errors |

What the agents built is standard: local search with random kicks for `tsp_h`, and LightGBM on a union of common
fingerprints and RDKit descriptors for `adme_h`. The baselines are weak on purpose, and the tasks use my own splits,
so these numbers cannot be compared with public leaderboards. Nothing here is a publishable result of its own.

## 7. What the selection rule chose (Nibi)

| Batch | Chosen interpretation | Score |
|---|---|---|
| `pilot_tsp_h_s1` | "The 0.3% merge threshold is the bar for a change to matter, so a change that cannot raise the score by at least 0.3% is not worth a cycle." | 7.0 (7 ideas set aside); next 2.5 |
| `pilot_tsp_h_s2` | "Stronger methods must be built on a fast, correct local-search engine, so they should wait until that engine is merged." | 3.0; next 2.0 |
| `pilot_tsp_h_s3` | "The score is bounded by the optimal tours, so no valid tour can score above them." | 4.0 (4 ideas set aside); next 3.0 |

In two of three batches the rule chose the trunk's own reason to stop. That is the belief that blocks the most ideas,
which the rule is built to find. On a solved task this belief is true, so its removal cannot help. Two cases are an
observation, not evidence.

## 8. Conclusions

**Supported by the runs**

1. The pipeline works on two clusters (trunk, fork, record, selection, arms, hidden scoring). The audit found no
   tool call outside a run folder.
2. The agents beat the task baselines, also on hidden data.
3. `tsp_h` cannot separate the approaches: 35 runs, all within 0.002 of the fork score.
4. A belief that is cheap to test does not hold Opus: 3 of 3 trunks refuted the planted belief before the fork.
5. Agents often end a run before the cycle budget is used. Most forks came from the trunk ending itself.
6. The 5-hour limit of the Max plan bounds the work (about 100 to 140 xhigh cycles a day) and made the budgets of
   the arms unequal.

**Not supported**

- Anything about selective forgetting against the other approaches. It is untested.
- Anything about the selection rule finding a false belief (the plant was invalid).
- That `adme_h` can separate the approaches. I expect that it cannot, but no arm has run.

## 9. Why this test bed could not show the effect

Selective forgetting can help only when three conditions hold at the fork.

| Condition | `tsp_h` | `adme_h` | CASMI campaign |
|---|---|---|---|
| Room to improve after the fork | no | doubtful | yes |
| A belief blocks the way to that room | no: the blocking belief is true | unknown | yes: "hidden test = visible test" |
| The agent cannot test the belief cheaply | no | unknown | yes: a test cost Kaggle submissions |

The fork rule cannot tell "stalled by a wrong belief" from "task finished". The pilot produced the second case only.

## 10. Operational lessons

- A failed launch during a limit window counts toward the 12 launches of a run, so runs end early. Fix before the
  next long run: do not count such a launch, and wait for the reset time in the limit message.
- Agents on two clusters share one limit with my chats. Run on one machine at a time.
- Several `adme_h` evaluations at the same time in one job can become very slow (one took over 2,400 s in place of
  150 s). Watch evaluation times when many arms run together.
- The "stronger reference" of `adme_h` (81.94) is too low as a scale: every trunk passes it in one or two cycles.
- The Slurm account on Fir is `def-ester_cpu`.
- Cost at list price: $2 to $6 per xhigh cycle, about $50 for one `tsp_h` batch.

**Extra details from the Fir notes** (kept from the first version of this note):

- Gate G1 of the runbook is not passed: point 1 fails (the `e2b_tsp_h_s1` fork failure; `adme_h` unfinished) and point 4 fails for `tsp_h` (saturated).
- E2b was started together with E2 wave 1, not after gate G1 as the runbook says.
- The `e2b_tsp_h_s1` record extraction failed after 3 tries at 01:43, at the time of the first limit hit; the cause is not confirmed.
- The forbidden-direction detector is a list of text patterns; the seed 3 hit was not read by a person (the score jump to 1.2582 supports it).
- An operator script writes `PAUSE` files while the limit is active, but it is untested in a real limit window.
- Not started on Fir: E2 wave 2 (seeds 4, 5) and M (a second model).

## 11. Next steps (proposal, not decided)

- [ ] Stop all work on `tsp_h` and on the kick plant.
- [ ] One complete `adme_h` batch at medium effort (`e2m_adme_h_s3` on Fir is at its fork). Rule fixed in advance:
      drop the task if no arm ends more than 1.0 hidden point above the fork score.
- [ ] Repair the launch counter in `batch.py` (new experiment name after the change).
- [ ] Design a task where the blocking belief is costly to test and room to improve remains (dev split and hidden
      holdout differ, few probes of the shifted data). CASMI is the model for it.
- [ ] Tell Prof. Ester the status: pipeline works, first test bed too easy, main claim has no result yet.
- [ ] Copy the results from scratch to project space (scratch has no backup on either cluster).

## 12. Lines for other notes

**[[AR 7 - Meeting Log]]**
> 2026-11-16: First real runs of the selective-forgetting experiment (Fir and Nibi, Opus 5.5). Five `tsp_h` batches
> finished; all approaches end at the fork score (task solved in 2 cycles). Planted belief refuted by the agent in
> 3 of 3 trunks. `adme_h`: nine trunks near 84 by cycle 2, no fork. Hypothesis untested. See
> [[Selective Forgetting Pilot]].

**[[04 Decisions Log]]**
> 2026-11-16: `tsp_h` and the kick plant are dropped as test beds for selective forgetting (saturated; plant does
> not bind). Open: one `adme_h` check batch at medium effort; new test bed with a costly-to-test belief.

**[[AR 5 - Open Questions]]**
> - How can a fork rule tell "stalled by a wrong belief" from "task finished"?
> - Which cheap task has a belief that the agent cannot test cheaply (as "hidden test = visible test" in CASMI)?
> - Does the selection rule `dependents / (1 + support)` find the stopping belief in general (2 of 3 cases so far)?

**[[01 Now and Next]]**
> Now: decide the next test bed for selective forgetting. Next: one `adme_h` check batch; status note to Prof. Ester.

## 13. Sources

- Fir notes: https://claude.ai/artifact/FTw8pRXC6Nu44VZmTSWcje
- Nibi handoff file `NIBI_RESULTS.md`: https://claude.ai/artifact/3DFku3dUq86eRFtMfFh799
- Nibi: `/scratch/kashi77/Creativity_Arbor/reports/exp/pilot_*/` (results, records, selections, logs)
- Fir: `/scratch/kashi77/Creativity_Arbor/reports/exp/e2_*`, `e2b_*`, `e2m_*`
- Plan and runbook: `reports/EXPERIMENT_PLAN.md`, `reports/FIR_RUNBOOK.md`
