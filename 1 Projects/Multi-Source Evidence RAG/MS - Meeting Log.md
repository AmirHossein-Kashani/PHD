---
type: project-log
project: Multi-Source Evidence RAG
tags: [type/project-log, project/multi-source-rag]
---
# MS - Meeting Log

Part of [[Multi-Source Evidence RAG]].

> [!summary] In short
> Everything that was said or reported in meetings about the **Multi-Source Evidence RAG** (BC Children / AMG-RAG) project, newest first. Each entry is one meeting (date + title) with a link to the original Notion page. Meetings are **not separate notes**: their content lives here and in the topic notes. If a meeting also talked about other things, the entry links to the other parts (for example in [[AR 7 - Meeting Log]]).

> [!tip] Add a new meeting
> Copy the `Meeting entry` template to the **top** of the entries below (or into [[AR 7 - Meeting Log]] / a topic note, depending on what the meeting was about).

---

## Entries

### 2026-10-20 One-on-One

*Original: [Notion page](https://app.notion.com/p/3ffd68da32f080759fc4f51afeba7a6b)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-10-20 One-on-One|AR - Meeting Log]], [[Skills and Tools#2026-10-20 One-on-One|Skills and Tools]]

#### What have done
- [ ] i have updated the target benchmarks for amg-rag

#### what to talk and todo
- [ ] about BC children meeting

### 2026-10-14 One-on-One

*Original: [Notion page](https://app.notion.com/p/3f9d68da32f08078b783ebb810e6a469)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-10-14 One-on-One|AR - Meeting Log]]

#### next week plan
- [ ] lets implement the trajectory idea of multi evidence 
- [ ] Finalizing baselines for multi evidence paper

### 2026-10-07 BC Children Meeting

> [!summary] This meeting in short
> Paper outline for the multi-source project. Multi-source RAG papers to read. Final choice: BioASQ list questions with five baselines (LLM only, PubMed RAG, MindMap, simple hybrid RAG, AMG-RAG).

*Original: [Notion page](https://app.notion.com/p/3f2d68da32f0805cbe09cc683e643ad8)*

#### Paper Outline
- introduction
- Related works
- Methode 
    - overview
        - Introduce the problem and define input and output 
            - generating a list of potential answers with their confidence 
            - input : Question + KG
        - Image architecture
    - Evidence Retrieval
        - Knowledge Graph and search augmentation
        - PubMed literature search 
    - Evidence Analysis and disagreement management
        - Scoring system
        - Conflict resolution 
        - generation of final answer and citing evidence  
- Experimental Evaluation
    - base lines
        - Mindplus+ (Tulika)
        - (Tulika) AMG-RAG
    - metrics
    - Datasets &lt;Dataset with multiple choice are targeted for this evaluation>
        - (Tulika) finding multiple answer questions
    - results
- conclusion 

Paper general good to read about multi source 

---
[HydraRAG: Structured Cross-Source Enhanced LLM Reasoning](https://arxiv.org/abs/2505.17464)

---
[Think-on-Graph 2.0 (ToG-2)](https://arxiv.org/abs/2407.10805)

---
[Chain-of-Knowledge (CoK)](https://arxiv.org/abs/2305.13269)

---
[WeKnow-RAG](https://arxiv.org/abs/2408.07611)

---
[HybGRAG](https://aclanthology.org/2025.acl-long.43)

---
[DeepSieve](https://arxiv.org/abs/2507.22050)

---
[PruningRAG: Multi-Source Knowledge Pruning](https://arxiv.org/abs/2409.13694)

---
[QUASAR: RAG over Heterogeneous Data and Text](https://arxiv.org/abs/2412.07420)

---
[PolyUQuest](https://arxiv.org/abs/2607.08269)

---

Final version 
For your **KG + PubMed search → list of answers** task, these are the relevant choices.

| Benchmark | KG + text/search? | List answers? | Recommendation |
|---|---|---|---|
| **BioASQ — list questions [no KG] [still good option]** | PubMed search; you add your KG | Yes, expert-written lists | **Best fit for your PubMed-based system**, but the KG component is your adaptation |
| **~~*RiTeK-PharmKG [*~~*context  attached to the entities so we don’t have text search* ~~*]*~~** | Built-in KG + searchable entity descriptions | Yes | **Best ready-made KG + text benchmark**; does not natively include PubMed search |
| **~~STaRK-Prime (Text is not search able )~~** | Built-in graph + entity text | Multiple-answer entity IDs | Good alternative for biomedical entity retrieval; does not natively include PubMed search |

more dataset 
- **BioMedHop** 
    best match  but  everything exist already in KG but text covert to make it deeper understanding 

These capabilities are documented by the official benchmark sources. [participants-area.bioasq.org](https://participants-area.bioasq.org/general_information/Task14b/?utm_source=chatgpt.com)
Compare your system against these **five baselines**:

| Baseline | Sources | Purpose |
|---|---|---|
| **LLM only** | No retrieval | Measures the model’s stored knowledge |
| **Plain PubMed RAG** | Literature only | Tests search without the KG |
| **MindMap** | KG | Tests graph-based answering |
| **Simple hybrid RAG** | KG + PubMed | Combines retrieved graph facts and passages without your evidence assessment |
| **AMG-RAG** | KG + external retrieval | Compares against your closest predecessor |

MindMap and AMG-RAG have public implementations; document their adaptations to your sources and list-answer format. [github.com](https://github.com/wyl-willing/MindMap?utm_source=chatgpt.com)
**Optional stronger hybrid baseline:** **ToG-2**, if you can adapt its released Wikidata/text retrieval to your biomedical KG and PubMed. [github.com](https://github.com/DataArcTech/ToG-2?utm_source=chatgpt.com)
**My selection: BioASQ list questions + those five baselines.** Choose RiTeK-PharmKG instead if having a supplied KG-and-text benchmark matters more than testing PubMed search.

### 2026-09-29 One-on-One

*Original: [Notion page](https://app.notion.com/p/3f1d68da32f0807585b6fa7786e244a9)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-09-29 One-on-One|AR - Meeting Log]]

- [ ] AMG-RAG
    - making repo 
    - trajectory have to be added (currenlty long path trajectory findings)
    - proposed plan to continue 

| Benchmark | What it tests | why | target |
|---|---|---|---|
| **[PGxQA](https://github.com/KarlKeat/PGxQA)** | Pharmacogenomic questions, including drug→gene identification, clinical guidance, and adversarial questions | Closest match to your domain, but mainly evaluates established guideline knowledge | **Use for the main external comparison** |
| **[HealthContradict](https://github.com/tinaboya/HealthContradict)** | Answering health questions given correct, incorrect, or conflicting documents | Closest match to your conflict-handling contribution; requires adapting your gene–drug-specific ledger to general claims | **Use if you can generalize that component** |
| **[PGxCorpus](https://github.com/practikpharma/PGxCorpus/)** | Extracting genes, variants, drugs, phenotypes, and their relationships from text | Directly tests your extraction and matching stage | **Use as a component evaluation** |

### 2026-09-21 One-on-One

*Original: [Notion page](https://app.notion.com/p/3ead68da32f080a3839afa4c8f6bead8)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-09-21 One-on-One|AR - Meeting Log]]

- [x] i have updated the architecture of the AMG-RAG
    - [ ] some idea about improving architecture but need to talk to tulika

### 2026-09-08 One-on-One

> [!summary] This meeting in short
> Read MedRAGChecker and Contradictions in Context. Wrote the methodology of the AMG-RAG extension and a proposal (Evidence-Scoped Revision).

*Original: [Notion page](https://app.notion.com/p/3dad68da32f080808b85f9233ec13aa0)*

- reading papers:
    - [x] [[MedRAGChecker]] 
    - [x] [[Contradictions in Context - Medical RAG]] 

writing methodology of the our AMG-RAG extension:

> [!attachment] File in Notion: **Auditing_Evidence_Sections_1_to_3_with_Bracket_Citations.pdf** (452.9 KiB) - [open the Notion page](https://app.notion.com/p/3dad68da32f080808b85f9233ec13aa0)

current proposal for my own research

> [!attachment] File in Notion: **Evidence_Scoped_Revision_Proposal.pdf** (86.8 KiB) - [open the Notion page](https://app.notion.com/p/3dad68da32f080808b85f9233ec13aa0)

### 2026-09-01 One-on-One

*Original: [Notion page](https://app.notion.com/p/3ced68da32f0800f93efc2133ea62a8b)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-09-01 One-on-One|AR - Meeting Log]]  
**Notes from this meeting:** [[AMG-RAG - Role of LLM, KG and Internet]]

### 2026-08-25 One-on-One

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[AR 7 - Meeting Log#2026-08-25 One-on-One|AR - Meeting Log]], [[Creativity in Multi-Agent Debate#2026-08-25 One-on-One|Creativity in Multi-Agent Debate]], [[Benchmarks and Datasets#2026-08-25 One-on-One|Benchmarks and Datasets]], [[Agent Architectures and Frameworks#2026-08-25 One-on-One|Agent Architectures and Frameworks]], [[Creativity and Exploration#2026-08-25 One-on-One|Creativity and Exploration]]  
**Notes from this meeting:** [[AMG-RAG Phase Evolution Report]], [[AMG-RAG Five Best Answers]], [[AMG-RAG Final Conclusion]], [[AMG-RAG Phase 11 - Method Note]], [[AMG-RAG Phases Comparison Tables]]

### 2026-08-13 One-on-One (13 and 20 Aug)

*Original: [Notion page](https://app.notion.com/p/3bbd68da32f080b9b7e7f143d7fa3505)*  
*Other parts of this meeting:* [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|Creativity in Multi-Agent Debate]], [[Biomedicine and Drug Discovery#2026-08-13 One-on-One (13 and 20 Aug)|Biomedicine and Drug Discovery]]  
**Notes from this meeting:** [[AMG-RAG Phase 9 - How It Works]], [[AMG-RAG Phases Report (Scores)]]

- [ ] how they search in pubmed ?
    - base version only search seperately
    - we have added multip search together
    - i have to add confidence also

#### New Results for AMG-RAG

> [!attachment] File in Notion: **adrclin_phase45_QA.md** (194.5 KiB) - [open the Notion page](https://app.notion.com/p/3bbd68da32f080b9b7e7f143d7fa3505)

### 2026-08-04 One-on-One

*Original: [Notion page](https://app.notion.com/p/3b3d68da32f08075814ed771e3e9af2b)*  
*Other parts of this meeting:* [[Reversible Exploration (ARIADNE and Retro-Fork)#2026-08-04 One-on-One|Reversible Exploration (ARIADNE and Retro-Fork)]]

#### Meeting
- [ ] Compare the AMG-RAG with Mindmap

### 2026-07-21 One-on-One

*Original: [Notion page](https://app.notion.com/p/3a4d68da32f080caaa03d4375e78da67)*  
*Other parts of this meeting:* [[Claim-Level Conflict in Debate#2026-07-21 One-on-One|Claim-Level Conflict in Debate]], [[Autonomous Research Agents#2026-07-21 One-on-One|Autonomous Research Agents]]

- [x] run the base line for BC children

### 2026-07-14 One-on-One

*Original: [Notion page](https://app.notion.com/p/39ed68da32f08065ba4ed44bdd4dea9d)*  
*Other parts of this meeting:* [[Claim-Level Conflict in Debate#2026-07-14 One-on-One|Claim-Level Conflict in Debate]]

- [ ] running AMG-RAG

### 2026-06-23 One-on-One

*Original: [Notion page](https://app.notion.com/p/388d68da32f080e0a389d4f9502a1fd1)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-23 One-on-One|Multi-Agent Debate]], [[Early Exploration (May-June 2026)#2026-06-23 One-on-One|Early Exploration (May-June 2026)]], [[Neuro-Symbolic Reasoning and Argumentation#2026-06-23 One-on-One|Neuro-Symbolic Reasoning and Argumentation]], [[Skills and Tools#2026-06-23 One-on-One|Skills and Tools]]

What i have dont
- [x] review of the [[AMG-RAG - Paper Review Notes]]

### 2026-06-09 One-on-One

*Original: [Notion page](https://app.notion.com/p/37ad68da32f080bcae3cdec43f9f2229)*  
*Other parts of this meeting:* [[Multi-Agent Debate#2026-06-09 One-on-One|Multi-Agent Debate]], [[Early Exploration (May-June 2026)#2026-06-09 One-on-One|Early Exploration (May-June 2026)]], [[RAG and Knowledge Graphs#2026-06-09 One-on-One|RAG and Knowledge Graphs]], [[Skills and Tools#2026-06-09 One-on-One|Skills and Tools]]

#### Next week plan
- [ ] follow up with avnee and tulika
- [x] survey of retriveal among documentations
    - Done list 
        - [[MedGraphRAG]] (It is too complicated)
        - [[GraphCheck]] (it uses GNN)
        - [[AMG-RAG - Paper Summary]] (it is good i think it is more straight forward and close to our approach)

### 2026-06-02 One-on-One

*Original: [Notion page](https://app.notion.com/p/374d68da32f080f1a6f5c6b4bb9b81b0)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-06-02 One-on-One|Early Exploration (May-June 2026)]], [[Creativity and Exploration#2026-06-02 One-on-One|Creativity and Exploration]], [[Skills and Tools#2026-06-02 One-on-One|Skills and Tools]], [[Multi-Agent Debate#2026-06-02 One-on-One|Multi-Agent Debate]]

#### Next week plan
- [ ] BC childeren project
    - [x] a meeting in 4th of june
        - We are trying to run it officially
