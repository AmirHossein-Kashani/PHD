---
type: review
date: 2026-07-02
date-end: 2026-07-05
status: Not started
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate", "Benchmarks and Datasets"]
notion-tags: ["classification", "classification topics"]
tags: [type/review, project/claim-conflict, topic/multi-agent-debate, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/395d68da32f080ce8354ee0d5fb161b3
created: 2026-07-06
updated: 2026-07-06
---
# Review - MAD Benchmarks for Complex Tasks

> [!summary] In short
> Which benchmarks MAD papers use, and which complex but gradable tasks could be used instead.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Original:** [Notion page](https://app.notion.com/p/395d68da32f080ce8354ee0d5fb161b3)

---

Most MAD papers recycle a small, overlapping pool of benchmarks:
- **GSM8K / MATH** (math word problems)
- **MMLU** (factual QA, multiple-choice — technically closed-form but shallow reasoning)
- **Arithmetic/counter-intuitive arithmetic** (synthetic, easy to verify)
- **Chess move validity/optimality** (Du et al.'s benchmark)
- **Commonsense MT** (translation quality — fuzzier)
- **Biography factuality** (Du et al.'s own benchmark — reference-checked against Wikipedia)

| Task type | Why it's still closed-form (gradable) | Example benchmarks |
|---|---|---|
| **Code generation w/ unit tests** | Pass/fail on test suite = objective | HumanEval, MBPP, SWE-bench |
| **Constraint satisfaction / planning** | Success = goal state reached, checkable | ALFWorld, PlanBench |
| **Formal/symbolic reasoning** | Proof either checks or doesn't | miniF2F, theorem-proving benchmarks |
| **Multi-hop QA with exact-match** | Final answer string is checkable even if path is long | HotpotQA, MuSiQue |
| **Tool-use/API-call correctness** | Call either matches expected schema/outcome or not | ToolBench, AgentBench (some subsets) |
| **Game-based sequential tasks** | Win/loss or score is objective | Chess (already used), Avalon, negotiation games with payoff matrices |

#### 1. By answer structure (how "complex" manifests)

| Type | Definition | Example benchmarks |
|---|---|---|
| **Multi-step/chained reasoning** | Answer requires several dependent intermediate steps; error in one step propagates | GSM8K, MATH, MathQA, ARC, StrategyQA |
| **Multi-hop retrieval/QA** | Requires combining facts from multiple sources/passages | HotpotQA, 2WikiMultihopQA, MuSiQue |
| **Open-ended generation** | No single correct answer; judged on quality, coherence, factuality | Summarization (XSum, CNN/DM), long-form QA (ELI5), creative writing |
| **Structured/executable output** | Answer must satisfy formal correctness (compiles, passes tests) | HumanEval, MBPP, SWE-bench, code repair |
| **Sequential decision-making** | Actions unfold over time, environment state changes, credit assignment across steps | ALFWorld, WebArena, AgentBench, ScienceWorld |
| **Interactive/dialogue-based** | Multi-turn, context accumulates, no single "final" judgment point | MT-Bench, dialogue-based agent benchmarks |
| **Multi-modal/grounded** | Complexity from integrating modalities, not just text reasoning | MathVista, ScienceQA (with diagrams) |

#### 2. By evaluation difficulty (a second, orthogonal axis)
- **Verifiable/closed-form** — has ground truth, easy to auto-grade (GSM8K, code tests)
- **Reference-based but fuzzy** — needs semantic similarity or LLM-judge scoring (summarization, translation)
- **Reference-free/subjective** — no ground truth exists at all, needs pairwise/debate-style judgment (creative writing, open-ended argumentation)
- **Process-level vs outcome-level** — does the benchmark only check the final answer, or does it also evaluate the reasoning trace/path (important for MAD, since debate should ideally improve the *process*, not just get lucky on the outcome)

#### 3. By task topology (relevant specifically to MAD)
- **Single-shot complex** — one hard question, but still one-shot judgment (MATH problem)
- **Decomposable** — the task can be split into independent sub-tasks that agents can debate separately (multi-hop QA, planning)
- **Sequential/stateful** — decisions depend on prior decisions, so debate must happen *per step*, not just once (agentic/tool-use benchmarks)
- **Adversarial/game-theoretic** — agents have competing objectives, not just cooperative debate (negotiation, Diplomacy-style benchmarks)

#### Foundational MAD papers
**1. "Improving Factuality and Reasoning in Language Models through Multiagent Debate"**
Du, Li, Torralba, Tenenbaum, Mordatch — **ICML 2024** (Proceedings of the 41st ICML)
Multiple LLM instances debate over rounds to converge on an answer. Evaluated on math word problems, GSM8K, chess move optimality, and a new biography-factuality benchmark — already goes beyond binary judgments into multi-step reasoning and open-ended factual generation.
arXiv: 2305.14325 (preprint) → published ICML 2024
**2. "Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate"**
Liang, He, Jiao, Wang, Wang, Wang, Yang, Shi, Tu — **EMNLP 2024** (main conference, pp. 17889–17904)
"Tit-for-tat" debate + judge to fix the Degeneration-of-Thought problem. Tests on commonsense machine translation and counter-intuitive arithmetic reasoning — sequential, non-binary tasks.

#### Multi-step reasoning
**3. "ReConcile: Round-Table Conference Improves Reasoning via Consensus among Diverse LLMs"**
Chen, Saha, Bansal — **ACL 2024** (Volume 1: Long Papers, pp. 7066–7085)
Heterogeneous LLMs debate with confidence-weighted voting; evaluated on commonsense, math, and multi-step reasoning benchmarks.

#### Long-form / evaluation tasks
**4. "ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate"**
Chan, Chen, Su, Yu, Xue, Zhang, Fu, Liu — **ICLR 2024** (Poster)
Multi-agent referee team debates to evaluate open-ended text quality (not a single correct answer) — directly targets non-binary, subjective, long-form evaluation tasks.

#### Sequential / agentic coordination
**5. "AgentVerse: Facilitating Multi-Agent Collaboration and Exploring Emergent Behaviors"**
Chen, Su, Zuo, Yang, Yuan, Qian, Chan, Qin, Lu, et al. — **ICLR 2024**
Multi-agent framework tested on reasoning tasks and software/tool-use-style sequential collaboration.

#### Recent follow-up (2025)
**6. "ConsensAgent: Towards Efficient and Effective Consensus in Multi-Agent LLM Interactions through Sycophancy Mitigation"**
Pitre, Ramakrishnan, Wang — **Findings of ACL 2025**, pp. 22112–22133
Addresses conformity failures in MAD consensus-building, relevant to why simple binary-debate setups break down on harder tasks.
