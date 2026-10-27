---
type: paper
date: 2026-07-06
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Memory and Long Context"]
tags: [type/paper, project/claim-conflict, topic/memory-and-long-context]
notion: https://app.notion.com/p/395d68da32f080c1a174cb480f746c04
created: 2026-07-06
updated: 2026-07-07
---
# Self-Anchor (ATAR)

> [!summary] In short
> Interleaves plan and action steps and steers attention back to the question and the current plan step.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Attention by Highlighting]]  
**Original:** [Notion page](https://app.notion.com/p/395d68da32f080c1a174cb480f746c04)

---

[https://arxiv.org/abs/2510.03223](https://arxiv.org/abs/2510.03223)
**1. Interleaved plan-action structure with dual anchoring**
Rather than generating a full plan up front and then executing steps far later (as in Tree-of-Thought, where actions get separated from their plan by lots of intervening context), ATAR interleaves planning and acting step-by-step, and at each action step steers attention to *both* Q and the current plan step sₜ simultaneously — balancing global coherence with local focus.
**2. Confidence-driven dynamic steering strength**
Instead of a static steering intensity, ATAR modulates steering strength per-step using the **harmonic mean of token probabilities** as a confidence proxy — steering harder when the model is uncertain, and backing off when it's already confident. The harmonic mean is chosen deliberately because it's disproportionately sensitive to low-probability tokens, which the authors argue are the best signal of local reasoning instability.
