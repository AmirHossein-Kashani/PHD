---
type: paper
date: 2026-06-04
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate", "Evaluation and Metrics"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate, topic/evaluation-and-metrics]
notion: https://app.notion.com/p/377d68da32f080d0ad19e471123dda4e
created: 2026-06-06
updated: 2026-06-08
---
# MAD for LLM Judges (Adaptive Stability)

> [!summary] In short
> Several LLM judges debate and stop when their answers become stable (adaptive stopping).

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]], [[Evaluation and Metrics]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Multi-Agent LLM Systems 2024-2026]]  
**Original:** [Notion page](https://app.notion.com/p/377d68da32f080d0ad19e471123dda4e)

---

[Multi-Agent Debate for LLM Judges with Adaptive Stability Detection](https://arxiv.org/abs/2510.12697)

> [!attachment] File in Notion: **NeurIPS-2025-multi-agent-debate-for-llm-judges-with-adaptive-stability-detection-Paper-Conference (1).pdf** (611.6 KiB) - [open the Notion page](https://app.notion.com/p/377d68da32f080d0ad19e471123dda4e)

### Multi-Agent Debate — Decision Process
**Setup:** *n* agents (paper uses 7), each an LLM judge. A function `e(·)` extracts each agent's final verdict (e.g. "1" or "2") from its reasoning text. Max rounds *T* = 10.

#### Step 1 — Round 0: Initial responses
- Each agent independently reads the task **x**.
- Each produces an initial judgment, with no knowledge of other agents.
- These form the round-0 response set.

#### Step 2 — Debate rounds (t = 1, 2, …)
- Each agent now sees the task **plus** all other judges' reasoning from the previous round.
- Each agent generates a **new** judgment, refining its view in light of the others.
- (Internally: this is modeled as Bayesian belief updating toward the "true concept" — agents shift toward the interpretation best supported by the collective reasoning.)

#### Step 3 — Check consensus after each round
- Compute the group's current verdicts.
- **If all agents agree (unanimous) → STOP and return that answer.**
- If not → continue to the next round.

#### Step 4 — Termination
- Repeat Steps 2–3 until either:
    - unanimity is reached (return consensus), **or**
    - the max round *T* = 10 is hit → **return the majority vote** of the final round.

#### Final decision rule
> **Unanimous consensus if agents converge; otherwise majority vote at the final round.**
The novelty isn't the aggregation rule — it's that the votes have been refined through debate, so agents usually converge strongly by the end (distributions collapse to "0 or 7 correct").

---

#### Separate layer — Adaptive Stopping (Algorithm 2)
*This decides WHEN to stop, not WHAT the answer is.*
- Model judge accuracy across rounds as a **time-varying mixture of two Beta-Binomial distributions** (parameters fit via EM).
- Track the **Kolmogorov–Smirnov (KS) statistic** between consecutive rounds' distributions.
- When KS &lt; 0.05 for **2 consecutive rounds** → distribution has stabilized → halt early to save computation.

| Mechanism | Question it answers | Rule |
|---|---|---|
| Algorithm 1 (Debate) | *What is the verdict?* | Unanimity, else majority vote |
| Algorithm 2 (Stability) | *Have we debated enough?* | KS &lt; 0.05 for 2 rounds |

---

#### ⚠️ Caveat to remember
Debate **amplifies the majority direction** — right or wrong. If agents share a bias and start mostly wrong, debate can push them to unanimous agreement on the *wrong* answer (hence the "0 or 7" all-or-nothing failure pattern). Gains concentrate on high-variance tasks; majority voting can match or beat debate on simple ones.
