---
type: project-hub
status: paused
period: 2026-08
tags: [type/hub, project/creativity-debate, topic/creativity-and-exploration, topic/multi-agent-debate]
---
# Creativity in Multi-Agent Debate

> [!summary] In short
> Two small experiments (August 2026) asked: **can we make a group of debating LLM agents more creative?** Main answers: if all agents see the same context they **agree too fast** and become *less* creative than one agent. **Giving each agent a different persona** (reasoning stance) is the best and cheapest way to get diversity. The **aggregator** (the step that merges ideas) matters more than the context, but ranking by originality **lost the correct medical diagnosis**. So: give two lists - a safe list and a speculative list.

Status: **paused** (screening results only, 1 repeat). The lessons feed the [[AR 0 - Start Here|Auto Researcher]] (exploration, diversity of agents).

Main notes: [[Creativity Experiments - Key Findings]] (start here), [[Creativity Exp 1 - Index and Conclusion]], [[Creativity Exp 1 - Full Results]], [[Creativity Exp 2 - Conclusion]], [[Creativity Exp 2 - Full Results]], [[Creativity Experiments - Datasets]], meeting [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]].

---

## 1. Setup

- 3 debater agents (Claude **Haiku**), 1 debate round after independent round-1 answers, an aggregator, and a blinded judge (Claude **Sonnet**).
- 4 datasets x 2 items = **8 items**, seed 42, **1 repeat per cell** (screening only).
- Datasets: `med_ddx` (real MedQA diagnosis questions, has a right answer), `med_repurpose` (synthetic drug repurposing), `gen_aut` (alternative uses of a brick), `gen_problems` (synthetic city problems).
- Measures: agent diversity in round 1, composite creativity, judge novelty, usefulness, flexibility, rare ideas, **usable novelty** (rare ideas minus flagged ones), flagged implausible ideas, gold-diagnosis recall and rank.

## 2. Experiment 1 - masking and shuffling

| Condition | Meaning | Composite creativity |
|---|---|---|
| C0 | single agent | -0.34 |
| C1 | 3 agents, identical context | **-0.74 (worst)** |
| C2 | each agent sees 50% of facts (together 100%) | +0.02 |
| C3 | facts in a different order per agent | +0.27 |
| C4 | masked + shuffled | **+0.79 (best)** |

- Masking raised agent diversity (0.116 to 0.152). Shuffling did nothing.
- Diversity did **not** predict creativity. C3 improved without more diversity, so the effect may be "break symmetry by any means", or noise.
- Cost: C4 had the most flagged implausible ideas.

## 3. Experiment 2 - asymmetry levers and the aggregator

| Condition | Composite | Usable novelty |
|---|---|---|
| E0 single agent | -1.05 | 5.9 |
| E1 identical debate | -0.23 | 4.9 |
| E2 masked | +0.28 | 6.1 |
| E3 **persona** | **+0.40** | **7.9** |
| E4 representation (prose, list, groups) | +0.28 | 5.0 |
| E5 persona + masked | +0.32 | 7.5 |

- Personas used: mechanism-first, precedent-first, skeptic.
- New aggregator (cap 20 ideas, originality-first) **reversed** Exp 1: debate now beat one agent.
- **But:** the correct diagnosis moved from mean rank **1.2 to 9.9**. In the Bartter syndrome case, all three agents proposed it and two ranked it first, but the aggregator threw it away because it was "not original".

## 4. Five lessons

1. Identical-context debate is a failure mode, not a baseline.
2. Surface changes (order, format) do nothing. Changing **what an agent knows** or **how it is told to think** works.
3. **Persona** is the best lever and it is free (agents keep full information).
4. The **aggregator** matters more than the context.
5. **Never let one ranked list do two jobs.** Output a plausibility-ranked list and a separate speculative list.

## 5. Recommended next run (not done yet)

Persona vs identical context, **3 or more repeats**, dual-output aggregator, gold rank as a main outcome, a second judge from another model family, and an arm with identical context but different seeds.

## 6. Background reading

