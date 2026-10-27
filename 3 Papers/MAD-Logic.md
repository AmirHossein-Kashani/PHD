---
type: paper
date: 2026-06-29
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Neuro-Symbolic Reasoning and Argumentation", "Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/neuro-symbolic-reasoning-and-argumentation, topic/multi-agent-debate]
notion: https://app.notion.com/p/38ed68da32f08063bfabce9b0314e755
created: 2026-06-29
updated: 2026-06-29
---
# MAD-Logic

> [!summary] In short
> Symbolic solvers and LLM solvers debate in a loop for logical reasoning, with pruning of low-confidence talk.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Neuro-Symbolic Reasoning and Argumentation]], [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Neuro-Symbolic Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/38ed68da32f08063bfabce9b0314e755)

---

The hole idea is that we run symbolic Solvers and NL solvers and then in a loop they try to solve the problem and also have pruning mechanism that reduce the unnessesary conversation or low confidence

![[mad-logic-1.webp]]

**MAD-Logic: Multi-Agent Debate for Logical Reasoning (ICLR 2026)**
**Core Problem**
LLMs struggle with complex logical reasoning. Prior work splits into two camps, each with a fatal flaw:
- **Symbolic (NL→SL + solver):** rigorous but brittle to translation errors
- **Neural (LLM direct reasoning):** robust but hallucinates

**Key Idea**
Let multiple agents — each representing a different reasoning paradigm — debate each other, combining their complementary strengths through majority vote.
**Two-Stage Debate**
1. **Translation Stage:** Multiple LLMs translate the same problem into different symbolic languages (LP, FOL, SAT) in parallel, then debate to cross-correct each other's translations before handing off to solvers (Pyke, Prover9, Z3)
1. **Reasoning Stage:** Solver trace outputs are verbalized as natural language narratives, then all agents (symbolic + NL) debate in a shared natural language space for N rounds, majority vote decides final answer

**The Bridge Problem** (how symbolics "talk")
Solver execution traces are rendered as structured NL narratives by an LLM wrapper. The solver itself is stateless — only its LLM wrapper gets updated between rounds, either by revising the translation (stage 1) or reinterpreting the trace (stage 2). If a solver fails, the wrapper simulates its reasoning style via LLM.
**Efficiency Fix: Sparse Communication Gate**
Full debate is expensive. They prune unnecessary agent interactions using a preference score:
`P(i→j) = C_i/C_j + λ(1 - cos(A_i, A_j))`
- **C_i/C_j:** prioritize communication from more confident to less confident agents
- **1 - cos(...):** prioritize when agents disagree (high information gain)
- Gate opens only if current score ≥ historical average between that pair

Result: 13-36% token savings while actually **improving** accuracy over full communication, because it filters redundant/noisy exchanges.
**Why it works theoretically**
Majority vote accuracy is bounded below by a function of agent correlation ρ. Since LP/FOL/SAT/COT/P&S agents make structurally different errors, their correlation is low enough that the ensemble reliably outperforms any individual.
**Results**
Hits near-perfect accuracy on synthetic benchmarks (ProntoQA, ProofWriter, LogiDeduct) across GPT-4, Claude 3.7, DeepSeek-V3. Meaningful gains on real-world benchmarks (AR-LSAT, FOLIO, Chinese LogiQA-V2). Scales down to 7B models.
**Key Takeaway for Your Notes**
The paper's real contribution is the **interface design**: using natural language as a universal debate medium that symbolic and neural agents can both participate in, with the LLM wrapper acting as a translator between the formal and informal worlds. Sparsity then makes this practical.

---
