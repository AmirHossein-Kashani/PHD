---
type: paper
date: 2026-07-15
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate", "Neuro-Symbolic Reasoning and Argumentation"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate, topic/neuro-symbolic-reasoning-and-argumentation]
notion: https://app.notion.com/p/39fd68da32f080b7bd21c6afb6f8a787
created: 2026-07-16
updated: 2026-07-17
---
# SMADE-IE

> [!summary] In short
> Sparse multi-agent debate with evidence for zero-shot information extraction (entities and relations). Good trick, but a different problem.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]], [[Neuro-Symbolic Reasoning and Argumentation]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/39fd68da32f080b7bd21c6afb6f8a787)

---

This paper is focusing on retrieving data not make the final decision, it only try to detect the entities and relation seperately and then revise the structure by puting them alongside each other. my opinion is that the trick that they have been used is good by they are solving wrong topic 🙂

## SMADE-IE: A Reading Guide

#### Sparse Multi-Agent Framework with Evidence-Driven Debate for Zero-Shot Information Extraction

---

### 1. Key Terms First
- **NER (Named Entity Recognition):** find the **"things"** in a sentence and label what kind of thing they are.
  *"Jordan Lee works at Amazon"* → **Jordan Lee = Person**, **Amazon = Organization**.
- **RE (Relation Extraction):** find **how the things are connected**.
  Same sentence → **(Jordan Lee, works at, Amazon)**.
- **JERE (Joint Entity–Relation Extraction):** do **both jobs at once** — and the two outputs must agree with each other.
- **Zero-shot:** the model gets **no training examples** — only the input text and the type definitions.

---

### 2. The Big Picture — Two Levels of the Pipeline
The framework works on **two separate levels**. Keeping them apart is the key to understanding the paper:

| Level | Name | What conflict it solves | When it runs |
|---|---|---|---|
| **Level 1 — Extraction** | Router + Global / Type-Centric modes + **Debate** | *Within-task* conflict: two agents doing the **same job** disagree on a label ("is Amazon ORG or LOC?") | During NER, and again during RE |
| **Level 2 — Alignment** | **IERA** | *Cross-task* conflict: the finished **entity list and relation list contradict each other** | Only for JERE, **after** both extractions are done |

> **Analogy:** Level 1 is two experts in the *same department* arguing about a label, with a judge. Level 2 is a *cross-department audit* — the entities report and the relations report are checked against each other until they match.
Flow for a JERE input:
  **NER extraction → RE extraction → IERA alignment → final output**

---

### 3. Level 1a — Adaptive Mode Selector (The Router)
- A **Router Agent** looks at each sentence and estimates two things:
    1. The **relevant type subset** (most sentences contain only 1–5 of the possibly 30+ types).
    1. The **complexity**: low / medium / high.
- **Low complexity** → cheap **Global Extraction Mode**.
- **Medium / high complexity** → careful **Type-Centric Extraction Mode**.
- **Why it matters:** you don't pay for heavy multi-agent machinery on easy sentences. This is where the "sparse" in the name comes from.

### 4. Level 1b — Global Extraction Mode (The Cheap Path)
- Just **two LLM calls**:
    1. **Universal Agent** — extracts all entities in one shot.
    1. **Verification Agent** — sees the candidates *and* the sentence together; **adds** missed entities, **removes** wrong ones.

### 5. Level 1c — Type-Centric Extraction Mode (The Careful Path)
- One **Type-Specific Agent per router-selected type** — each hunts for only its own type.
- A **Review Agent** sweeps the **leftover types** the router didn't select, as a safety net.
- Results are merged:
    - Entities with **one type** → output directly (no debate needed).
    - Entities claimed by **multiple types** → sent to the **Evidence-Driven Debate** (next section).

---

### 6. The Evidence-Driven Debate (Main Contribution — How the Debate Is Controlled)

#### 6.1 Entry Control — Filtering Before Debate Starts
- Each conflicting type must be written as a **Toulmin argument**: **Claim**, **Ground** (textual evidence), **Warrant** (reasoning link), **Backing**, and a self-written **Rebuttal**.
- An **external evidence scorer (AlignScore)** — *not* the LLM itself — rates how well the sentence supports each argument → qualifier **Q ∈ [0,1]**.
- Only the **top-2 candidates** enter the debate. Weak claims never get a voice, and O(N²) pairwise attacks collapse into one head-to-head match.

