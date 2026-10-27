---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher, topic/autonomous-research-agents, topic/evaluation-and-metrics]
---
# AR - Experiments and Results

Part of [[Auto Researcher]]. Sources: [[AR - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]], [[AR - Meeting Log#2026-09-29 One-on-One|2026-09-29 One-on-One]], [[AR - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]].

> [!summary] In short
> Our Phase 1 on Arbor is **faster, deeper and cheaper** than Arbor default, but the **final score is the same**. On a Kaggle task with four scaffolds, our approach led early, but **Claude Code won at the end**. The Kaggle setup itself has problems, so we need better evaluation tasks.

---

## Run 1 - Phase 1 vs Arbor default (September 2026)

Task: a Kaggle task with spectra data (the exact competition name is not written in Notion). Goal score: 0.35.

| What | Arbor default (base) | Our Phase 1 |
|---|---|---|
| Final score | 0.325 | 0.322 (tie, inside leaderboard noise) |
| Rounds | 6 | 3 (one was a leaky first try) |
| Cycles merged | 23% | **62%** |
| Hours used | about 2x | **about half** |
| Speed from scratch | 3 rounds to pass 0.275 | **0.287 in one round** |
| Depth of reasoning | trees 2 levels deep | **chains 3 to 5 levels deep** |
| Stopping | always used all 12 cycles | stopped after 9 when no ideas were left |

Other findings:
- **Key discovery by Phase 1:** the Kaggle test spectra are copies of training rows with wrong labels (worth +0.028). Base saw the signal but did not act on it.
- **Honest self-correction by base:** base found its own data leak and retrained honestly in round 6, which gave its best score.
- **Shared limits:** the local proxy was easier than Kaggle, and the neural network is weak on unseen natural products.
- **Next step written then:** carry the Phase 1 tree and memory across rounds, and add a leaf-selection tool.

What worked and what was not used: see the table in [[AR - Method and Contributions#6. Mechanisms tried in Phase 1 and what happened]].

## Run 2 - more cycles, Phase 2 and Claude Code (end of September)

From [[AR - Meeting Log#2026-09-29 One-on-One|2026-09-29 One-on-One]]:
- More cycles improved the speed of reaching the goal.
- Ran a **Claude Code** baseline.
- Ran **Phase 2**: plan, implement, plan, implement.

## Run 3 - four scaffolds on Kaggle (October 2026)

From [[AR - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]]:
- In the **first 4 cycles our approach was much better**. It improved the base code and reached **rank 17** (about +0.18 compared to Claude Code and Arbor base).
- In **round 10, Claude Code won** the final with about +0.03 over all others. The other three approaches ended at about the same level.
- **Logs:** our *blocking* criteria made the model less ambitious; it paused ideas.
- At the time of the note: Claude Code at rank 60 (40 two days before); the others around rank 200. These scores were higher than the first place of two weeks before.

## Problems with the Kaggle setup

- The number of final test submissions is limited.
- Good validation scores did not give good final scores.
- Agents could search the web and seemed to **copy ("cheat")** more than create. But they did improve the best public solution by about 0.24, which is still worth noting.

So: we need tasks with a fair, hidden evaluation. See [[AR - Benchmarks]].

## Earlier pilot with four research scaffolds (used in the paper text)

From [[AR Paper - Introduction Draft]]:
- About **77%** of all model usage was agents re-reading their own context.
- The arm with optional planning stopped early with budget left; the arm with enforced plan reviews produced the only revisions of blocked ideas.
- Blocked ideas stayed blocked after their reopening condition was met; agents trusted a misleading local validation; agents bypassed a merge tool instead of revising the idea.

## Related experiments in other projects

- [[Creativity in Multi-Agent Debate]] - how to make agents more diverse (persona works best).
