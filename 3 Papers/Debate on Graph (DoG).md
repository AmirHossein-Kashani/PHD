---
type: paper
date: 2026-06-29
status: In Progress
projects: ["Claim-Level Conflict in Debate", "Multi-Source Evidence RAG"]
topics: ["Neuro-Symbolic Reasoning and Argumentation", "RAG and Knowledge Graphs", "Multi-Agent Debate"]
notion-tags: ["dataset"]
tags: [type/paper, project/claim-conflict, project/multi-source-rag, topic/neuro-symbolic-reasoning-and-argumentation, topic/rag-and-knowledge-graphs, topic/multi-agent-debate]
notion: https://app.notion.com/p/38ed68da32f0806b9a52d1083bcc9abb
created: 2026-06-29
updated: 2026-06-30
---
# Debate on Graph (DoG)

> [!summary] In short
> KG question answering with debate to avoid long reasoning paths and false-positive relations.

**Project:** [[Claim-Level Conflict in Debate]], [[Multi-Source Evidence RAG]]  
**Topics:** [[Neuro-Symbolic Reasoning and Argumentation]], [[RAG and Knowledge Graphs]], [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Neuro-Symbolic Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/38ed68da32f0806b9a52d1083bcc9abb)

---

This paper introduces **DoG (Debate on Graph)**, a framework for getting large language models to answer questions reliably using knowledge graphs—a task called Knowledge Graph Question Answering (KGQA).
**The problem they're solving**
When an LLM answers a question by retrieving facts from a knowledge graph, it follows a chain of connected facts (triples like `<Albert Nobbs, starred_by, Glenn Close>`). The authors point to two recurring failures:
1. **Excessively long reasoning paths** — When you feed the model a long chain of facts all at once, it loses track of what matters. It tends to latch onto a familiar entity at the end of the chain and just guess from its own prior knowledge, producing answers that sound plausible but are wrong.
1. **False-positive relations** — When expanding the path, models gravitate toward relations that look similar to wording in the question (e.g., reusing `starred_by` because the question mentions starring), even when a different relation like `in_language` is what's actually needed. This leads to incomplete or misdirected paths.

**Their two key ideas**
*Subgraph-focusing (answer trying):* Instead of building a complete evidence chain and only then answering, DoG checks after **every single step** whether the one fact just retrieved is enough to answer. This keeps the model focused on one small piece at a time rather than drowning in a long path.
*Multi-role debate to simplify questions:* When a single fact isn't enough, DoG doesn't just keep extending the path—it uses a team of three LLM "agents" who debate to rewrite the question into a simpler form:
- an **expert** proposes a first (often rough) simplification,
- a **critic** points out errors and corrects it,
- a **linguist** strips out leftover redundant information.

Through this, a hard multi-hop question (K hops) is gradually turned into an easier (K-1 hop) one, step by step, until it becomes a simple single-hop question the model can answer directly. This mimics how a person breaks a complex question into manageable pieces, and crucially it *removes* the already-used relation from the simplified question—so the model isn't tempted by those false-positive relations again.
**The payoff**
Across five public datasets, DoG matches or beats prior methods. Their headline numbers: it outperforms the strong baseline ToG by **23.7%** on WebQuestions and **9.1%** on GrailQA in accuracy. They also show it works when plugged into different LLMs (GPT-3.5, GPT-4, Qwen, Llama), arguing the approach is flexible rather than tied to one model.
The core insight, in one sentence: rather than reasoning over one big tangled path, **reason one fact at a time and use a debate among multiple roles to progressively simplify the question**, which makes the whole process both more reliable and more transparent.

![[debate-on-graph-dog-1.webp]]
