---
type: paper
date: 2026-06-14
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/37fd68da32f0802f9baed634955ff9d4
created: 2026-06-14
updated: 2026-06-16
---
# iMAD - Intelligent Multi-Agent Debate

> [!summary] In short
> Runs debate only when it will likely help, using a self-critique and a classifier. Saves tokens.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - State of the Art Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/37fd68da32f0802f9baed634955ff9d4)

---

They believe that some conversiations do not need to be disscuss as group, because some research show that the group decisions are weaker that single LLM, they introduce a metric to evaluate score that shows us do we need group decision or not

#### What They Have Done
The authors propose a token-efficient framework called **intelligent Multi-Agent Debate (iMAD)** to address the computational costs and potential accuracy degradation of running Multi-Agent Debates (MAD) for every query. The system selectively triggers MAD only when it is likely to be beneficial (i.e., when it can correct an initially wrong answer).
Specifically, the framework consists of the following steps:
- **Structured Self-Critique Prompting:** iMAD prompts a single Large Language Model (LLM) agent to generate an initial Chain-of-Thought (CoT) justification, a required self-critique (counterargument), and confidence scores for both perspectives.
- **Feature Extraction:** The system extracts 41 interpretable linguistic and semantic features from this single-agent response to capture subtle internal hesitation cues.
- **Debate Decision Classifier:** These features are fed into a lightweight multi-layer perceptron (MLP) classifier to determine if MAD should be triggered.
- **FocusCal Loss:** To train this classifier robustly for zero-shot settings without test-dataset-specific tuning, they propose a novel composite objective called FocusCal loss. The loss function is defined as $L_{FC} = L_{AF} + \lambda \cdot L_{CP} + \mu \cdot ECE$. It integrates Asymmetric Focal Loss (to penalize overconfident errors), a Confidence Penalty (to penalize misalignments between confidence scores and semantic uncertainty), and Expected Calibration Error (to regularize predicted scores).

#### How They Evaluated
The authors evaluated iMAD by measuring two primary metrics against competing baselines:
- **Token Efficiency:** Evaluated by measuring the total token usage per data instance, which includes both the input tokens (prompts) and output tokens (generated responses).
- **Accuracy:** Evaluated by measuring the final answer correctness.

Ultimately, the authors found that iMAD significantly reduces token usage (by up to 92%) while concurrently improving final answer accuracy (by up to 13.5%) by selectively skipping unnecessary or detrimental debates.

#### Baselines
The authors compared iMAD against five competitive baselines split into three categories:
- **Single-Agent Methods:** Chain-of-Thought (CoT) and Self-Consistency (SC).
- **Full-Debate MAD Methods (triggers debate for all instances):** Multi-Agent Debate (MAD) and GroupDebate (GD).
- **Selective MAD Triggering:** DOWN (Debate Only When Necessary), a concurrent selective framework that relies on confidence score thresholds.

#### Datasets
The evaluation was conducted across six held-out datasets spanning both textual Question Answering (QA) and Visual Question Answering (VQA).
- **Textual QA Datasets:** MedQA (USMLE-style medical questions), MMLU (professional exam questions), and GSM8K (grade-school math word problems).
- **Visual QA Datasets:** OKVQA (visual questions requiring external knowledge), VQA-v2 (natural image QA), and ScienceQA (multimodal science questions).

*(Note: To ensure task-agnostic generalization and a true zero-shot evaluation, the classifier itself was trained solely on PubMedQA and GQA, which were excluded from the final evaluation.)*

#### Core Baseline Frameworks & Papers
The framework was evaluated against five major baseline approaches:
- **CoT (Chain-of-Thought):**
    - *Paper:* "Chain-of-thought prompting elicits reasoning in large language models" (Wei et al., 2022).
- **SC (Self-Consistency):**
    - *Paper:* "Self-Consistency Improves Chain of Thought Reasoning in Language Models" (Wang et al., 2023).
- **MAD (Multi-Agent Debate):**
    - *Paper:* "Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate" (Liang et al., 2024).
- **GD (GroupDebate):**
    - *Paper:* "GroupDebate: Enhancing the Efficiency of Multi-Agent Debate Using Group Discussion" (Liu et al., 2024).
- **DOWN (Debate Only When Necessary):**
    - *Paper:* "Debate Only When Necessary: Adaptive Multiagent Collaboration for Efficient LLM Reasoning" (Eo et al., 2025).
