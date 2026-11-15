---
type: topic
aliases: [RAG, GraphRAG, KG, knowledge graph]
tags: [type/topic, topic/rag-and-knowledge-graphs]
---
# RAG and Knowledge Graphs

> [!summary] In short
> **RAG (retrieval-augmented generation):** the model searches documents first and then answers using them. A **knowledge graph (KG)** stores facts as links between entities (for example drug -> gene -> side effect). Combining both helps with **multi-step medical questions**, but it brings new problems: noisy search, old or conflicting papers, and the need to show **where each claim comes from**.

## Key ideas from my notes

- **KG prompting:** extract evidence subgraphs and let the LLM reason over them ([[MindMap - KG Prompting for LLMs|MindMap]]).
- **Agentic KG building:** agents build and update a medical KG with confidence scores ([[AMG-RAG - Paper Summary|AMG-RAG]]) - base of my project.
- **Graph RAG for medicine:** three-tier KG and U-Retrieval ([[MedGraphRAG]]); GNN-based checking ([[GraphCheck]]); static + dynamic knowledge ([[KAG - Knowledge Augmented Generation|KAG]]).
- **KG reasoning:** Think-on-Graph (beam search on the KG), Reasoning-on-Graphs, ToG-2, ToG v3 (hierarchical graph, dynamic KG update by agents).
- **When the KG is incomplete:** KG agent + text agent + judge ([[DoM - Debate over Mixed-knowledge|DoM]]).
- **Checking answers:** split into claims, check with NLI and a KG ([[MedRAGChecker]]).
- **Conflicts in medical RAG:** relevant papers can disagree; most systems filter them ([[Contradictions in Context - Medical RAG]]). We want to **explain** them.
- **Search shape matters (my result):** joining KG-related terms with AND raised on-topic retrieval from about 20% to 54%; LLM-written focused queries reached 65% ([[MS - Phases and Results]]).

## Where it is used

- [[Multi-Source Evidence RAG]] (main topic)

## Reviews

- [[Review - Stance-Aware KG Medical Fact Verification]]
- [[Review - Medical Fact-Verification Bibliography]]
- [[Review - Selected KG and RAG Papers]]

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-06-16 One-on-One

*Original: [Notion page](https://app.notion.com/p/381d68da32f080b98331db91fa9ab6a2)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-16 One-on-One|Early Exploration (May-June 2026)]], [[Skills and Tools#2026-06-16 One-on-One|Skills and Tools]]

- [ ] Kag paper
    - it is in progress
    - [[KAG - Knowledge Augmented Generation]]

### 2026-06-09 One-on-One

*Original: [Notion page](https://app.notion.com/p/37ad68da32f080bcae3cdec43f9f2229)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-09 One-on-One|Multi-Agent Debate]], [[Early Exploration (May-June 2026)#2026-06-09 One-on-One|Early Exploration (May-June 2026)]], [[MS - Meeting Log#2026-06-09 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-06-09 One-on-One|Skills and Tools]]

#### Next week plan
- [ ] Rag
    - [x] standfor session about Rag
        - [Stanford CS25: V3 I Retrieval Augmented Language Models](https://www.youtube.com/watch?v=mE7IDf2SmJg)
    - [x] why not rag
        - [Is RAG Still Needed? Choosing the Best Approach for LLMs](https://www.youtube.com/watch?v=UabBYexBD4k)

## Notes on this topic

### Reports and experiments (7)

- [[AMG-RAG Final Conclusion]] - Final conclusion: Phase 13 is the best research configuration (contradiction discovery); Phase 11 stays as the precision and safety control.
- [[AMG-RAG - Role of LLM, KG and Internet]] - Write-up: what parametric LLM knowledge, the curated KG, and internet search each add to ADR question answering. The best system is a hybrid.
- [[AMG-RAG Phase 11 - Method Note]] - Full method note of Phase 11: knowledge sources, 6-stage pipeline, evidence ledger with rule-based confidence, contradiction flags, results and limitations.
- [[AMG-RAG Phase Evolution Report]] - Story of all AMG-RAG phases from baseline to Phase 13. Phase 11 is most precise; Phase 13 is best for finding and explaining contradictions.
- [[AMG-RAG Phases Comparison Tables]] - All comparison tables: configuration matrix, headline metrics, retrieval precision, search shape, ledger results, calibration and cost.
- [[AMG-RAG Phase 9 - How It Works]] - Phase 9 of AMG-RAG: search related KG entities together as one PubMed AND-query, with back-off, then read open full text.
- [[AMG-RAG Phases Report (Scores)]] - Phase catalogue, scores (on-topic %, grounded %, ClinPGx-valid %, false premise), search shape and genes found per phase.

### Papers (11)

- [[AMG-RAG - Paper Review Notes]] - My review/feedback notes on the AMG-RAG paper (agentic medical KG for QA).
- [[AMG-RAG - Paper Summary]] - AMG-RAG: LLM agents build and update a medical KG from live sources and reason over it. Our multi-source project extends this system.
- [[Contradictions in Context - Medical RAG]] - Shows that medical RAG is not reliable just because papers are relevant: similar papers can disagree. They filter out contradicting papers instead of explaining why.
- [[Debate on Graph (DoG)]] - KG question answering with debate to avoid long reasoning paths and false-positive relations.
- [[DoM - Debate over Mixed-knowledge]] - For incomplete KGs: a KG agent and a text (RAG) agent work in parallel and a judge decides.
- [[GraphCheck]] - Uses a GNN over a KG for fact checking. Ignored for now.
- [[KAG - Knowledge Augmented Generation]] - Combines an expert-made static KG with dynamic knowledge from context. Planning, reasoning and retrieval loop.
- [[MedGraphRAG]] - Graph RAG for medicine with a three-tier KG (patient data, literature, UMLS) and U-Retrieval. Too complex for us.
- [[MedRAGChecker]] - Breaks an answer into claims and checks each claim with NLI on retrieved text and with a biomedical KG (DRKG or BioPortal).
- [[MindMap - KG Prompting for LLMs]] - MindMap: extract path-based and neighbor-based evidence subgraphs from a KG, then let the LLM reason over them. Used as a baseline in the multi-source project.
- [[ResearchAgent - Iterative Idea Generation]] - Research ideas from a core paper, a citation graph and an entity co-occurrence store, refined by 15 reviewer agents. No experiments (NAACL 2025).

### Literature reviews and lists (5)

- [[Review - BC Children Related Works]] - Folder page for related work of the BC Children (multi-source) project.
- [[Review - Medical Fact-Verification Bibliography]] - Categorized bibliography (about 55 papers) for stance-aware, KG-grounded, multi-document medical fact verification.
- [[Review - Selected KG and RAG Papers]] - Selected papers that do documents to graph to LLM decision in one pipeline (MedGraphRAG, GraphCheck, AMG-RAG, ...).
- [[Review - Stance-Aware KG Medical Fact Verification]] - Literature review for a system that splits support vs contradict evidence over a dynamic medical KG and gives a confidence score.
- [[Review - State of the Art Multi-Agent Debate]] - Long review of the state of the art in multi-agent debate and KG reasoning (ToG, RoG, ...), benchmarks and SOTA results.