- [[Review - Creativity Measuring Benchmarks]], [[EscapeBench]], [[MUTATE - Divergent Thinking Benchmark|MUTATE]]
- [[Is AI Really Creative]], [[Rethinking Creativity in the Age of GenAI (Mila talk)]]
- [[Creativity and Exploration]]

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-25 One-on-One

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-08-25 One-on-One|AR - Meeting Log]], [[Benchmarks and Datasets#2026-08-25 One-on-One|Benchmarks and Datasets]], [[Agent Architectures and Frameworks#2026-08-25 One-on-One|Agent Architectures and Frameworks]], [[Creativity and Exploration#2026-08-25 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-08-25 One-on-One|MS - Meeting Log]]

- [ ] find new datasets for creativity

### 2026-08-13 One-on-One (13 and 20 Aug)

> [!summary] This meeting in short
> Creativity debate slides and experiments (Exp 1, Exp 2). Find creativity metrics (diversity, novelty). New AMG-RAG results. List of multi-agent drug discovery papers to read.

*Original: [Notion page](https://app.notion.com/p/3bbd68da32f080b9b7e7f143d7fa3505)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-08-13 One-on-One (13 and 20 Aug)|MS - Meeting Log]], [[Biomedicine and Drug Discovery#2026-08-13 One-on-One (13 and 20 Aug)|Biomedicine and Drug Discovery]]  
**Notes from this meeting:** [[Creativity Experiments - Key Findings]], [[Creativity Exp 1 - Index and Conclusion]], [[Creativity Exp 2 - Conclusion]], [[Creativity Experiments - Datasets]]

> [!attachment] File in Notion: **creativity-debate-slides (7).html** - [open the Notion page](https://app.notion.com/p/3bbd68da32f080b9b7e7f143d7fa3505)

- continue with two new idea of creativity
- find metrics of creativity 
    - diversity included in slide
    - novelty 
- check that our dataset 9 drug discover works for us or not
- find and check how they evaluate the omni paper
- write how do you want to evaluate the creativity 
- [ ] Add something look like p value and confidence
    - ithink i miss this 

- [ ] todo next
    - current data set do not have long context , change the dataset with more challenging one
    - repeating for new ideas 

#### New Results for AMG-RAG

---

## 7. All notes linked to this direction

### Reports and experiments (6)

- [[Creativity Experiments - Datasets]] - The 4 small datasets used in the creativity experiments: med_ddx (real MedQA), med_repurpose (synthetic), gen_aut (alternative uses), gen_problems (synthetic city...
- [[Creativity Experiments - Key Findings]] - Five main findings from both creativity experiments: identical-context debate fails, surface changes do nothing, persona is the best lever, the aggregator matters...
- [[Creativity Exp 1 - Index and Conclusion]] - Index of both creativity experiments and the full conclusion of Exp 1 (masking and shuffling of context between debating agents).
- [[Creativity Exp 1 - Full Results]] - Full statistics of Exp 1. Masking raised agent diversity; shuffling did nothing; plain debate was worse than one agent.
- [[Creativity Exp 2 - Conclusion]] - Conclusion of Exp 2 (asymmetry levers). Persona works best; representation is a null; the new aggregator helped creativity but lost the correct diagnosis.
- [[Creativity Exp 2 - Full Results]] - Full statistics of Exp 2 (persona, masking, representation, aggregator bottleneck, medical safety).

### Papers (6)

- [[BioDiscoveryAgent]] - Stanford agent that designs gene perturbation experiments. Main metric: cumulative hit ratio over rounds.
- [[EscapeBench]] - Text-only room-escape games to measure creative intelligence of LLM agents. Top models reach only about 15% progress without hints.
- [[GENEDISCO - Creativity View]] - GENEDISCO seen as a creativity test: maximize target discovery rate and model performance.
- [[MUTATE - Divergent Thinking Benchmark]] - Text sandboxes where the obvious solution fails. Measures path-level and action-level divergence of agents. Close to EscapeBench.
- [[PMO - Practical Molecular Optimization]] - Benchmark with 23 molecule objectives and a 10K oracle budget; scores the path too (AUC top-10). Idea: use intermediate rewards in long horizons.
- [[SMDD-Bench]] - 502 medicinal-chemistry tasks over 102 protein targets (pharmacophore, interaction points, scaffold hopping, ...).

### Literature reviews and lists (1)

- [[Review - Creativity Measuring Benchmarks]] - Overview of benchmarks that measure creativity (CreativityBench, MacGyver, DeepMath-Creative, EscapeBench, MUTATE, bio tasks).

