---
type: learning
date: 2026-05-14
date-end: 2026-06-11
status: In Progress
topics: ["Skills and Tools", "Agent Architectures and Frameworks"]
tags: [type/learning, topic/skills-and-tools, topic/agent-architectures-and-frameworks]
notion: https://app.notion.com/p/35dd68da32f0805fb3e6f2477f7da8fd
created: 2026-05-11
updated: 2026-06-11
---
# LangGraph

> [!summary] In short
> LangGraph tutorial notes: typed state, nodes, edges, conditional edges, start and end.

**Topics:** [[Skills and Tools]], [[Agent Architectures and Frameworks]]  
**Notion category:** [[Notion Category - General Skills]]  
**Part of:** [[Agentic AI - Learning Path]]  
**Original:** [Notion page](https://app.notion.com/p/35dd68da32f0805fb3e6f2477f7da8fd)

---

### [LangGraph Complete Course for Beginners – Complex AI Agents with Python](https://www.youtube.com/watch?v=jGg_1h0qzaM)

## Lane Graph Tutorial

## Type Annotations

### Dictionary

![[langgraph-1.webp]]

We are not sure about structure => logical error and can’t scalable
To solve it we can use typed Dict

![[langgraph-2.webp]]

### Union
It is flexible and nice

![[langgraph-3.webp]]

### Optional

![[langgraph-4.webp]]

### Any
Simple as you can see

![[langgraph-5.webp]]

### Lambda function
Simplified the simple implimentations

![[langgraph-6.webp]]

## Session 2

### State
is a shared data structure hold the current information
Each nodes do specific job, and the graph connect them to gether.
It visually represent the workflosh and show the sequence
Edges are the connections work as train track

### Conditional edges
Conditional choose which node to process, decides the next step look like the traffic light

### Start point
The node the process start, it is not necessary do a job but it is the start of process

### End point
The graph execution will stop, and signifies the conclusion of the workflow

### Tools
Specifical facilities

#### Tool node
Special kind of node that the main job is to run a tool
Other nodes can use this

### State Graph
Is a class in lange graph to build and structred the graph
Structure and flow defined in this

### Runnable
Runnable can be a block of Legos
