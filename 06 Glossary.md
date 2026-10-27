---
type: glossary
tags: [type/glossary]
---
# Glossary

> [!summary] In short
> Short, simple definitions of words used in this vault. Sorted A to Z.

| Term | Simple meaning | See |
|---|---|---|
| **ADR** | Adverse drug reaction: a harmful side effect of a drug (for example hearing loss from cisplatin) | [[Multi-Source Evidence RAG]] |
| **Aggregator** | The step that merges the answers of many agents into one output | [[Creativity in Multi-Agent Debate]] |
| **AMG-RAG** | Agentic Medical Graph-RAG: agents build a medical KG and answer questions over it. Base of our multi-source system | [[AMG-RAG - Paper Summary]] |
| **Arbor** | Research agent with a persistent hypothesis tree, a coordinator and executors. Base of our auto researcher | [[Arbor - Hypothesis Tree Refinement]] |
| **ARIADNE / ARIADNE-Bio** | Our August plan for a multi-agent research system with reversible exploration | [[Reversible Exploration (ARIADNE and Retro-Fork)]] |
| **Back-off (search)** | If a search finds nothing, remove one term and try again | [[MS - Method (Pipeline and Ledger)]] |
| **BioASQ** | Biomedical QA challenge; we use its list questions | [[MS - Paper Plan (Baselines and Datasets)]] |
| **Calibration** | Does a confidence of 0.8 really mean "right 80% of the time"? | [[Evaluation and Metrics]] |
| **Claim-level conflict** | Disagreement about one specific statement, not about the whole answer | [[Claim-Level Conflict in Debate]] |
| **Cliff's d** | Effect size for small samples (how often one group beats the other) | [[Evaluation and Metrics]] |
| **ClinPGx / PharmGKB** | Curated database of gene-drug-variant links with evidence levels (1A strong to 4 weak) | [[MS - Method (Pipeline and Ledger)]] |
| **Compute Canada / Alliance** | Canadian research computing (HPC clusters like Fir and Nibi) | [[How to Start Compute Canada]] |
| **Contradiction flag** | A label in the ledger that says sources disagree (for example `curated_vs_literature`) | [[MS - Method (Pipeline and Ledger)]] |
| **Coordinator / executor** | In Arbor: the long-lived planner that owns the tree / short-lived workers that test one idea | [[Arbor - Hypothesis Tree Refinement]] |
| **Evidence ledger** | A table per gene-drug pair with all supporting and refuting claims, a rule-based confidence and flags | [[MS - Method (Pipeline and Ledger)]] |
| **Evidence tier** | Type of evidence: curated, clinical, mechanistic, narrative. Only same-tier evidence can contradict | [[MS - Method (Pipeline and Ledger)]] |
| **Exploration vs exploitation** | Try new things vs improve what already works | [[Creativity and Exploration]] |
| **False premise test** | A question that contains a wrong claim (CYP2D6-doxorubicin). A good system should say it is wrong | [[MS - Phases and Results]] |
| **Fixation** | An agent keeps defending its first idea even when evidence says it is wrong | [[AR - Method and Contributions]] |
| **GFlowNet** | A model that learns to build objects with probability proportional to a reward | [[GFlowNet Foundations]] |
| **Grounded %** | Share of genes in the answer that really appear in the retrieved evidence | [[MS - Phases and Results]] |
| **KG** | Knowledge graph: facts stored as entities and links | [[RAG and Knowledge Graphs]] |
| **LangGraph** | Python library to build agents as graphs of nodes and edges | [[LangGraph]] |
| **Local knowledge (path to root)** | Our idea: each agent keeps the chain of decisions from its current idea back to the root | [[AR - Method and Contributions]] |
| **MAD** | Multi-agent debate | [[Multi-Agent Debate]] |
| **Masking (context)** | Each agent sees only part of the facts | [[Creativity in Multi-Agent Debate]] |
| **MCMC** | Markov chain Monte Carlo: sampling by a long random walk | [[MCMC - Markov Chain Monte Carlo]] |
| **MCP** | Model Context Protocol: a standard way to give tools to an LLM | [[Claude Skills vs MCP]] |
| **MoA** | Mixture-of-Agents: layers of proposer and aggregator LLMs | [[MoA - Mixture-of-Agents]] |
| **On-topic %** | Share of retrieved papers that mention both the drug and the side effect | [[MS - Phases and Results]] |
| **Persona asymmetry** | Give each agent a different way of thinking (mechanism-first, precedent-first, skeptic) | [[Creativity in Multi-Agent Debate]] |
| **PubMed / PMC** | PubMed = database of biomedical abstracts; PMC = open full-text articles | [[MS - Method (Pipeline and Ledger)]] |
| **QD (quality-diversity)** | Search that keeps a set of good *and different* solutions, not only the single best | [[Creativity and Exploration]] |
| **RAG** | Retrieval-augmented generation: search first, then answer with the found text | [[RAG and Knowledge Graphs]] |
| **Retro-Fork** | Later name of the reversible exploration proposal | [[Reversible Exploration (ARIADNE and Retro-Fork)]] |
| **Scientist-level agent** | An agent that runs the whole discovery loop (vs Tool or Analyst level) | [[Autonomous Research Agents]] |
| **Spearman (rho)** | Rank correlation between two lists of numbers | [[Evaluation and Metrics]] |
| **Trajectory of thinking** | Append-only log of plan, implementation, result, revision for all agents; used as a control signal | [[AR - Method and Contributions]] |
| **Usable novelty** | Rare ideas minus the ones a judge flags as implausible | [[Creativity in Multi-Agent Debate]] |
| **Worktree (git)** | A separate working copy of a git repo, so an experiment does not touch the main code | [[Arbor - Hypothesis Tree Refinement]] |
