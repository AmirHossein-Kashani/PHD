---
type: review
date: 2026-06-12
date-end: 2026-06-13
status: In Progress
projects: ["Claim-Level Conflict in Debate", "Multi-Source Evidence RAG"]
topics: ["Multi-Agent Debate", "RAG and Knowledge Graphs"]
tags: [type/review, project/claim-conflict, project/multi-source-rag, topic/multi-agent-debate, topic/rag-and-knowledge-graphs]
notion: https://app.notion.com/p/37dd68da32f0803ba675fe964d6c3203
created: 2026-06-12
updated: 2026-06-14
---
# Review - State of the Art Multi-Agent Debate

> [!summary] In short
> Long review of the state of the art in multi-agent debate and KG reasoning (ToG, RoG, ...), benchmarks and SOTA results.

**Project:** [[Claim-Level Conflict in Debate]], [[Multi-Source Evidence RAG]]  
**Topics:** [[Multi-Agent Debate]], [[RAG and Knowledge Graphs]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Sub-notes:** [[AutoGen and MetaGPT]], [[Key Decision-Makers in Multi-Agent Debate]], [[DoM - Debate over Mixed-knowledge]], [[ACE - Analyze Compose Execute Debate]], [[iMAD - Intelligent Multi-Agent Debate]]  
**Original:** [Notion page](https://app.notion.com/p/37dd68da32f0803ba675fe964d6c3203)

---

## to explore more
**1. ToG (Think-on-Graph)The Core Idea:** ToG introduces a training-free framework that treats the Large Language Model (LLM) as an active agent that performs an iterative "beam search" directly on the Knowledge Graph (KG). Instead of pulling the whole graph into the LLM, the LLM explores the graph step-by-step, retrieves candidate entities/relations, and "prunes" irrelevant reasoning paths using its own internal logic to find the final answer.
- **The Limitation:** ToG is strictly constrained by the physical structure of the graph. If a vital edge or node is missing, the beam search hits a dead end, causing the reasoning chain to fail.

**2. RoG (Reasoning on Graphs)The Core Idea:** RoG is a fine-tuning-based "planning-retrieval-reasoning" framework. It first prompts a fine-tuned LLM to generate a logical "plan" (a sequence of relation paths). It then uses this plan to retrieve valid multi-hop reasoning paths from the actual graph, and finally feeds those retrieved paths back to the LLM to generate a faithful, interpretable answer.
- **The Limitation:** Because RoG relies on generating plans that map directly to the existing graph topology, it struggles heavily when the required knowledge falls outside the scope of the graph, making it brittle in incomplete KG scenarios.

**3. GoG (Generate-on-Graph)The Core Idea:** GoG is the direct predecessor to DoM, designed specifically for the problem of *Incomplete* Knowledge Graphs (IKGQA). It uses a "Thinking-Searching-Generating" framework. When the LLM agent is searching the graph and realizes that a necessary triple (fact) is missing, GoG allows the LLM to act *as* the knowledge graph and hallucinate/generate the missing triple based entirely on its own pre-trained internal memory.
- **The Limitation:** GoG relies entirely on the LLM's parametric memory to fill in the blanks. If the missing fact involves highly specific, recent, or obscure information that the LLM didn't memorize during training, GoG will generate a hallucinated or incorrect triple, leading to the wrong answer.

---

---

---

---

---

---

---

## Positioning a "Compressed Facts + Confidence" Multi-Agent LLM Framework: State of the Art, Baselines, and the Novelty Gap

### TL;DR
- The student's idea — compressing multi-agent LLM conversations into a structured table of extracted FACTS each paired with a CONFIDENCE score, then using that representation to drive the next step or final decision — has **no exact precedent**, but it sits between two close competitors: **ReConcile** (ACL 2024), which does confidence-weighted *agent-level* voting after discussion, and **DiscoUQ** (2026 preprint), which extracts *structured disagreement features* to produce a calibrated team-confidence score. Neither produces an explicit, enumerated fact-level confidence representation used as a controller for next-action selection, so the framing is genuinely novel.
- The strongest *baselines* the student must beat are cheap and well-established: **self-consistency / majority voting** (Wang et al. 2022), **"More Agents Is All You Need"** sampling-and-voting (Li et al. 2024), and **multi-agent debate** (Du et al. 2023); the strongest *state-of-the-art aggregation* competitors are **Mixture-of-Agents** (Wang et al. 2024), **ReConcile**, and newer confidence/higher-order aggregators (Beyond Majority Voting, 2025; DiscoUQ, 2026). A critical caveat from the literature: debate frequently fails to beat self-consistency at equal sample budget — at a matched budget of 9 responses on GSM8K, self-consistency reaches 88.2% while 3-agent/2-round debate reaches only 83.0% (Huang et al. 2023) — so the student must benchmark against self-consistency, not just vanilla debate.
- For the drug-discovery application, the relevant multi-agent baselines are **CLADD** (AAAI, code public), **PharmAgents**, **DrugAgent**, and medical-reasoning systems **MedAgents/MDAgents**; evaluation converges on **Therapeutics Data Commons (TDC)/ADMET**, drug–target interaction, and medical QA (MedQA, MedMCQA, PubMedQA) plus newer agentic benchmarks (PharmaBench, DO Challenge, "Beyond SMILES").

### Key Findings
1. **The idea's closest competitors are ReConcile and DiscoUQ.** ReConcile already uses per-agent confidence in a weighted vote and discusses overconfidence correction; DiscoUQ already extracts structured features (linguistic + embedding geometry) from multi-agent disagreement to produce calibrated confidence. The student's distinct claim must be the **fact-level** decomposition (not agent-level or answer-level) plus using the fact+confidence object as a **compressed state/controller** for next-step decisions — a framing neither paper occupies.
1. **Voting beats debate at equal budget, often.** "Large Language Models Cannot Self-Correct Reasoning Yet" (Huang et al. 2023) shows that at a matched budget of 9 total responses on GSM8K, self-consistency reaches 88.2% while multi-agent debate (3 agents, 2 rounds, gpt-3.5-turbo-0301) reaches only 83.0%; the authors conclude the gain is "not attributed to 'self-correction', but rather to 'self-consistency'." This is the single most important baseline-design lesson.
1. **Confidence-weighted and higher-order aggregation is an active 2024–2026 frontier** the student is squarely entering: ReConcile, Beyond Majority Voting (Optimal Weight / Inverse Surprising Popularity), credibility-scoring adversary-resistant systems, EMS (Majority-then-Stopping), and DiscoUQ all weight or gate aggregation by reliability/confidence.
1. **Context/conversation compression is mature but disconnected from multi-agent decision-making.** LLMLingua/LLMLingua-2 compress prompts; MemGPT, Generative Agents, A-Mem, recursive summarization compress dialogue/memory; structured belief-state and fact-extraction work exists in RAG and fact-checking. But these compress for *efficiency or memory*, not to produce a *fact+confidence decision object for an agent committee*. This is the cleanest gap.
1. **Drug-discovery multi-agent LLMs are abundant but aggregate at the agent/answer level**, not via fact+confidence structures, and the domain explicitly lacks decision-quality and uncertainty-aware benchmarks — a strong venue for the student's contribution.

### Details by Category

#### 1. Multi-Agent Debate / Discussion
- **Du et al. 2023, "Improving Factuality and Reasoning in Language Models through Multiagent Debate"** (arXiv 2305.14325; ICML 2024). The seminal "society of minds" method: multiple instances of an LLM propose and debate over rounds, taking the majority at the end. Tasks: arithmetic, GSM8K, MMLU, biographies, chess move validity. Code at composable-models.github.io/llm_debate. **This is the canonical debate baseline.**
- **Liang et al. 2024, "Encouraging Divergent Thinking… through Multi-Agent Debate" (MAD)** — addresses "Degeneration-of-Thought" with adversarial roles + judge.
- **Chan et al. 2024, ChatEval** (arXiv 2308.07201; ICLR 2024) — multi-agent referee team for evaluation; diversity of roles is critical. For GPT-4 it improves average Spearman and Kendall-Tau correlation with human judgments by 0.096 (16.3%) and 0.057 (10.0%) respectively, and on the FairEval QA task improves accuracy by 6.2% (ChatGPT) and 2.5% (GPT-4). Code public.
- **Critical counter-evidence:** Huang et al. 2023 "LLMs Cannot Self-Correct Reasoning Yet" (arXiv 2310.01798; ICLR 2024) — debate ≈ consistency mechanism; underperforms self-consistency at equal samples (88.2% vs 83.0% on GSM8K). Smit et al. 2024 — debate doesn't reliably beat strong single-agent baselines without tuning. iMAD (2025), Hear Both Sides (2026) — debate costs 3–5× tokens of CoT for modest gains.
- **Baseline:** Du et al. debate (3 agents, 2 rounds). **SOTA framing:** heterogeneous/role-diverse debate with judge, but with the caveat above.

#### 2. Voting / Ensembling / Self-Consistency
- **Wang et al. 2022, Self-Consistency** (arXiv 2203.11171; ICLR 2023) — sample diverse CoT paths, majority vote. Gains: GSM8K +17.9%, SVAMP +11.0%, AQuA +12.2%, StrategyQA +6.4%, ARC-challenge +3.9%. **The mandatory baseline.**
- **Li et al. 2024, "More Agents Is All You Need" (Agent Forest)** (arXiv 2402.05120; TMLR; Tencent) — "the performance of large language models (LLMs) scales with the number of agents instantiated… while the degree of enhancement is correlated to the task difficulty"; the method is orthogonal to existing methods such as CoT-SC and Debate. Code public (github.com/MoreAgentsIsAllYouNeed/AgentForest).
- **Wang et al. 2024, Mixture-of-Agents (MoA)** (arXiv 2406.04692; ICLR 2025) — layered proposers + aggregators; open-source MoA scores 65.1% on AlpacaEval 2.0 vs GPT-4 Omni's 57.5%. Code public. **SOTA for open-ended generation aggregation.**
- **Jiang et al. 2023, LLM-Blender** (arXiv 2306.02561; ACL 2023) — PairRanker + GenFuser; benchmark MixInstruct. Code public.
- **Universal Self-Consistency** (arXiv 2311.17311) — LLM picks most consistent free-form answer when no exact-match aggregation exists.
- **Beyond Majority Voting** (arXiv 2510.01499, 2025) — Optimal Weight + Inverse Surprising Popularity aggregators provably beat majority vote; tested on UltraFeedback, MMLU, ARMMAN healthcare.
- **Baseline:** majority vote / self-consistency. **SOTA:** MoA (generation), higher-order weighted aggregation (classification/QA).

#### 3. Structured Pipeline / Role-Based Systems
- **AutoGen** (Wu et al., COLM 2024) — flexible conversable agents; most general orchestration; code public (Microsoft).
- **MetaGPT** (Hong et al., ICLR 2024) — SOP-encoded role assembly line (PM/architect/engineer).
- **CAMEL** (Li et al. 2023) — inception-prompted role-play.
- **ChatDev** (Qian et al. 2024) — virtual software company; outperforms single-agent GPT-Engineer on completeness/executability.
- **AgentVerse** — staged group problem-solving.
- **Reflexion** (Shinn et al. 2023) — verbal self-reflection with episodic memory + binary reward.
- **Self-Refine** (Madaan et al. 2023; NeurIPS 2023, arXiv 2303.17651) — single-LLM iterative feedback→refine; "across all evaluated tasks, outputs generated with Self-Refine are preferred by humans and automatic metrics… improving by ~20%" absolute (e.g., Sentiment Reversal jumps 3.8→36.2, +32.4, on GPT-4). Code public.
- **Baseline:** Self-Refine / Reflexion (single-agent iterative). **SOTA:** AutoGen/MetaGPT for orchestration; these are *frameworks*, not aggregation methods — the student's method could plug in as an aggregation layer.

#### 4. Confidence Estimation & Calibration; Confidence-Weighted Aggregation
- **Lin et al. 2022** — introduced *verbalized confidence*. **Tian et al. 2023** — verbalized confidence better-calibrated than conditional probabilities for QA. **Xiong et al. 2024** (arXiv 2306.13063; ICLR 2024) — black-box framework (Prompting + Sampling + Aggregation); finding: LLMs are overconfident; white-box vs black-box AUROC gap narrow (0.522→0.605). **Geng et al. 2024** — comprehensive survey distinguishing verbalized / consistency-based / probability-based confidence.
- **Confidence-weighted multi-agent aggregation:** **ReConcile** (arXiv 2309.13007; ACL 2024) — round-table of ChatGPT/Bard/Claude2, multi-round discussion with grouped answers + confidence + answer-rectifying demonstrations, then confidence-weighted vote; surpasses single/multi-agent baselines by up to 11.4%, beats GPT-4 on 3 datasets, +8% on MATH; recalibrates confidence to counter overconfidence. **This is the most direct competitor for the "confidence" part.**
- **DiscoUQ** (arXiv 2603.20975, 2026 preprint) — extracts linguistic structure features + embedding-geometry features from agent disagreement → classifiers → calibrated confidence; reports AUROC 0.802 vs 0.791 best baseline, ECE 0.036 vs 0.098. **Closest match on "structured signals + confidence," but a single-author preprint with self-reported numbers — treat as unconfirmed.**
- **Adversary-resistant credibility scoring** (arXiv 2505.24239), **EMS Majority-then-Stopping** (arXiv 2604.02863), **A-HMAD** confidence consensus optimizer — all weight aggregation by reliability/confidence.
- **Baseline:** verbalized-confidence weighted vote (ReConcile-style). **SOTA:** DiscoUQ / higher-order aggregation, with calibration (ECE/AUROC) as the metric.

#### 5. Conversation/Context Compression & Memory
- **LLMLingua / LongLLMLingua / LLMLingua-2** (Jiang et al., EMNLP 2023 / ACL 2024; arXiv 2310.05736, 2310.06839, 2403.12968) — perplexity-based and token-classification prompt compression up to 20×. Code public (Microsoft).
- **MemGPT** (Packer et al. 2023) — OS-style tiered memory (core/recall/archival). **Generative Agents** (Park et al. 2023) — memory stream + reflection. **A-Mem** (arXiv 2502.12110) — Zettelkasten-style evolving memory. **Recursive Summarization** for long dialogue.
- **Structured belief / fact extraction:** "Structured Belief State…" (arXiv 2605.11325) — belief schema with epistemic status, confidence, supersession; REAP (arXiv 2511.09966) — Fact Extractor producing structured facts with evidence; LoCal fact-checking (WWW 2025) — agents assign confidence degrees to sub-claims.
- **Belief-driven multi-agent:** "From Debate to Equilibrium: Belief-Driven Multi-Agent LLM Reasoning via Bayesian Nash Equilibrium" (arXiv 2506.08292) — agents maintain belief networks mapping trajectory→belief→action. **Closest "structured belief state for multi-agent decision" match.**
- **Gap:** These compress for efficiency/memory or single-agent QA, not to produce a fact+confidence decision object aggregated across an agent committee. This is the student's cleanest white space.

#### 6. Drug-Discovery Multi-Agent LLMs
- **CLADD** ("RAG-Enhanced Collaborative LLM Agents for Drug Discovery," arXiv 2502.17506; AAAI; Lee et al., KAIST/Genentech) — Planning Team + Knowledge Graph Team + Molecular Understanding Team + Prediction Agent; no fine-tuning; GPT-4o-mini backbone. Tasks: drug–target prediction (Drug Repurposing Hub, DrugBank, STITCH), property captioning (BBBP/SIDER/ClinTox/BACE), toxicity/activity (hERG, DILI, Skin, Carcinogens, Macro-F1). Reports outperforming all general-purpose and domain-specific baselines. **Code public: github.com/Genentech/CLADD.** **Most directly relevant drug-discovery multi-agent baseline.**
- **PharmAgents** (arXiv 2503.22164) — virtual pharma simulating full discovery pipeline (target→lead→ADMET→toxicity) with explainable agents + ML tools.
- **DrugAgent** (arXiv 2411.15692, Liu et al.) — automates ML programming for drug tasks via Planner + Instructor; +4.92% ROC-AUC vs ReAct on DTI; F1 0.92 on PAMPA absorption. (A different same-named DrugAgent by Inoue et al. does DTI prediction with KG/Search/Reasoning agents.)
- **MADD, AgentD, ChemCrow, Coscientist, dZiner, DrugPilot** — tool-orchestrating LLM agents for cheminformatics/molecular design.
- **Medical decision-making (transferable):** **MedAgents** (Tang et al. 2024; arXiv 2311.10537; ACL 2024 Findings) — training-free zero-shot multi-disciplinary role-play: experts → summarize report → consensus, evaluated on nine datasets (MedQA, MedMCQA, PubMedQA + six MMLU subtasks); GPT-4 86.7% vs 80.6% zero-shot. **MDAgents** (Kim et al. 2024; arXiv 2404.15155; NeurIPS 2024) — adaptive solo/group structure; on full MedQA 5-options with GPT-4o-mini it scores 83.6%, beating ReConcile (80.2%), Weighted Voting (79.1%), and CoT-SC single-agent (77.2%). **MedCoAct** (arXiv 2510.10461) — confidence-aware clinical multi-agent. Code public for MedAgents.

#### 7. Relevant Benchmarks
- **General reasoning/aggregation:** GSM8K, MATH, SVAMP, AQuA, StrategyQA, ARC-Challenge, MMLU, TruthfulQA, MMLU-Pro; AlpacaEval 2.0, MT-Bench, FLASK, MixInstruct (generation).
- **Confidence/calibration metrics:** ECE, AUROC, failure prediction.
- **Drug discovery:** Therapeutics Data Commons (TDC)/ADMET group (22 tasks), MoleculeNet, GuacaMol, MOSES, PharmaBench (88 tasks, TDC + HLE subset), WelQrate, DO Challenge (resource-constrained virtual screening), "Beyond SMILES" 15-task capability taxonomy.
- **Medical:** MedQA, MedMCQA, PubMedQA, DDXPlus, PMC-VQA, PathVQA; references Med-PaLM 2, GPT-4 Medprompt.

### Recommendations (staged)
1. **Frame the contribution precisely as fact-level, confidence-tagged conversation compression used as a decision controller.** Differentiate explicitly from (a) ReConcile (agent-level confidence vote), (b) DiscoUQ (disagreement-feature confidence on the final answer), and (c) belief-state/Bayesian-equilibrium work (latent belief, not explicit fact tables). Claim the gap: *no prior method compresses a multi-agent conversation into an enumerated, individually-scored fact set that is then used to select the next action or final answer.*
1. **Build the baseline ladder in this order** (the threshold that changes your story): (i) single-agent CoT; (ii) **self-consistency at matched sample budget** — if you don't beat this, the contribution is weak (recall 88.2% SC vs 83.0% debate on GSM8K at 9 samples); (iii) Du et al. debate (3 agents/2 rounds); (iv) MoA and ReConcile (confidence-weighted) as the SOTA aggregation competitors; (v) DiscoUQ-style structured-confidence as the closest rival. Always report token/compute cost alongside accuracy, since the debate literature shows 3–5× cost for marginal gains.
1. **Report calibration, not just accuracy.** Use ECE and AUROC (selective-prediction) so the fact+confidence representation's value is visible even where accuracy ties. This is where a confidence-native method should dominate vote-only baselines, and it is how DiscoUQ frames its advantage (ECE 0.036 vs 0.098).
1. **For the drug-discovery instantiation, benchmark on TDC/ADMET + a DTI task and reuse CLADD as the public, directly-comparable multi-agent baseline**; consider PharmaBench and "Beyond SMILES"/DO Challenge for decision-quality and uncertainty-aware evaluation, which existing systems explicitly lack. Position the fact+confidence object as enabling auditable, uncertainty-aware molecular decisions — a stated open gap.
1. **Ablations that would change conclusions:** remove confidence (facts only) → tests whether confidence adds value beyond fact compression; remove compression (raw transcript + confidence) → tests whether structuring helps beyond ReConcile; vary calibration of verbalized confidence → tests robustness to LLM overconfidence (the documented failure mode of ReConcile/debate).

### Caveats
- **DiscoUQ (arXiv 2603.20975) is a single-author 2026 preprint with self-reported numbers and an unusual backbone name; treat its metrics as unconfirmed** until peer-reviewed. It is nonetheless the closest published framing and must be cited and distinguished.
- Several cited preprints carry 2026 arXiv IDs (DiscoUQ, EMS, Mozi, MAPLE, Beyond SMILES); these are recent and may not be peer-reviewed — verify before relying on exact numbers.
- The debate-vs-self-consistency literature is genuinely contested; some surveys report consistent debate gains while controlled studies (Huang et al. 2023; Smit et al. 2024) find debate ≤ self-consistency at equal budget. Present this tension honestly rather than assuming debate is a strong baseline.
- CLADD's exact per-dataset Macro-F1/AUC cells were not cleanly extractable from the rendered paper; pull verbatim numbers from the AAAI camera-ready Tables 2–3. Similarly, the headline "GPT-4 86.7%" for MedAgents is the average across its nine datasets, not a single-benchmark figure.
- "Verbalized confidence" from LLMs is systematically overconfident; any confidence-weighted method (including the student's) must include calibration or it will inherit this bias.

## Older
- The foundational "communicate-to-decide" canon is now well-established: **~~AutoGen~~** (most-cited orchestration framework, ~2,365 Google Scholar citations), **MetaGPT** (ICLR 2024 Oral, ~1,193 citations), and the two seminal multi-agent debate papers — Du et al.'s **"Multiagent Debate"** (ICML 2024, ~1,004 citations) and Liang et al.'s **"Encouraging Divergent Thinking"** (EMNLP 2024, ~671 citations) — are the most influential and highly-cited works.
- The 2024–2026 frontier has shifted from "more agents debate" toward **adaptive, optimizable, and scalable** structures: **MDAgents** (NeurIPS 2024 Oral, adaptive collaboration for medicine), **Mixture-of-Agents** (layered aggregation, beats GPT-4 Omni on AlpacaEval 2.0), **GPTSwarm** and **AFlow** (treating agent collaboration as optimizable graphs/workflows), and **MacNet** (scaling to 1,000+ agents) represent the current state of the art.
- A strong counter-current of 2024–2025 work — **"Should we be going MAD?"** (ICML 2024), **"Rethinking the bounds of LLM reasoning"** (ACL 2024), and **"Why Do Multi-Agent LLM Systems Fail?"** (NeurIPS 2025) — shows that naive multi-agent debate often does NOT reliably beat strong single-agent baselines (chain-of-thought + self-consistency); this is essential context for anyone choosing a method.

#### 1. General-purpose conversational / collaboration frameworks
**AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation** (Wu et al., Microsoft). arXiv:2308.08155 (Aug 2023); published at COLM 2024. This is the most widely adopted multi-agent framework, with ~2,365 Google Scholar citations as of June 2026. It defines "conversable" and "customizable" agents that exchange messages — combining LLMs, humans, and tools — to solve tasks across mathematics, coding, QA, operations research, online decision-making, and entertainment. It implements design patterns including reflection, hand-offs, group chat, and multi-agent debate. Link: [https://arxiv.org/abs/2308.08155](https://arxiv.org/abs/2308.08155) [Google Scholar + 2](https://scholar.google.com/scholar_lookup?hl=en&publication_year=2023&author=Q.+Wu&author=G.+Bansal&author=J.+Zhang&author=Y.+Wu&author=B.+Li&author=E.+Zhu&author=L.+Jiang&author=X.+Zhang&author=S.+Zhang&author=J.+Liu&author=A.+H.+Awadallah&author=R.+W.+White&author=D.+Burger&author=C.+Wang&title=AutoGen:+Enabling+Next-Gen+LLM+Applications+via+Multi-Agent+Conversation)
**CAMEL: Communicative Agents for "Mind" Exploration of LLM Society** (Li et al., KAUST). arXiv:2303.17760; NeurIPS 2023 (~335+ citations on its main Semantic Scholar record; true total higher due to split records). The progenitor of role-playing communicative agents, introducing "inception prompting" to guide two agents (e.g., a user and an assistant) toward autonomous cooperation without continuous human steering. CAMEL's open-source library is the foundation many later frameworks (including ChatDev) build upon. Link: [https://arxiv.org/abs/2303.17760](https://arxiv.org/abs/2303.17760) [arXiv](https://arxiv.org/abs/2303.17760)
**AgentVerse: Facilitating Multi-Agent Collaboration and Exploring Emergent Behaviors** (Chen et al., THUNLP/OpenBMB). arXiv:2308.10848; ICLR 2024 (~449 citations). Orchestrates a dynamically composed group of expert agents as a "greater-than-the-sum-of-its-parts" system, with expert recruitment, collaborative decision-making, and action-execution stages. Notable for analyzing emergent social/collaborative behaviors across text understanding, reasoning, coding, tool use, and embodied AI. Link: [https://arxiv.org/abs/2308.10848](https://arxiv.org/abs/2308.10848) [OpenReview](https://openreview.net/forum?id=EHg5GDnyq1)

#### 2. Multi-agent debate, consensus, and evaluation
**Improving Factuality and Reasoning in Language Models through Multiagent Debate** (Du, Li, Torralba, Tenenbaum, Mordatch — MIT/Google). arXiv:2305.14325; ICML 2024 (~1,004 citations). The canonical multi-agent debate paper, inspired by Minsky's "Society of Mind": multiple LLM instances independently answer, then read and critique each other's responses over several rounds to converge on a consistent answer, improving mathematical and strategic reasoning and factual accuracy. Requires only black-box access to a language generator. Link: [https://arxiv.org/abs/2305.14325](https://arxiv.org/abs/2305.14325) [arxiv](https://arxiv.org/pdf/2305.14325)
**Encouraging Divergent Thinking in LLMs through Multi-Agent Debate (MAD)** (Liang et al., Tsinghua/SJTU/Tencent AI Lab). arXiv:2305.19118; EMNLP 2024 (~671 citations). Introduces a "tit-for-tat" debate with a judge to counter the "Degeneration-of-Thought" problem of self-reflection. Demonstrated on commonsense machine translation and counter-intuitive arithmetic reasoning. Notably warns that LLMs "might not be a fair judge" when a different LLM is used for agents vs. judge — an early flag of the bias issues later formalized in failure-mode studies. Link: [https://arxiv.org/abs/2305.19118](https://arxiv.org/abs/2305.19118)
**ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate** (Chan et al., HKUST/THU). arXiv:2308.07201; ICLR 2024 (~700 citations). A multi-agent "referee team" that debates to evaluate text quality, using diverse role personas and communication strategies (One-By-One, Simultaneous-Talk). It delivers superior accuracy and correlation with human judgment over single-agent evaluation; the paper shows diverse personas are essential — identical role descriptions degrade performance. Link: [https://arxiv.org/abs/2308.07201](https://arxiv.org/abs/2308.07201)

#### 6. Critical evaluations and surveys (essential context)
**Should we be going MAD? A Look at Multi-Agent Debate Strategies for LLMs** (Smit, Grinsztajn, Duckworth, Barrett, Pretorius — InstaDeep). arXiv:2311.17371; ICML 2024. Benchmarks cost/time/accuracy trade-offs and concludes that "multi-agent debating systems, in their current form, do not reliably outperform other proposed prompting strategies, such as self-consistency and ensembling using multiple reasoning paths." Link: [https://proceedings.mlr.press/v235/smit24a.html](https://proceedings.mlr.press/v235/smit24a.html)
