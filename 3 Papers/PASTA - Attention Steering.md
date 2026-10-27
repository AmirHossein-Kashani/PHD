---
type: paper
date: 2026-07-05
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Memory and Long Context"]
tags: [type/paper, project/claim-conflict, topic/memory-and-long-context]
notion: https://app.notion.com/p/395d68da32f080fd9c4dd51681f2172a
created: 2026-07-06
updated: 2026-07-07
---
# PASTA - Attention Steering

> [!summary] In short
> Lets the user mark important text and steers model attention to it, like bold text for humans.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Attention by Highlighting]]  
**Original:** [Notion page](https://app.notion.com/p/395d68da32f080fd9c4dd51681f2172a)

---

#### PASTA — Post-hoc Attention Steering Approach (ICLR 2024)
**The Idea:** When humans write, they use bold or italics to direct a reader's attention to important information — but LLMs have no equivalent mechanism, since they only process plain text. As a result, models often struggle to properly weigh critical instructions, lengthy contextual details, or new facts that conflict with their pretrained knowledge, especially as prompts grow longer or more complex. PASTA addresses this by letting users explicitly mark which part of their input matters most (e.g., an instruction or key fact), and then steering the model's internal attention toward those tokens — mimicking how emphasis works for human readers, but operating at the level of the model's attention mechanism rather than the text itself. The paper shows that simply adding markdown-style emphasis markers to the prompt (like asterisks or quotes) barely works, since LLMs rarely encounter such cues meaningfully during pretraining — motivating a deeper, mechanistic solution.
**The Innovation:** PASTA's key contribution is a **post-hoc, training-free intervention**: at inference time, it multiplies the attention scores of non-highlighted tokens by a small scaling factor (α ≈ 0.01) in a carefully chosen subset of attention heads, then renormalizes — effectively amplifying the relative attention paid to user-specified tokens without altering any model weights or requiring gradients. The second and equally important innovation is **multi-task model profiling**: since different attention heads encode different types of information, blindly steering all heads degrades performance. PASTA instead runs a one-time search across several tasks to identify a small set of heads (roughly 5–15% of all heads) that reliably improve performance when steered, and this head-set becomes a reusable "profile" for the model that generalizes to unseen tasks. Tested on LLaMA-7B and GPT-J-6B, PASTA outperformed zero-shot, marker-based, and even few-shot prompting across instruction-following, long-context, and knowledge-conflict tasks — while also reducing sensitivity to prompt rephrasing. Its main limitation is that it requires white-box access to a model's attention scores, meaning it works on open-weight models (LLaMA, GPT-J, Vicuna) but not closed API-based models.