#### 6.2 Confidence Initialization — Bayesian Priors
- Each surviving candidate gets a **Beta posterior**: **α = Qκ, β = (1−Q)κ**.
- The prior strength **κ is scaled by the agent's own Rebuttal**: if the scorer finds the self-criticism credible, the agent starts with a **weaker, more movable prior** — honest self-doubt is built into the math.

#### 6.3 Attack Control During Rounds
- Agents may attack **only Ground and Warrant** — the two evidence-carrying components — never the Claim itself.
- Attack strength is computed by the **scorer, not the arguing LLMs**:
    `a = σ( γ · ( φ(refutation) − φ(defended text) ) )`
    → an attack counts **only if it is better supported by the sentence** than the text it targets.

#### 6.4 Validity Decay and Revision Limits
- Every component has a **validity score v** (starts at 1) that **decays exponentially** under successful attacks.
- Below threshold **θᵥ**, the agent may revise — but revived components return at a **discounted ceiling (ωᵏ)** that shrinks with every revision.
- Repeatedly broken components are **permanently removed** — no endless resurrection of bad arguments.

#### 6.5 Joint Update — No Posterior Recovery Loophole
- The **same attack score** updates the Beta posterior: refutation mass → **β**, residual support → **α**.
- **Attacks from weakened opponents count less** (weighted by the opponent's average validity).
- Already-damaged components contribute less support — confidence cannot recover while the evidence stays broken.

#### 6.6 Dual-Track Early Stopping (Backed by a Chebyshev Bound)
- **Track A — Posterior superiority:** stop when the leader beats the runner-up with probability > **δ_stop = 0.75** (a decisive winner exists).
- **Track B — Confidence convergence:** stop when consecutive posteriors barely move (**squared Hellinger distance &lt; ε**) — further debate cannot add information.
- Hard cap of **3 rounds**; if neither track fires, the **scorer breaks the tie** directly.
> **The philosophy in one line:** LLMs *generate* arguments and attacks, but an **external verifier scores everything**, and **probability math — not another LLM — makes the final call**.

---

### 7. Level 2 — Iterative Entity–Relation Alignment (IERA)
> **⚠ Not to be confused with Type-Centric mode.** Type-Centric mode settles disagreements *inside one task* (which type is this entity?). IERA settles disagreements *between two finished tasks* (the entity list vs. the relation list). It runs **only for JERE**, after both extractions are complete, and it **never debates types itself**.

#### 7.1 The Problem
The entity list (NER) and relation list (RE) are built separately, so they can **disagree**:
- A relation mentions a name that **was never put on the entity list**.
- A relation has a **type rule** (e.g., *"works at"* needs a **Person** on the left and an **Organization** on the right), but the entity list gives one side the **wrong type**.
- RE sometimes **invents relations** just because two names appear in the same sentence.

When the lists disagree, **one side is wrong — but we don't know which.**

#### 7.2 The Fix: A Proofreading Loop Between the Two Lists
**Step 1 — Fill in missing names.**
  If a relation mentions something not on the entity list, **add it** and figure out its type. The relation itself is evidence the thing exists.
**Step 2 — When the lists disagree, ask a referee.**
  A **Consistency Agent** picks a side for each conflict:
- **Entity label is right** → the relation must be wrong → **delete it** and put it on a **blacklist** (a do-not-return list).
- **Relation is right** → the entity label must be wrong → **fix the entity's type** to fit the rule.

**Step 3 — Clean up.**
  Entities no longer connected to any relation are **removed**.
**Step 4 — Try again with the cleaner lists.Re-run RE** on the corrected entities, filtered through the blacklist. With cleaner input, bogus relations usually **just don't come back**.
**Step 5 — Repeat** until nothing disagrees (in practice, **one pass is enough**; the paper sets K_JERE = 1).

#### 7.3 Simple Example (From the Paper)
A sentence says a freight company has **"a hub at the Dayton airport"**:
- First pass: the model wrongly claims the company is **"based in"** the airport — a hub is not a headquarters.
- After one IERA pass, RE is re-run and the **fake relation loses its support and gets dropped**.
- The baseline method has **no such loop**, so it keeps several invented location relations.

#### 7.4 The Intuition in One Line
**Each list fact-checks the other:** relations catch entities NER missed; entity types catch relations RE invented. The **blacklist is the loop's memory** — once a relation is judged wrong, it stays wrong.

---

### 8. Datasets Used (9 Benchmarks, 12 Settings)

| Task | Datasets |
|---|---|
| **NER** | CoNLL03, OntoNotes5, SciERC, CrossRE, REDFM |
| **RE** | DocRED, SemEval-2010 Task 8, SciERC, CrossRE, REDFM |
| **JERE** | CoNLL04, NYT |
