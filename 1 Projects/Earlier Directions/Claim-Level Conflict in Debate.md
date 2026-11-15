---
type: project-hub
status: paused
period: 2026-06 to 2026-07
tags: [type/hub, project/claim-conflict, topic/multi-agent-debate]
---
# Claim-Level Conflict in Debate

> [!summary] In short
> Idea from July 2026 (the "Gaps protocol"). Multi-agent debate systems usually **vote on whole answers**. They do not find **which claims** conflict, and they treat all conflict types the same. Proposal: extract claims from each agent, detect and classify conflicts (factual, evidential, definitional, logical, reasoning-path, value), build a **claim-conflict graph**, and resolve each conflict with the right tool (retrieval, solver, clarification, decision protocol).

Status: **paused**. On 27 July we decided to stay close to MAD and MoA, and in August the focus moved to research agents ([[Reversible Exploration (ARIADNE and Retro-Fork)]], then [[AR 0 - Start Here|Auto Researcher]]). The conflict ideas live on in the **contradiction layer** of [[Multi-Source Evidence RAG]].

Main notes: [[Gaps Protocol - Claim Conflict Proposal]], [[Suggested Papers for Gaps Protocol]], [[Review - Baselines for Gaps Protocol]], meetings [[Claim-Level Conflict in Debate#2026-07-07 One-on-One|2026-07-07 One-on-One]] and [[Claim-Level Conflict in Debate#2026-07-14 One-on-One|2026-07-14 One-on-One]].

---

## 1. The gap

- Current limit: majority vote or answer-level aggregation.
- Main gap: no detection of **which specific claims conflict** (for non-binary decisions), and too much unneeded information is passed around.
- Extra gap: different conflict types need different solutions.
- Bottleneck: reliable claim extraction and conflict detection in long, messy debate transcripts.
- Planned contributions: a conflict taxonomy and annotated benchmark; a cross-agent conflict detector; claim-level conflict-aware aggregation.

## 2. My own idea (from the proposal page)

- Agents **tag** their claims and give a separate final statement.
- A **manager** finds conflicts and starts small local conversations between the agents involved. It picks only the related data, to save compute.
- Long context can be handled by the manager reading sections by tag, and by **attention highlighting** (bold regions) - see [[Review - Attention by Highlighting]].
- Final goal: an output-augmentation protocol and a conversation architecture that fits the system.

## 3. Baselines chosen

- [[Du et al. - Multiagent Debate]] - the classic baseline (majority vote).
- [[ReConcile]] - strongest answer-level aggregation (confidence-weighted voting).
- [[MACI - Moderator with Two Dials|MACI]] - moderator controls information flow.
- [[SMADE-IE]] - evidence-driven debate (nearest structure, but for information extraction).
- [[MoA - Mixture-of-Agents|MoA]].

## 4. Related reading

- State of the art: [[Review - State of the Art Multi-Agent Debate]], [[iMAD - Intelligent Multi-Agent Debate|iMAD]], [[ACE - Analyze Compose Execute Debate|ACE]], [[Key Decision-Makers in Multi-Agent Debate]], [[DoM - Debate over Mixed-knowledge|DoM]].
- Neuro-symbolic: [[Review - Neuro-Symbolic Multi-Agent Debate]], [[ArgLLM]], [[MArgE]], [[MAD-Logic]], [[Debate on Graph (DoG)]].
- Benchmarks: [[Review - MAD Benchmarks for Complex Tasks]].

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-07-21 One-on-One

> [!summary] This meeting in short
> Ran the BC Children baseline. Read AlphaEvolve. References for FunSearch and AlphaEvolve.

*Original: [Notion page](https://app.notion.com/p/3a4d68da32f080caaa03d4375e78da67)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-07-21 One-on-One|MS - Meeting Log]], [[Autonomous Research Agents#2026-07-21 One-on-One|Autonomous Research Agents]]

- [ ] Run you first idea about the paper
- [x] [[AlphaEvolve]]

### 2026-07-14 One-on-One

> [!summary] This meeting in short
> Running AMG-RAG. Found baselines for the Gaps protocol: SMADE-IE, ReConcile, MACI.

*Original: [Notion page](https://app.notion.com/p/39ed68da32f08065ba4ed44bdd4dea9d)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-07-14 One-on-One|MS - Meeting Log]]

- [ ] finding base line for the [[Gaps Protocol - Claim Conflict Proposal]] 
    - [x] [[SMADE-IE]] 
    - [x] [[ReConcile]] 
    - [x] [[MACI - Moderator with Two Dials]]

### 2026-07-07 One-on-One

> [!summary] This meeting in short
> Ran the base tree without KG, found the research gap and direction, met Youjen about a KG idea. Sub-notes: Gaps protocol and suggested papers.

*Original: [Notion page](https://app.notion.com/p/397d68da32f080a5a40ef0e312f7e322)*  
**Notes from this meeting:** [[Gaps Protocol - Claim Conflict Proposal]], [[Suggested Papers for Gaps Protocol]]

- [x] run the base tree with out KG
- [x] find the GAP and direction of research
- [x] Meeting youjen and idea for  KG

## 5. All notes linked to this direction

### Reports and experiments (3)

- [[PhD Progress and Literature Report (July 2026)]] - File only: a progress and literature report that gathers all steps until July 22.
- [[Gaps Protocol - Claim Conflict Proposal]] - Proposal: debate systems vote on whole answers and miss which claims conflict. Idea: extract claims, detect and classify conflicts, build a claim-conflict graph,...
- [[Suggested Papers for Gaps Protocol]] - Reading list in tiers A to E for the claim-conflict proposal (SMADE-IE, MACI, MAD-Fact, DebateCV, argumentation theory, baselines).

### Papers (20)

- [[ACE - Analyze Compose Execute Debate]] - Dynamic debate: agents choose the next discussion act (sense-plan-act) instead of a fixed number of rounds.
- [[AlphaEvolve]] - Evolutionary coding agent: LLMs propose program changes, automatic evaluation selects survivors. Creativity comes from a pool of ideas that keeps improving.
- [[ArgLLM]] - Builds a tree of supporting and attacking arguments, scores them, and uses a formula to accept or reject a claim.
- [[Communication Strategy (draft)]] - Unfinished note on communication strategies between agents (one image).
- [[Debate on Graph (DoG)]] - KG question answering with debate to avoid long reasoning paths and false-positive relations.
- [[DoM - Debate over Mixed-knowledge]] - For incomplete KGs: a KG agent and a text (RAG) agent work in parallel and a judge decides.
- [[Du et al. - Multiagent Debate]] - The original multi-agent debate paper: several LLM copies answer, read each other and revise until they agree. Main baseline for debate work.
- [[iMAD - Intelligent Multi-Agent Debate]] - Runs debate only when it will likely help, using a self-critique and a classifier. Saves tokens.
- [[Key Decision-Makers in Multi-Agent Debate]] - Speaking order matters: agents who speak later have more power over the final answer.
- [[MACI - Moderator with Two Dials]] - A moderator controls debate with an information dial (evidence quality) and a behavior dial (how agents interact).
- [[MAD for LLM Judges (Adaptive Stability)]] - Several LLM judges debate and stop when their answers become stable (adaptive stopping).
- [[MAD-Logic]] - Symbolic solvers and LLM solvers debate in a loop for logical reasoning, with pruning of low-confidence talk.
- [[MAPoRL]] - Co-train debating agents with RL and extra rewards so they really cooperate instead of pretending.
- [[MArgE]] - Debate version of ArgLLM: LLMs build pro/con argument trees that are merged and scored. Only binary decisions.
- [[MoA - Mixture-of-Agents]] - Layers of proposer LLMs and aggregator LLMs. Agents work separately and aggregators control the flow.
- [[PASTA - Attention Steering]] - Lets the user mark important text and steers model attention to it, like bold text for humans.
- [[ReConcile]] - Round-table discussion between different LLMs with confidence-weighted voting. Strong answer-level aggregation baseline.
- [[Self-Anchor (ATAR)]] - Interleaves plan and action steps and steers attention back to the question and the current plan step.
- [[SMADE-IE]] - Sparse multi-agent debate with evidence for zero-shot information extraction (entities and relations). Good trick, but a different problem.
- [[SPA - Selective Prompt Anchoring]] - Fixes "attention dilution" in long code generation by keeping attention on the user prompt.

### Literature reviews and lists (7)

- [[Review - Attention by Highlighting]] - Papers about making the model pay attention to marked parts of long context (Self-Anchor, SPA, PASTA).
- [[Review - Baselines for Gaps Protocol]] - Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.
- [[Review - LLM Evaluation Metrics]] - Table of metrics: accuracy, F1, Brier score, calibration, agreement and more, with notes on how to report them.
- [[Review - MAD Benchmarks for Complex Tasks]] - Which benchmarks MAD papers use, and which complex but gradable tasks could be used instead.
- [[Review - Multi-Agent LLM Systems 2024-2026]] - Reading list of top-tier multi-agent LLM papers (MARFT, leader training, OPTAGENT, C2C, ...).
- [[Review - Neuro-Symbolic Multi-Agent Debate]] - Literature review of neuro-symbolic methods in multi-agent debate: argumentation, symbolic solvers as judges, KG grounding.
- [[Review - State of the Art Multi-Agent Debate]] - Long review of the state of the art in multi-agent debate and KG reasoning (ToG, RoG, ...), benchmarks and SOTA results.

