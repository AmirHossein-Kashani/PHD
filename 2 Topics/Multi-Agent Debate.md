---
type: topic
aliases: [MAD, multi-agent debate, debate]
tags: [type/topic, topic/multi-agent-debate]
---
# Multi-Agent Debate

> [!summary] In short
> **Multi-agent debate (MAD):** several LLM agents answer the same question, read each other's answers, and revise over a few rounds. A final step (vote, judge, or aggregator) gives the answer. It can improve factuality and reasoning, but it can also **collapse into fast agreement**, cost many tokens, and hide *which* claims really conflict.

## Key ideas from my notes

- **Original MAD** ([[Du et al. - Multiagent Debate]]): copies of one model debate until they agree.
- **Different models, confidence voting** ([[ReConcile]]); **layers of proposers and aggregators** ([[MoA - Mixture-of-Agents|MoA]]).
- **Control the debate:** a moderator with two dials ([[MACI - Moderator with Two Dials|MACI]]); run debate only when useful ([[iMAD - Intelligent Multi-Agent Debate|iMAD]]); dynamic discussion acts ([[ACE - Analyze Compose Execute Debate|ACE]]); stop when stable ([[MAD for LLM Judges (Adaptive Stability)]]).
- **Who speaks last has more power** ([[Key Decision-Makers in Multi-Agent Debate]]).
- **Train agents to cooperate** with RL ([[MAPoRL]]).
- **Debate with structure:** argument trees ([[ArgLLM]], [[MArgE]]), solvers ([[MAD-Logic]]), knowledge graphs ([[Debate on Graph (DoG)]], [[DoM - Debate over Mixed-knowledge|DoM]]).
- **My findings** ([[Creativity in Multi-Agent Debate]]): identical context makes agents agree too fast; persona gives useful diversity; the aggregator is the bottleneck.
- **My gap idea** ([[Claim-Level Conflict in Debate]]): find and resolve conflicts claim by claim, not answer by answer.

## Where it is used

- [[Claim-Level Conflict in Debate]] (main topic)
- [[Creativity in Multi-Agent Debate]]
- [[Multi-Source Evidence RAG]] (idea: one agent per paper to aggregate evidence)
- [[AR 0 - Start Here|Auto Researcher]] (multiple agents, diversity)

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-06-23 One-on-One

> [!summary] This meeting in short
> Read 4 neuro-symbolic debate papers and reviewed the AMG-RAG paper. Plan: benchmarks for group thinking of LLMs, check SAGA results.

