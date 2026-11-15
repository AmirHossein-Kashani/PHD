---
type: project-note
project: Auto Researcher
tags: [type/project-note, project/auto-researcher]
---
# AR 5 - Open Questions

Part of [[AR 0 - Start Here|Auto Researcher]]. Collected from [[AR 7 - Meeting Log#2026-09-21 One-on-One|2026-09-21 One-on-One]], [[AR 7 - Meeting Log#2026-10-14 One-on-One|2026-10-14 One-on-One]], [[AR 7 - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]], [[01 Now and Next#Daily 2026-10-20 to 2026-10-23|Daily 2026-10-20 to 2026-10-23]] and [[AR 6 - Paper Draft (Introduction)]].

> [!tip] How to use this note
> When you decide something, move it to [[04 Decisions Log]] and tick it here.

## Method
- [ ] **Exploration vs refinement:** how much budget for new "jumping" ideas and how much for refining? How to combine them?
- [ ] **Failure refinement rule:** the current rule is weak. What should the path record when an idea fails, and how should it be used?
- [ ] **Blocking rule:** blocking made agents less ambitious in the October run. Keep it, soften it, or replace it?
- [ ] **Explicit research steps:** should we define research steps (with a literature review step) like 3 of 4 close works do? Which level of freedom?
- [ ] **Carry memory across rounds:** keep the Phase 1 tree and memory across rounds, and add a leaf-selection tool, so unused features get tested.
- [ ] **Ambitious sub-agents:** test short-lived agents that ignore parent decisions to make very novel ideas.

## Evaluation
- [ ] Which benchmark replaces or joins the Kaggle task? (See [[AR 3 - Benchmarks]].)
- [ ] How to stop agents from copying public answers (search access)?
- [ ] How many repeats per setting? (Creativity experiments showed 1 repeat is not enough.)
- [ ] Which metrics: final score, speed to a target score, cost (hours, tokens), depth, merged cycles?

## Baselines
- [ ] Finalize baselines for the researcher paper. Two options in [[AR 4 - Related Work and Baselines#3. Which baselines to run|Related Systems, section 8]]: a runnable set (Arbor default, Claude Code, ML-Master 2.0, AIRA-dojo, autoresearch) or the list's set (Arbor, AI Scientist-v2, ResearchAgent, plus Robin and MOOSE-Chem).

## Paper
- [ ] Make the introduction clearer ("fix this version and improve", Autoresearcher (6).pdf in [[AR 7 - Meeting Log#2026-10-20 One-on-One|2026-10-20 One-on-One]]).
- [ ] Find references for: "every problem can be solved by LLMs but we must find how much time" and for the exploration claims (marked "find reference" in the draft).
- [ ] Footnote: in the AI co-scientist case the 10 years included lab validation (time-to-hypothesis, not time-to-proof).
- [ ] Confirm title and author order of the Penadés et al. (2025) Cell paper.

## Scientific questions

Added from our discussion on 1 November 2026.

[Planning to do](#planning-to-do) defines the proposed experiments; [Related work answered](#related-work-answered) identifies results and mechanisms we must compare against. [What we already explored](#what-we-already-explored) records the local observations motivating these questions. Proposed mechanisms are untested, and their novelty remains to be established.

### Planning to do

- **Can independent ideas improve the balance between exploration and exploitation?**  
  Let some ideas start without a parent hypothesis, while others refine existing branches. Independent ideas may expand on their own or later exchange findings with other branches; connection is optional. Test whether this improves performance under the same total research budget, rather than merely producing more ideas.

- **Which inherited assumptions should a new direction keep or remove? — First priority.**  
  Separate measured observations from the interpretations attached to them. At selected checkpoints, compare proposals generated with the usual context against proposals with one inherited assumption withheld, while retaining the underlying observations. Test whether selective removal produces better exploration than random independent ideas or complete history resets.

- **Should a branch receive credit for helping another branch succeed?**  
  A branch may have a weak final solution but discover a useful component or finding. At selected transfer events, compare matched continuations of the receiving branch with and without that contribution. Investigate whether funding branches using both direct gains and measured transfer benefits improves the whole run; count the cost of measuring those benefits.

- **How long should a new direction develop before it competes with the incumbent?**  
  A fresh idea may initially lose to a solution that has already received many rounds of refinement. Give new directions a bounded initial allowance, then examine improvement trajectories and uncertainty when allocating further budget. Treat this as a supporting mechanism and baseline until we establish a distinction from existing budget-allocation methods.

- **What experiment would establish that selective independence is useful?**  
  Compare existing search, random independent directions, complete history resets, and selective assumption removal with matched models, tools, and total cost. First hold starting code fixed to isolate context effects; then test independence from the incumbent's implementation. Measure held-out performance, progress versus cost, and repeated-run variability, including all selection and probing overhead.

### Related work answered

These studies answer parts of our questions in their evaluated settings; they do not establish a universal exploration policy. Their mechanisms constrain the claims we can make in the planned experiments.

- **Does accumulated research evidence help later experiments?**  
  [Arbor](https://arxiv.org/html/2606.11926v1) already propagates insights along ancestor paths and uses failure evidence to guide later proposals. Relative to our [[AR 1 - Problem and Method|path-to-root proposal]], ancestry and failure memory alone are therefore insufficient distinctions. Our question concerns when inherited interpretations should stop constraining a new direction.

- **Can stagnation guide the switch from refinement to exploration?**  
  [FML-bench](https://arxiv.org/html/2605.17373v2) evaluates AdaptiveSearch, which begins with greedy improvement and switches to multiple branches after stagnation. Compared with our independent-origin proposal, its branches generally start from strong earlier codebases, with baseline fallbacks. A generic rule to branch when progress stalls is already covered.

- **Can removing search history restore exploration?**  
  [Scaling Discovery Requires Forgetting](https://arxiv.org/abs/2610.20563) reports gains from periodically discarding agent sessions and candidate history while retaining the best candidate. It is a direct baseline for selective assumption removal: our proposed method must justify choosing what to remove, including its extra cost, against this broader reset.

- **Can independent branches benefit from selective evidence sharing?**  
  [CoEvolve](https://arxiv.org/html/2609.35315v1) routes evidence between parallel branches while maintaining separate beliefs and discounting imported evidence. Optional connections and selective sharing are consequently close prior work. Its reported transfer-window uplift is associational; our proposed matched continuations would investigate the effect of particular transfers and their use in budget allocation.

- **Can private histories and adaptive allocation support a research team?**  
  [Building AI Research Teams](https://arxiv.org/html/2610.28887v1) combines disciplinary perspectives, private research histories, shared evidence, and adaptive allocation in linear-attention discovery. Like CoEvolve, it makes independence plus collaboration an existing design pattern. We need to isolate the benefit of our specific detachment or allocation rule beyond that pattern.

- **Does broader or more diverse search necessarily produce better discoveries?**  
  [Heuresis](https://arxiv.org/abs/2606.25198) compares greedy, archive-based, island, and divergent search, finding trade-offs among quality, diversity, and novelty in its evaluated domains. Together with FML-bench, it motivates measuring useful gains rather than assuming that additional roots or semantic diversity establish better research.

### What we already explored

These are observations from our project records and earlier pilots, not results for the independent-origin or selective-removal experiments proposed above.

- **Did our Phase 1 improve the research trajectory?**  
  The September notes report scores of 0.322 for Phase 1 and 0.325 for Arbor, about half the runtime, and reasoning chains of 3–5 levels versus 2. This suggests an efficiency question worth testing, but the available summary does not establish statistical equivalence or a general performance advantage. Source: [[AR 2 - Experiments and Results]].

- **Did remembering failures help, and could blocking become restrictive?**  
  Shared findings and failure memory were reported as the most useful Phase 1 feature. The October logs also describe blocking as making agents less ambitious, and an idea remaining blocked after its reopening condition was met. These observations motivate testing when to retain a failure-based restriction and when to reconsider it. Source: [[AR 1 - Problem and Method]].

- **Did early gains persist through longer exploration?**  
  In the October four-scaffold comparison, our approach led in the early cycles, while Claude Code finished about +0.03 ahead. The result motivates studying the allocation between refinement and new directions across the run. It does not yet identify which mechanism caused the later difference. Source: [[AR 2 - Experiments and Results]].

- **Were the proposed search controls actually exercised?**  
  Reactivation, duplicate merging, concurrent ideas, and confidence-based selection were unused in the September run; the evidence router found 0 candidate leaves in 10 attempts. Each round used one agent and a fresh tree, limiting what those runs could test. Persistent branches and exercised selection decisions are prerequisites for evaluating the new questions. Source: [[AR 1 - Problem and Method#10. Mechanisms tried in Phase 1 and what happened]].

- **Could the evaluation signal itself explain misleading research decisions?**  
  Local validation and leaderboard results did not reliably agree, final submissions were limited, and web access allowed agents to reuse public solutions. Our next comparisons therefore need consistent development feedback and a separate final evaluation unavailable to search. Source: [[AR 2 - Experiments and Results#Problems with the Kaggle setup]].

- **What did the earlier creativity pilots establish about generating alternatives?**  
  The small debate pilots found that persona differences and masking could change diversity, while diversity gains did not consistently translate into useful final outputs; aggregation could discard a correct candidate. These findings motivate separating generation, refinement, and selection, but do not establish benefits for long-horizon research search. Sources: [[Creativity Experiments - Key Findings]], [[Creativity Exp 2 - Conclusion]].
