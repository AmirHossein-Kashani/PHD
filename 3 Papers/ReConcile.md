---
type: paper
date: 2026-07-17
date-end: 2026-07-20
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/3a3d68da32f080ebb41ad228b3b07691
created: 2026-07-20
updated: 2026-07-21
---
# ReConcile

> [!summary] In short
> Round-table discussion between different LLMs with confidence-weighted voting. Strong answer-level aggregation baseline.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3a3d68da32f080ebb41ad228b3b07691)

---

-  giving some sample to the model to teach them how to convince each other
- they only see the previous layer of conversation
- the model evaluate the confidence of them selves

**ReConcile (Chen et al., 2024)** is a multi-agent reasoning framework that holds a "round-table discussion" between *different* LLMs (e.g., ChatGPT, Bard, Claude2) rather than multiple copies of the same model, on the premise that model diversity yields more varied solutions and genuine external feedback. It runs in three phases:
**1. Initial response generation:** Each agent independently answers the question with a chain-of-thought explanation and a self-reported confidence score (0–1).
**2. Multi-round discussion:** The framework runs up to R discussion rounds (R = 3 in the paper). In each round, every agent receives a tailored *discussion prompt* built from three components:
- *Grouped responses:* All agents' answers and explanations from the previous round, organized by stance rather than simply concatenated — e.g., "2 agents think the answer is Yes (with these explanations); 1 agent thinks No (with this explanation)." This grouping makes the state of the disagreement easier for the model to parse, and ablations show it contributes to performance on its own.
- *Confidence scores:* Each answer is accompanied by its agent's stated confidence, so agents can weigh a hesitant dissent (e.g., 40% confident) differently from a firm one (95% confident). Confidence is estimated by directly prompting each model to verbally quantify its uncertainty, since the API-based models are black boxes.
- *Convincing samples:* Four few-shot demonstrations per agent, each consisting of a question, gold answer, and a human-written explanation that was verified to flip that agent's initially wrong answer to the correct one. These are selected via this answer-rectification criterion (not just any human explanation), and each agent is shown the convincing samples of the *other* agents, since the goal is to persuade them. In-context, these teach agents to produce genuinely persuasive explanations rather than merely restating their positions.

Conditioned on this prompt, each agent outputs a revised answer, explanation, and confidence, which feed into the next round. The discussion terminates early once all agents converge on the same answer. Notably, without convincing samples agents tend to entrench in their initial positions and fail to converge; with them, initially wrong agents actually update their answers. For datasets lacking human explanations, ReConcile still works (and still beats debate baselines) by simply omitting this component.
**3. Team answer generation:** At the end of each round, a team answer is computed via a confidence-weighted vote: each agent's vote for its answer is weighted by a *recalibrated* version of its confidence score, and the answer with the highest total weight wins. Recalibration is needed because LLMs are systematically overconfident — raw scores cluster near 1.0, making them poorly discriminative. The paper uses a simple fixed rescaling function that maps confidence bands to spread-out weights (e.g., 1.0 → 1.0, [0.9, 1.0) → 0.8, [0.8, 0.9) → 0.5, (0.6, 0.8) → 0.3, else 0.1). The same mapping works out-of-the-box across all seven tasks and reduces Expected Calibration Error substantially. This weighted vote outperforms both a simple majority vote and picking the single most confident agent's answer.
