---
type: paper
date: 2026-07-07
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Memory and Long Context"]
tags: [type/paper, project/claim-conflict, topic/memory-and-long-context]
notion: https://app.notion.com/p/396d68da32f080afb4afc9228a148cc5
created: 2026-07-07
updated: 2026-07-07
---
# SPA - Selective Prompt Anchoring

> [!summary] In short
> Fixes "attention dilution" in long code generation by keeping attention on the user prompt.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Memory and Long Context]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Attention by Highlighting]]  
**Original:** [Notion page](https://app.notion.com/p/396d68da32f080afb4afc9228a148cc5)

---

**The Problem: "Attention Dilution"**
  While Large Language Models (LLMs) are great at writing code, they often make mistakes or fail to follow all the user's instructions when the code gets long. The authors investigated *why* this happens and found a phenomenon they call **attention dilution**.  
  • As an LLM generates more and more code tokens, it starts paying less attention to the initial user prompt.  
  • Instead, the model gets distracted by its own recently generated code.  
  • If the model forgets a specific instruction from the prompt (like "count only uppercase vowels") halfway through generating the code, the final output will be incorrect.  
  **The Solution: Selective Prompt Anchoring (SPA)**
  To fix this, the authors propose a technique called **Selective Prompt Anchoring (SPA)**. SPA acts as a mathematical reminder, forcing the LLM to pay continuous attention to the original instructions without needing to retrain or fine-tune the model.  
  Here is how the SPA mechanism works step-by-step:
  1. **Identify Anchored Text:** SPA takes the most important part of the user's prompt (usually the natural language task description) and labels it as the "anchored text".  
  2. **Generate Two Outputs:** During code generation, the model calculates two different sets of predictions (logits) for the next word:  
      ◦ The *original* prediction (using the full prompt).  
      ◦ A *masked* prediction (where the anchored text is hidden using mask embeddings).  
  3. **Calculate the Difference:** By subtracting the masked prediction from the original prediction, SPA isolates the exact mathematical influence (the "contextual impact") of those specific instructions.  
  4. **Amplify the Instructions:** SPA multiplies this difference by a hyperparameter called the **anchoring strength** ($\omega$) and adds it back to the original prediction.

  
  
  **1. The Two Predictors**
  At any given step in generating code, the system runs the LLM twice to get two different perspective vectors:  
  • **Original Logits** $F_{\theta,i,x}(1)$**:** These are the predictions generated when the model can see the **entire prompt** (including the specific instructions).  
  • **Masked Logits** $F_{\theta,i,x}(0)$**:** These are the predictions generated when the important parts of the prompt—the "anchored text"—are completely **hidden or masked out**.  
  **2. The Subtraction Step**
  The mathematical operation is:

![[spa-selective-prompt-anchoring-1.webp]]