*Original: [Notion page](https://app.notion.com/p/388d68da32f080e0a389d4f9502a1fd1)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-23 One-on-One|Early Exploration (May-June 2026)]], [[Neuro-Symbolic Reasoning and Argumentation#2026-06-23 One-on-One|Neuro-Symbolic Reasoning and Argumentation]], [[MS - Meeting Log#2026-06-23 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-23 One-on-One|Skills and Tools]]

> [!attachment] File in Notion: **weekly_report_2026-06-30.pdf** (153.5 KiB) - [open the Notion page](https://app.notion.com/p/388d68da32f080e0a389d4f9502a1fd1)

#### Week plan

- [ ] finalize and run some benchmark about group thinking of LLMs and start process of running them

### 2026-06-09 One-on-One

> [!summary] This meeting in short
> Read 5 debate papers. SAGA cannot run on HPC (use cloud). RAG study. Retrieval survey: AMG-RAG chosen as closest to our approach; MedGraphRAG too complex; GraphCheck uses GNN.

*Original: [Notion page](https://app.notion.com/p/37ad68da32f080bcae3cdec43f9f2229)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-09 One-on-One|Early Exploration (May-June 2026)]], [[MS - Meeting Log#2026-06-09 One-on-One|MS - Meeting Log]], [[RAG and Knowledge Graphs#2026-06-09 One-on-One|RAG and Knowledge Graphs]], [[Skills and Tools#2026-06-09 One-on-One|Skills and Tools]]

> [!attachment] File in Notion: **Datasset and SOTA.pdf** (410 KiB) - [open the Notion page](https://app.notion.com/p/37ad68da32f080bcae3cdec43f9f2229)

#### Next week plan
- [x] Continue on judgment and find benchmark and state of the art for LLMs
    - Papers done
        - [[Key Decision-Makers in Multi-Agent Debate]] 
        - [[DoM - Debate over Mixed-knowledge]] 
        - [[ACE - Analyze Compose Execute Debate]] 
        - [[AutoGen and MetaGPT]] (no need to read)
        - [[iMAD - Intelligent Multi-Agent Debate]]

### 2026-06-02 One-on-One

*Original: [Notion page](https://app.notion.com/p/374d68da32f080f1a6f5c6b4bb9b81b0)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-02 One-on-One|Early Exploration (May-June 2026)]], [[Creativity and Exploration#2026-06-02 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-06-02 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-02 One-on-One|Skills and Tools]]

#### Next week plan
- [ ] communication between agents
    - [x] [[MAD for LLM Judges (Adaptive Stability)]] 
    - [x] [[GFlowNet Foundations]]

## Notes on this topic

### Reports and experiments (7)

- [[Creativity Experiments - Key Findings]] - Five main findings from both creativity experiments: identical-context debate fails, surface changes do nothing, persona is the best lever, the aggregator matters...
- [[Creativity Exp 1 - Index and Conclusion]] - Index of both creativity experiments and the full conclusion of Exp 1 (masking and shuffling of context between debating agents).
- [[Creativity Exp 1 - Full Results]] - Full statistics of Exp 1. Masking raised agent diversity; shuffling did nothing; plain debate was worse than one agent.
- [[Creativity Exp 2 - Conclusion]] - Conclusion of Exp 2 (asymmetry levers). Persona works best; representation is a null; the new aggregator helped creativity but lost the correct diagnosis.
- [[Creativity Exp 2 - Full Results]] - Full statistics of Exp 2 (persona, masking, representation, aggregator bottleneck, medical safety).
- [[Gaps Protocol - Claim Conflict Proposal]] - Proposal: debate systems vote on whole answers and miss which claims conflict. Idea: extract claims, detect and classify conflicts, build a claim-conflict graph,...
- [[Suggested Papers for Gaps Protocol]] - Reading list in tiers A to E for the claim-conflict proposal (SMADE-IE, MACI, MAD-Fact, DebateCV, argumentation theory, baselines).

### Papers (18)

- [[ACE - Analyze Compose Execute Debate]] - Dynamic debate: agents choose the next discussion act (sense-plan-act) instead of a fixed number of rounds.
- [[AutoResearchClaw]] - 23-stage idea-to-paper pipeline with PROCEED / REFINE / PIVOT and a lesson store. Judged by an LLM.
- [[Communication Strategy (draft)]] - Unfinished note on communication strategies between agents (one image).
- [[Debate on Graph (DoG)]] - KG question answering with debate to avoid long reasoning paths and false-positive relations.
- [[DoM - Debate over Mixed-knowledge]] - For incomplete KGs: a KG agent and a text (RAG) agent work in parallel and a judge decides.
- [[Du et al. - Multiagent Debate]] - The original multi-agent debate paper: several LLM copies answer, read each other and revise until they agree. Main baseline for debate work.
- [[Google AI Co-Scientist]] - Generate, debate, Elo-rank and evolve biomedical hypotheses (Nature 2026). No public code; does not run its own experiments.
- [[iMAD - Intelligent Multi-Agent Debate]] - Runs debate only when it will likely help, using a self-critique and a classifier. Saves tokens.
- [[Key Decision-Makers in Multi-Agent Debate]] - Speaking order matters: agents who speak later have more power over the final answer.
- [[MACI - Moderator with Two Dials]] - A moderator controls debate with an information dial (evidence quality) and a behavior dial (how agents interact).
- [[MAD for LLM Judges (Adaptive Stability)]] - Several LLM judges debate and stop when their answers become stable (adaptive stopping).
- [[MAD-Logic]] - Symbolic solvers and LLM solvers debate in a loop for logical reasoning, with pruning of low-confidence talk.
- [[MAPoRL]] - Co-train debating agents with RL and extra rewards so they really cooperate instead of pretending.
- [[MArgE]] - Debate version of ArgLLM: LLMs build pro/con argument trees that are merged and scored. Only binary decisions.
- [[MoA - Mixture-of-Agents]] - Layers of proposer LLMs and aggregator LLMs. Agents work separately and aggregators control the flow.
- [[Multi-Agent Debate - First Notes]] - First short note on multi-agent debate (same idea as Du et al.).
- [[ReConcile]] - Round-table discussion between different LLMs with confidence-weighted voting. Strong answer-level aggregation baseline.
- [[SMADE-IE]] - Sparse multi-agent debate with evidence for zero-shot information extraction (entities and relations). Good trick, but a different problem.

### Literature reviews and lists (5)

- [[Review - Baselines for Gaps Protocol]] - Which baselines to use for the claim-conflict work (Du et al., ReConcile, ...). Also the folder for later research-agent baselines.
- [[Review - MAD Benchmarks for Complex Tasks]] - Which benchmarks MAD papers use, and which complex but gradable tasks could be used instead.
- [[Review - Multi-Agent LLM Systems 2024-2026]] - Reading list of top-tier multi-agent LLM papers (MARFT, leader training, OPTAGENT, C2C, ...).
- [[Review - Neuro-Symbolic Multi-Agent Debate]] - Literature review of neuro-symbolic methods in multi-agent debate: argumentation, symbolic solvers as judges, KG grounding.
- [[Review - State of the Art Multi-Agent Debate]] - Long review of the state of the art in multi-agent debate and KG reasoning (ToG, RoG, ...), benchmarks and SOTA results.

