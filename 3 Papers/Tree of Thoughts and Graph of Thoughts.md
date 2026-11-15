---
type: paper
date: 2026-05-15
date-end: 2026-05-16
status: Done
projects: ["Early Exploration (May-June 2026)", "Auto Researcher"]
topics: ["Agent Architectures and Frameworks", "Creativity and Exploration"]
notion-tags: ["Tier2", "LLM", "Generative", "protein", "paper"]
tags: [type/paper, project/early-exploration, project/auto-researcher, topic/agent-architectures-and-frameworks, topic/creativity-and-exploration]
notion: https://app.notion.com/p/362d68da32f080e5be18ce9b00a41d18
created: 2026-05-16
updated: 2026-05-17
---
# Tree of Thoughts and Graph of Thoughts

> [!summary] In short
> Reasoning as a tree or a graph of thoughts with generate, refine and aggregate steps.

**Project:** [[Early Exploration (May-June 2026)]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Agent Architectures and Frameworks]], [[Creativity and Exploration]]  
**Notion category:** [[Notion Category - Protein Generation]]  
**Original:** [Notion page](https://app.notion.com/p/362d68da32f080e5be18ce9b00a41d18)

---

> [!attachment] File in Notion: **Tree of Thoughts.pdf** (750 KiB) - [open the Notion page](https://app.notion.com/p/362d68da32f080e5be18ce9b00a41d18)

> [!attachment] File in Notion: **Graph of Thoughts.pdf** (2.8 MiB) - [open the Notion page](https://app.notion.com/p/362d68da32f080e5be18ce9b00a41d18)

![[tree-of-thoughts-and-graph-of-thoughts-1.webp]]

## Graph of Though

#### The Architecture
Besta et al. (2024) model an LLM's reasoning as an arbitrary directed graph **G = (V, E)** where vertices are "thoughts" and edges are derivation dependencies. This generalizes Chain-of-Thought (a path), CoT-SC (parallel paths), and Tree of Thoughts (a tree) by allowing vertices with in-degree greater than one — that is, **aggregation** of independent reasoning paths. Formally, GoT is a tuple **(G, T, E, R)**: the reasoning state, the admissible transformations, an evaluator, and a ranker.
Three primitive transformations are defined:
1. **Generation** — fans out from a thought v to k children, modeling exploration.
1. **Refining** — adds a self-loop (v, v), modeling in-place iterative improvement.
1. **Aggregation** — introduces a new vertex v⁺ with in-edges from multiple parents v₁, …, v_k, modeling the merge step of a divide-and-conquer schema. This is the transformation that breaks tree topology and is GoT's primary novelty.

The runtime decomposes into four modules:
1. **Prompter** — serializes the relevant graph context into a natural-language prompt.
1. **Parser** — does the inverse, extracting structured thought-state from the LLM's reply.
1. **Scoring & Validation** — assigns scores via either deterministic functions (sorting error count, set symmetric difference) or LLM self-evaluation (redundancy/retention for document merging).
1. **Controller** — orchestrates the entire process.

Two data structures carry the system:
1. **Graph of Operations (GoO)** — a *static* declarative plan, built once before execution, specifying which transformations to apply in which order with which branching factors.
1. **Graph Reasoning State (GRS)** — the *dynamic* evolving graph of thoughts with their scores and provenance.

#### Who Decides to Branch and Aggregate?
**The human user decides, entirely and statically, before execution begins.**

### what does the Controller do
- **Selects thoughts from the GRS** — at each step, it looks at the current Graph Reasoning State and picks which existing thoughts will participate in the next operation (e.g., "take the best-scoring sorted half" or "take both intermediate merge results").
- **Selects the next transformation** — it consults the GoO to determine *what* operation comes next (Generate, Aggregate, Score, KeepBest, Improve) and with what parameters (branching factor k, repeat count).
- **Dispatches to the Prompter** — it passes the selected thoughts and the chosen operation to the Prompter, which constructs the actual LLM prompt.
- **Decides termination** — it determines whether to issue the next round of LLM interaction or finalize the process. In the current design, this decision is purely a function of whether the GoO plan has been fully walked.

## Tree of Thought

#### The Architecture
The ToT framework frames any problem as a search over a tree, where each node is a state **s = [x, z₁...ᵢ]** representing the input plus a sequence of thoughts so far. Instantiating ToT for a specific problem requires answering four design questions:

#### 1. Thought Decomposition
Decide what constitutes a "thought" for the problem. Examples from the paper: an intermediate equation for Game of 24, a writing plan for Creative Writing, or a word fill-in for Crosswords.

#### 2. Thought Generator G(pθ, s, k)
Two strategies for generating k candidate next thoughts from a state:
- **Sample (i.i.d.)** — draw k independent samples from a CoT prompt. Better when the thought space is rich (e.g., paragraphs in Creative Writing), since independence yields diversity.
- **Propose** — use a "propose prompt" to generate k thoughts in one sequential call. Better when the thought space is constrained (e.g., Game of 24, Crosswords), avoiding duplicates.

#### 3. State Evaluator V(pθ, S)
Two strategies for heuristically evaluating states:
- **Value** — assign each state an independent scalar/classification (e.g., "sure/maybe/impossible" in Game of 24) via a value prompt that performs lookahead simulations and commonsense reasoning.
- **Vote** — compare states against each other in a vote prompt and select the most promising. Useful when direct valuation is hard (e.g., judging passage coherency in Creative Writing).

Both can be sampled multiple times for robustness.

#### 4. Search Algorithm
Plug-and-play classical search:
- **BFS (Algorithm 1)** — maintains the b most promising states per step. Used for Game of 24 (b=5, depth=3) and Creative Writing (b=1, depth=2).
- **DFS (Algorithm 2)** — explores the most promising state first, prunes when the evaluator deems a state hopeless, and backtracks. Used for Crosswords, which has up to 10 steps and benefits from exploring alternative word placements.
