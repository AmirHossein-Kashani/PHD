---
type: topic
aliases: [agent architectures, ReAct, LangGraph, frameworks]
tags: [type/topic, topic/agent-architectures-and-frameworks]
---
# Agent Architectures and Frameworks

> [!summary] In short
> The basic patterns for building LLM agents, and the software used to build them. Patterns: **ReAct** (think, act, observe), **Plan-and-Execute**, **Reflexion** (learn from verbal feedback), **Tree/Graph of Thoughts** (search over thoughts), **CodeAct** (act by writing code), **multi-agent** teams (debate, Virtual Lab). Frameworks: **LangGraph**, LangChain, AutoGen, OpenAI Agents SDK, and Claude tools (skills, MCP, sub-agents).

## Key ideas from my notes

| Pattern | One line | Note |
|---|---|---|
| ReAct | Thought, action, observation loop | [[ReAct]] |
| Plan-and-Execute | Plan first, then solve | [[Plan-and-Execute]] |
| Reflexion | Actor + evaluator + self-reflection memory | [[Reflexion]] |
| Tree / Graph of Thoughts | Generate, refine, aggregate thoughts as a graph | [[Tree of Thoughts and Graph of Thoughts]] |
| CodeAct | Actions are executable code | [[CodeAct]] |
| Multi-agent debate | Agents critique each other | [[Multi-Agent Debate]] |
| Virtual Lab | PI, scientists and critic hold meetings | [[Virtual Lab]] |
| Confucius | Planning DAGs, tool DSLs, RAG memory, human validation | [[Agent Architectures and Frameworks#2026-05-14 Explore CS Talk - Confucius Framework\|2026-05-14 Explore CS Talk - Confucius Framework]] |

- **GoT idea to keep:** separate the *plan of operations* (static) from the *graph of reasoning state* (dynamic) ([[AR 7 - Meeting Log#2026-08-25 One-on-One|2026-08-25 One-on-One]]).
- **Learning:** [[LangGraph]], [[Agentic AI - Learning Path]], [[Claude Skills vs MCP]], [[AutoGen and MetaGPT]].
- **Survey:** [[Review - Agent Architectures for Molecule and Chemistry Design]].

## Where it is used

- [[Early Exploration (May-June 2026)]]
- [[AR 0 - Start Here|Auto Researcher]] (Arbor uses a coordinator + executors)

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-08-25 One-on-One

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-08-25 One-on-One|AR - Meeting Log]], [[Creativity in Multi-Agent Debate#2026-08-25 One-on-One|Creativity in Multi-Agent Debate]], [[Benchmarks and Datasets#2026-08-25 One-on-One|Benchmarks and Datasets]], [[Creativity and Exploration#2026-08-25 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-08-25 One-on-One|MS - Meeting Log]]

#### learned

##### The original GoT representation (still the conceptual anchor)
The Besta et al. GoT paper (AAAI 2024) contributes a representation idea that's more durable than the method itself: it separates two graphs. There's a **Graph of Operations (GoO)** — a static, pre-defined plan of transformations (generate, aggregate, refine, score) — and the **Graph of Reasoning State (GRS)**, the dynamic graph of actual thoughts that results from executing the GoO. This approach enables combining arbitrary LLM thoughts into synergistic outcomes, distilling the essence of whole networks of thoughts, or enhancing thoughts using feedback loops. The practical takeaway: represent your reasoning as *nodes = thoughts/partial solutions, edges = dependencies or transformations*, and keep the plan (operations) separate from the state (thoughts). That separation is exactly what makes it composable for agents. [ACM Digital Library](https://dl.acm.org/doi/10.1609/aaai.v38i16.29720)

### 2026-05-21 Explore CS Talk - PINC

> [!summary] This meeting in short
> Talk notes on PINC-2026: combine user request with background knowledge, generate code, validate code.

*Original: [Notion page](https://app.notion.com/p/367d68da32f080f8825ffbc6625c688c)*

> [!attachment] File in Notion: **PINC-2026.pdf** (372.1 KiB) - [open the Notion page](https://app.notion.com/p/367d68da32f080f8825ffbc6625c688c)

#### Architecture
it contains 3 section
1. combine the user request with background knowledge and system configuration
1. generate  codes
1. validate code 

![[2026-05-21-explore-cs-talk-pinc-1.webp]]

### 2026-05-14 Explore CS Talk - Confucius Framework

> [!summary] This meeting in short
> Talk notes on Confucius (Meta): a multi-agent LLM framework for network management with planning DAGs, tool translation (DSLs), RAG memory and human validation.

*Original: [Notion page](https://app.notion.com/p/360d68da32f08072bc4fea241122f7fa)*

[https://dl.acm.org/doi/epdf/10.1145/3718958.3750537](https://dl.acm.org/doi/epdf/10.1145/3718958.3750537)
[sfu.instructure.com](https://sfu.instructure.com/courses/15302)

Confucius is a **multi-agent LLM framework** organized around five guiding principles: separate reasoning from factual knowledge, leverage existing expertise, orchestrate with engineered prompts, iterate quickly, and avoid dependence on fine-tuning.
The architecture has four interacting components surrounding the LLM agent:
**1. Planning (Workflows)**
Confucius represents network procedures as **directed acyclic graphs (DAGs)** where each node is a reusable building block from Meta's existing codified MOPs. With hundreds of workflows available, a RAG-based indexer/retriever recommends the right ones for a given intent. Two key primitives support this layer: **Ensemble** (runs multiple LLMs in parallel and combines results via first/merge/filter/return-all modes) and **Orchestrator** (lets the LLM autonomously chain building blocks for tasks that don't fit a predefined workflow).
**2. Tools (DSLs)**
Rather than building new tools, Confucius connects to existing ones by translating natural language into the structured inputs those tools require. The framework supports three foundational DSL families that cover most use cases: **topology graphs** (TML), **time series data** (ODS queries), and **network data models** (Robotron). Three primitives handle this translation: **Translator** (natural language → structured output), **Selector** (narrow a large search space to relevant candidates), and **Collector** (resolve ambiguity by asking follow-up questions). Prompt engineering techniques like chain-of-thought, contrastive examples, tool calling, and "code as reasoning" boost translation accuracy.
**3. Memory (RAG)**
Confucius maintains both **short-term memory** (a hierarchical tree of messages within a session, letting child Analects inherit parent context) and **long-term memory** via RAG over external knowledge stores (workflows, network logs, topology databases, wikis). Three RAG flavors are used: Naive RAG for small datasets, Hybrid RAG (similarity search + secondary filtering) for larger ones, and Query Transformations to clean up ambiguous user queries before retrieval.
**4. Human Interface**
Built-in **validation** ties Confucius to existing verification systems — DSL parsers, dry-run modes, external graph validators — and feeds errors back to the LLM for self-correction. For high-stakes operations, the Collector primitive enforces structured human approval. A **privacy redaction service** anonymizes 40 types of sensitive data (IPs, names, device IDs) before content reaches third-party LLMs.

![[2026-05-14-explore-cs-talk-confucius-framework-1.webp]]

## Notes on this topic

### Papers (8)

- [[AutoGen and MetaGPT]] - AutoGen is a Microsoft framework for multi-agent systems; compared with n8n. No need to read more.
- [[ChemCRAFT]] - Small LLMs trained step by step to use chemistry tools. Not my favourite.
- [[CodeAct]] - Agents act by writing executable code instead of JSON tool calls.
- [[Plan-and-Execute]] - First plan the steps, then solve. Very simple; no need to continue.
- [[ReAct]] - Thought, action, observation loop.
- [[Reflexion]] - Actor, evaluator and self-reflection model. The agent learns from verbal feedback stored in memory.
- [[Tree of Thoughts and Graph of Thoughts]] - Reasoning as a tree or a graph of thoughts with generate, refine and aggregate steps.
- [[Virtual Lab]] - AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

### Literature reviews and lists (3)

- [[Review - Agent Architectures for Molecule and Chemistry Design]] - Survey of agentic AI for molecule, DNA and chemistry design since 2024, with a comparison table and the selected papers to read.
- [[Review - Agentic Survey Generated by AI]] - Folder page for AI-generated surveys of agent systems.
- [[Review - Multi-Agent LLM Systems 2024-2026]] - Reading list of top-tier multi-agent LLM papers (MARFT, leader training, OPTAGENT, C2C, ...).

### Learning (2)

- [[Agentic AI - Learning Path]] - Suggested order to learn agents: OpenAI Agents SDK, LangGraph, LangChain, CrewAI and more.
- [[LangGraph]] - LangGraph tutorial notes: typed state, nodes, edges, conditional edges, start and end.

