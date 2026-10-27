---
type: learning
date: 2026-05-07
status: In Progress
topics: ["Skills and Tools", "Agent Architectures and Frameworks"]
tags: [type/learning, topic/skills-and-tools, topic/agent-architectures-and-frameworks]
notion: https://app.notion.com/p/359d68da32f0806e8047d738450ef614
created: 2026-05-07
updated: 2026-05-13
---
# Agentic AI - Learning Path

> [!summary] In short
> Suggested order to learn agents: OpenAI Agents SDK, LangGraph, LangChain, CrewAI and more.

**Topics:** [[Skills and Tools]], [[Agent Architectures and Frameworks]]  
**Notion category:** [[Notion Category - General Skills]]  
**Sub-notes:** [[Agentic AI Course Video (2 hours)]], [[Claude Skills vs MCP]], [[LangGraph]], [[LangChain]]  
**Original:** [Notion page](https://app.notion.com/p/359d68da32f0806e8047d738450ef614)

---

## Youtube
[AI Agents Full Course 2026: Master Agentic AI (2 Hours)](https://www.youtube.com/watch?v=EsTrWCV0Ph4)

### Detail Of Video

### Best learning path
**1. Start with OpenAI Agents SDK — Python**
Watch: **“OpenAI Agents SDK Tutorial (FULL SERIES)”**
Good for learning the core agent concepts: agents, tools, handoffs, and multi-agent workflows. The official OpenAI docs describe agents as apps that can plan, call tools, collaborate, and maintain state across multi-step work.
**2. Then learn LangGraph — production-style Python agents**
Watch: **“LangGraph Complete Course for Beginners – Complex AI Agents”**
LangGraph is a strong choice once you want more control over workflows, state, memory, human-in-the-loop steps, and orchestration.
**3. Learn LangChain agents after that**
Watch: **“LangChain Agents in 2025 | Full Tutorial for v0.3”**
This is useful because LangChain is still common in real projects, and its quickstart focuses on building a functional AI agent quickly.
**4. Learn CrewAI for multi-agent “teams”**
Watch: **CrewAI official beginner material / “Build your First CrewAI Agents”**
CrewAI is useful when you want researcher/writer/coder-style agent teams. Their quickstart builds a flow with an agent that researches and outputs a report.
**5. For JavaScript / TypeScript agents**
Watch: **“Vercel Ship 2025 workshop: Building agents with the AI SDK”** or **“Building An AI Agent in NodeJS using LangChain & LangGraph”**
These are good after Python because many production AI apps use Next.js, TypeScript, and the Vercel AI SDK.

### My recommended order
Start with **OpenAI Agents SDK in Python**, then **LangGraph**, then **CrewAI**, then move to **TypeScript agents**. For a developer, this path is better than starting with no-code tools because you’ll learn the real patterns: tool calling, memory, routing, multi-agent handoffs, state, evaluation, and deployment.
Also keep the official docs open while watching videos: OpenAI’s Agents SDK docs explain when to use the SDK for orchestration, tools, guardrails, tracing, and handoffs, while LangGraph’s quickstart teaches a calculator agent using Python.
