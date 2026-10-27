---
type: project-hub
status: done
period: 2026-05 to 2026-06
tags: [type/hub, project/early-exploration, topic/agent-architectures-and-frameworks]
---
# Early Exploration (May-June 2026)

> [!summary] In short
> The first two months of the PhD research plan. The starting question was **"Is AI creative, and how can agent systems be more creative?"**, first in the context of **protein and molecule generation**. I surveyed agent architectures (ReAct, Plan-and-Execute, Reflexion, Tree/Graph of Thoughts, CodeAct, Multi-Agent Debate, Virtual Lab), learned LangGraph, studied GFlowNets and MCMC, tried to run **SAGA**, and read about multi-agent debate and LLM judges. In June the **BC Children** project started and a RAG + KG survey led to **AMG-RAG**, which became the [[Multi-Source Evidence RAG]] project.

---

## 1. What happened, week by week

| Date | What | Notes |
|---|---|---|
| 11 May | First summer meeting: two ideas (whole-sequence prediction + noise for creativity; same experts per query in MoE) | [[Early Exploration (May-June 2026)#2026-05-11 Weekly Meeting (Summer 1)\|2026-05-11 Weekly Meeting (Summer 1)]] |
| 12 May | Plan: mini survey of architectures, choose a base architecture and a real scientific question | [[Early Exploration (May-June 2026)#2026-05-12 One-on-One\|2026-05-12 One-on-One]] |
| 12-17 May | Read ChemCRAFT, Virtual Lab, ReAct, Plan-and-Execute, Reflexion, ToT/GoT, CodeAct, MAD | [[Review - Agent Architectures for Molecule and Chemistry Design]] |
| 13 May | MindMap paper | [[MindMap - KG Prompting for LLMs]] |
| 14, 21 May | Explore CS talks (Confucius, PINC) | [[Agent Architectures and Frameworks#2026-05-14 Explore CS Talk - Confucius Framework\|2026-05-14 Explore CS Talk - Confucius Framework]], [[Agent Architectures and Frameworks#2026-05-21 Explore CS Talk - PINC\|2026-05-21 Explore CS Talk - PINC]] |
| 19 May | Report on agentic architectures; plan: LangGraph, QKV attention, diffusion, MCMC, SAGA | [[Early Exploration (May-June 2026)#2026-05-19 One-on-One (Report)\|2026-05-19 One-on-One (Report)]] |
| 25-26 May | MAPoRL; "how creative are LLMs" (RL explores more) | [[MAPoRL]], [[Creativity and Exploration#2026-05-26 One-on-One\|2026-05-26 One-on-One]] |
| 2 Jun | BC Children meeting planned (4 June); SAGA blocked by Compute Canada account | [[Early Exploration (May-June 2026)#2026-06-02 One-on-One\|2026-06-02 One-on-One]] |
| 4-13 Jun | LLM judges and debate papers; RAG and KG survey | [[Review - State of the Art Multi-Agent Debate]], [[Review - Stance-Aware KG Medical Fact Verification]] |
| 8 Jun | GFlowNet study notes | [[GFlowNet Foundations]] |
| 9 Jun | AMG-RAG chosen as closest to our approach; SAGA must run in the cloud | [[Multi-Agent Debate#2026-06-09 One-on-One\|2026-06-09 One-on-One]] |
| 16 Jun | SAGA run with gpt-mini; KAG; Compute Canada guide | [[Early Exploration (May-June 2026)#2026-06-16 One-on-One\|2026-06-16 One-on-One]], [[How to Start Compute Canada]] |
| 23-29 Jun | Neuro-symbolic debate papers; AMG-RAG paper review | [[Review - Neuro-Symbolic Multi-Agent Debate]], [[AMG-RAG - Paper Review Notes]] |
| 30 Jun | LangGraph course finished; attention steering papers | [[Memory and Long Context#2026-06-30 One-on-One\|2026-06-30 One-on-One]] |

## 2. What came out of it

- The agent-architecture knowledge is now in [[Agent Architectures and Frameworks]].
- The debate reading led to [[Claim-Level Conflict in Debate]].
- The RAG + KG survey led to [[Multi-Source Evidence RAG]].
- The creativity question came back in [[Creativity in Multi-Agent Debate]] and [[Auto Researcher]].

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-06-23 One-on-One

*Original: [Notion page](https://app.notion.com/p/388d68da32f080e0a389d4f9502a1fd1)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-23 One-on-One|Multi-Agent Debate]], [[Neuro-Symbolic Reasoning and Argumentation#2026-06-23 One-on-One|Neuro-Symbolic Reasoning and Argumentation]], [[MS - Meeting Log#2026-06-23 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-23 One-on-One|Skills and Tools]]

#### Week plan
- [ ] check the final result of the SAGA with suplimentary section of SAGA  paper

### 2026-06-16 One-on-One

> [!summary] This meeting in short
> Ran SAGA with gpt-mini to cut cost. KAG paper in progress. Wrote the Compute Canada guide.

*Original: [Notion page](https://app.notion.com/p/381d68da32f080b98331db91fa9ab6a2)*  
*Other parts of this meeting:* [[RAG and Knowledge Graphs#2026-06-16 One-on-One|RAG and Knowledge Graphs]], [[Skills and Tools#2026-06-16 One-on-One|Skills and Tools]]

- [x] Run Saga
    - I have changed the model that being used by SAGA to gpt-mini to reduce cost of api calls

### 2026-06-09 One-on-One

*Original: [Notion page](https://app.notion.com/p/37ad68da32f080bcae3cdec43f9f2229)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-09 One-on-One|Multi-Agent Debate]], [[MS - Meeting Log#2026-06-09 One-on-One|MS - Meeting Log]], [[RAG and Knowledge Graphs#2026-06-09 One-on-One|RAG and Knowledge Graphs]], [[Skills and Tools#2026-06-09 One-on-One|Skills and Tools]]

#### Next week plan
- [ ] Saga implimentation
    - it seems there is not way to run on hpc and we have to use cloud version of it

### 2026-06-02 One-on-One

> [!summary] This meeting in short
> Plan: creativity, BC Children project (meeting on June 4, trying to make it official), LangGraph, communication between agents, SAGA (Compute Canada account issues).

*Original: [Notion page](https://app.notion.com/p/374d68da32f080f1a6f5c6b4bb9b81b0)*  
*Other parts of this meeting:* [[Creativity and Exploration#2026-06-02 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-06-02 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-02 One-on-One|Skills and Tools]], [[Multi-Agent Debate#2026-06-02 One-on-One|Multi-Agent Debate]]

#### Next week plan
- [ ] Saga implimentation
    - [x] we exchange about 10 emails to computer canada and sfu staffs to finally remove my account and let me create new one

### 2026-05-19 One-on-One (Report)

> [!summary] This meeting in short
> Report on agentic LLM architectures. Next week: LangGraph, attention (QKV), debate LLMs, diffusion, MCMC, start coding with SAGA, read MAPoRL.

*Original: [Notion page](https://app.notion.com/p/365d68da32f080cf9859df80c1b1e583)*  
*Other parts of this meeting:* [[Skills and Tools#2026-05-19 One-on-One (Report)|Skills and Tools]], [[Generative Models and Sampling#2026-05-19 One-on-One (Report)|Generative Models and Sampling]]

> [!attachment] File in Notion: **Agentic LLM Architectures_19thMay.pdf** (749 KiB) - [open the Notion page](https://app.notion.com/p/365d68da32f080cf9859df80c1b1e583)

> [!attachment] File in Notion: **Agentic LLM Architectures_19thMay.docx** (1.7 MiB) - [open the Notion page](https://app.notion.com/p/365d68da32f080cf9859df80c1b1e583)

#### next week plan
- [ ] continue in the field of debate llm
- [ ] Start coding with SAGA
    - [x] Running and LLM free on kaggle for API 
        - [AmirHossein kashani](https://www.kaggle.com/amirkashani)
- [ ] [[MAPoRL]]

### 2026-05-13 Disease Detection Meeting

> [!summary] This meeting in short
> Short meeting: read MindMap, learn LangGraph.

*Original: [Notion page](https://app.notion.com/p/35fd68da32f080718c30f48a63e3cb31)*

- [x] Read MindMap:  [[MindMap - KG Prompting for LLMs]] 
- [ ] learn LangGraph [[LangGraph]]

### 2026-05-12 One-on-One

> [!summary] This meeting in short
> Plan: mini survey of agent architectures, choose a base architecture and a real scientific question, learn LangChain and LangGraph. Lists the first papers read.

*Original: [Notion page](https://app.notion.com/p/35fd68da32f080768d09dd4d84b935f8)*

- [x] make a mini survey
    - [x] existing architectures
    - [ ] what is actual creativity ( a presentation in a conferances)
- [ ] choose a base architecture
- [ ] choose a true scientific question
- [ ] learning process
    - [ ] langchain [[LangChain]] 
    - [ ] langGraph [[LangGraph]] 

#### what happened
- [x] [[MindMap - KG Prompting for LLMs]] 
- [x] [[ChemCRAFT]] 
    - done but not my favourite
- [x] [[Agent Architectures and Frameworks#2026-05-14 Explore CS Talk - Confucius Framework|2026-05-14 Explore CS Talk - Confucius Framework]] 
- [x] [[Virtual Lab]] 
- [x] [[ReAct]] 
- [x] [[Plan-and-Execute]] 
- [x] [[Reflexion]] 
- [x] [[Tree of Thoughts and Graph of Thoughts]] 
- [x] [[CodeAct]] 
- [x] [[Multi-Agent Debate - First Notes]] 
- [ ] Learnings
    - [ ] langGraph until 2:52:00 , about 20 min still has

### 2026-05-11 Weekly Meeting (Summer 1)

> [!summary] This meeting in short
> First summer meeting. Two ideas: predict the whole sequence (plus input noise) to get more creative output, and use the same experts for a whole query in mixture-of-experts style systems.

*Original: [Notion page](https://app.notion.com/p/35dd68da32f0808ebea2e5e8c553d78a)*

Two main idea discussed 
1. rather than tryin to predict token by token which eliminate the innovation use all entire sequence prediction + adding some noise to the input can behave as creativeness
1. in MOE such as multi agent systems, idea of using same experts in all entire query which could give us to specifically pick the only experts that we need to train our models

## 3. All notes linked to this period

### Reports and experiments (1)

- [[PhD Progress and Literature Report (July 2026)]] - File only: a progress and literature report that gathers all steps until July 22.

### Papers (15)

- [[AutoGen and MetaGPT]] - AutoGen is a Microsoft framework for multi-agent systems; compared with n8n. No need to read more.
- [[ChemCRAFT]] - Small LLMs trained step by step to use chemistry tools. Not my favourite.
- [[CodeAct]] - Agents act by writing executable code instead of JSON tool calls.
- [[GFlowNet Foundations]] - Long study notes on GFlowNets: a trained sampler that builds objects step by step with probability proportional to a reward. Compared with MCMC.
- [[KAG - Knowledge Augmented Generation]] - Combines an expert-made static KG with dynamic knowledge from context. Planning, reasoning and retrieval loop.
- [[MAPoRL]] - Co-train debating agents with RL and extra rewards so they really cooperate instead of pretending.
- [[MindMap - KG Prompting for LLMs]] - MindMap: extract path-based and neighbor-based evidence subgraphs from a KG, then let the LLM reason over them. Used as a baseline in the multi-source project.
- [[Molecule Generation for Drug Design - Graph Learning View]] - Reading item (no notes yet): molecule generation for drug design from a graph learning view.
- [[Multi-Agent Debate - First Notes]] - First short note on multi-agent debate (same idea as Du et al.).
- [[Plan-and-Execute]] - First plan the steps, then solve. Very simple; no need to continue.
- [[ReAct]] - Thought, action, observation loop.
- [[Reflexion]] - Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.
- [[SAGA]] - SAGA paper files. We tried to run SAGA (Compute Canada, then cloud with gpt-mini).
- [[Tree of Thoughts and Graph of Thoughts]] - Reasoning as a tree or a graph of thoughts with generate, refine and aggregate steps.
- [[Virtual Lab]] - AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

### Literature reviews and lists (3)

- [[Review - Agent Architectures for Molecule and Chemistry Design]] - Survey of agentic AI for molecule, DNA and chemistry design since 2024, with a comparison table and the selected papers to read.
- [[Review - Agentic Survey Generated by AI]] - Folder page for AI-generated surveys of agent systems.
- [[Review - Multi-Agent LLM Systems 2024-2026]] - Reading list of top-tier multi-agent LLM papers (MARFT, leader training, OPTAGENT, C2C, ...).

### Learning (4)

- [[Creativity in AI (folder)]] - Folder page for the question "is AI creative?".
- [[Is AI Really Creative]] - Open question page. Holds the Mila talk note.
- [[MCMC - Markov Chain Monte Carlo]] - Notes on Monte Carlo simulation, Markov chains and MCMC, with formulas.
- [[Rethinking Creativity in the Age of GenAI (Mila talk)]] - Talk note: test creativity with "give 10 unrelated words"; some models beat the median human.

