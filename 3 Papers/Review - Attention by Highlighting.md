---
type: review
date: 2026-07-06
date-end: 2026-07-09
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Memory and Long Context"]
tags: [type/review, project/claim-conflict, topic/memory-and-long-context]
notion: https://app.notion.com/p/395d68da32f0807abd96f6349878d71f
created: 2026-07-06
updated: 2026-07-13
---
# Review - Attention by Highlighting

> [!summary] In short
> Papers about making the model pay attention to marked parts of long context (Self-Anchor, SPA, PASTA).

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Sub-notes:** [[PASTA - Attention Steering]], [[Self-Anchor (ATAR)]], [[SPA - Selective Prompt Anchoring]]  
**Original:** [Notion page](https://app.notion.com/p/395d68da32f0807abd96f6349878d71f)

---

- **Self-Anchor / ATAR** — Zhang, Tian & Zhang, arXiv 2510.03223 (2025). This is probably the closest fit. The model decomposes its reasoning into a structured plan, then at each subsequent step it explicitly re-aligns its attention back to the most relevant earlier steps rather than letting them get "buried" in a long context. It beat SOTA prompting baselines on 6 benchmarks and let non-reasoning models match specialized reasoning models. The framing — "critical intermediate steps get buried in the context and receive insufficient attention" — is exactly the problem you're describing, and their fix is essentially self-generated structural markers that persist into later steps.
- **Selective Prompt Anchoring (SPA)** — Tian & Zhang, 2024. Directly reweights logit probabilities to keep emphasis on user- (or self-) specified spans of context across generation. Conceptually the closest "make certain tokens count more in later attention" mechanism.
- **PASTA (Post-hoc Attention STeering Approach)** — Zhang et al., 2023 (ICLR 2024). Identifies specific attention heads and reweights them to redirect focus toward a user-specified span. This is the original "tell the model which part of the input matters more" technique that most later work builds on.
- **Selective Self-Attention (SSA)** — Zhang et al., 2024. Adds temperature scaling to softmax attention so certain tokens get sharper (more concentrated) attention weight — another mechanism for "turning up the volume" on a chosen span.
- **ThoughtFold** — arXiv 2606.03503 (2026). Uses attention scores from the model's own generation to rank which reasoning steps actually mattered for the final answer, then prunes/folds the less-important ones. This is closer to *retrospective* highlighting — the model looks back at its own CoT and marks what was actually load-bearing, which sounds close to what you want for a "next iteration" loop.

- **Self-Anchor / ATAR** (Zhang, Tian & Zhang, 2025) — [https://arxiv.org/abs/2510.03223](https://arxiv.org/abs/2510.03223)
- **Selective Prompt Anchoring (SPA)** (Tian & Zhang, 2024) — [https://arxiv.org/abs/2408.09121](https://arxiv.org/abs/2408.09121)
- **PASTA** (Zhang et al., ICLR 2024) — [https://arxiv.org/abs/2311.02262](https://arxiv.org/abs/2311.02262)
- **Selective Self-Attention (SSA)** — [https://arxiv.org/abs/2411.12892](https://arxiv.org/abs/2411.12892) (titled "Selective Attention: Enhancing Transformer through Principled Context Control")
- **ThoughtFold** (2026) — [https://arxiv.org/abs/2606.03503](https://arxiv.org/abs/2606.03503)

- **Soft/mechanistic**: reweight attention heads or logits toward spans the model flags as important (PASTA, SPA, SSA) — requires access to internals, not just text-level markup.
- **Structural/text-level**: have the model emit explicit structure (plans, step labels, "key finding" tags) as *text*, and re-inject that structure into the next prompt so it acts as a retrieval anchor rather than relying on the model's raw attention over a long undifferentiated context (Self-Anchor/ATAR does this).
