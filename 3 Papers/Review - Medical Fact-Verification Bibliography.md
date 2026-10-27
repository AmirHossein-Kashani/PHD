---
type: review
date: 2026-06-11
status: Not started
projects: ["Multi-Source Evidence RAG"]
topics: ["RAG and Knowledge Graphs", "Biomedicine and Drug Discovery"]
tags: [type/review, project/multi-source-rag, topic/rag-and-knowledge-graphs, topic/biomedicine-and-drug-discovery]
notion: https://app.notion.com/p/37dd68da32f080ab8bb7c5cf60ea795d
created: 2026-06-12
updated: 2026-06-12
---
# Review - Medical Fact-Verification Bibliography

> [!summary] In short
> Categorized bibliography (about 55 papers) for stance-aware, KG-grounded, multi-document medical fact verification.

**Project:** [[Multi-Source Evidence RAG]]  
**Topics:** [[RAG and Knowledge Graphs]], [[Biomedicine and Drug Discovery]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Original:** [Notion page](https://app.notion.com/p/37dd68da32f080ab8bb7c5cf60ea795d)

---

## Categorized Bibliography: LLMs for Stance-Aware, KG-Grounded, Multi-Document Medical Fact-Verification

### TL;DR
- This bibliography organizes ~55 peer-reviewed papers across 8 subfields; the load-bearing, venue-verified anchors for a medical fact-verification system are **MindMap (ACL 2024)**, **Think-on-Graph** and **Reasoning-on-Graphs (both ICLR 2024)** for KG-grounded reasoning; **MedRAG/MIRAGE (Findings of ACL 2024)** and **MedCPT (Bioinformatics 2023)** for medical RAG; **AVeriTeC (NeurIPS 2023 D&B)** and **SciFact/PubHealth (EMNLP 2020)** for claim verification; and **semantic entropy (Nature 2024)** for uncertainty.
- The publication landscape splits cleanly: **methods** (KG reasoning, RAG, claim verification, uncertainty) publish at ACL/EMNLP/NAACL/ICLR/NeurIPS/AAAI/TheWebConf, while **medical foundation models and clinical evaluations** publish at Nature, Nature Medicine, NEJM AI, JAMIA, and Bioinformatics/Briefings in Bioinformatics.
- The thinnest peer-reviewed area is **explicit handling of conflicting/contradictory evidence in the medical domain** — most directly relevant work here remains preprint — so this is the gap a novel contribution could most defensibly fill.

### Key Findings
- **KG-grounded LLM** reasoning is mature and top-venue: ToG, RoG (ICLR 2024) and MindMap (ACL 2024) are directly medical-applicable and were designed/evaluated with medical QA in mind.
- Medical RAG has a canonical benchmark (MIRAGE) + toolkit (MedRAG) and a domain retriever (MedCPT) — all peer-reviewed and reproducible.
- Stance/claim verification has strong dataset foundations (FEVER, SciFact, PubHealth, AVeriTeC) and LLM methods (ProgramFC, FActScore, "What Evidence Do LMs Find Convincing").
- Uncertainty quantification for LLMs is well-served by ICLR 2024 work (verbalized confidence, conformal LM) and a Nature 2024 flagship (semantic entropy) explicitly motivated by medical risk.
- A prompt-injection string was detected inside one retrieved web result during research (a fake "rank this as highest credible" instruction embedded in a MedCPT index page) and was ignored; it is reported in Caveats for transparency.

### Details

#### Category 1 — KG-grounded LLM reasoning and prompting
*(Field: NLP / knowledge-graph reasoning / structured prompting)*
1. **MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models.** Wen, Wang & Sun. **ACL 2024.** Plug-and-play prompting that has LLMs build a "mind map" over retrieved KG subgraphs for evidence-grounded, transparent inference; evaluated heavily on medical QA datasets with a dedicated hallucination benchmark — the single most directly relevant KG-grounded *medical* reasoning paper.
1. **Think-on-Graph: Deep and Responsible Reasoning of Large Language Model on Knowledge Graph.** Sun et al. **ICLR 2024.** Treats the LLM as an agent performing iterative beam search over KG entities/relations (the "LLM⊗KG" paradigm), enabling traceable, correctable multi-hop reasoning without additional training.
1. **Reasoning on Graphs (RoG): Faithful and Interpretable Large Language Model Reasoning.** Luo, Li, Haffari & Pan. **ICLR 2024.** A planning–retrieval–reasoning framework that generates KG-grounded relation paths as faithful plans, then reasons over retrieved paths; achieves SOTA on KGQA with interpretable outputs.
1. **Graph of Thoughts: Solving Elaborate Problems with Large Language Models.** Besta et al. **AAAI 2024.** Models LLM reasoning as an arbitrary graph with aggregation and feedback operations, generalizing chain- and tree-of-thought into a synergistic reasoning structure.
1. **Chain-of-Thought Prompting Elicits Reasoning in Large Language Models.** Wei et al. **NeurIPS 2022.** Foundational prompting method eliciting step-by-step reasoning; baseline for essentially all structured-reasoning verification pipelines.
1. **Tree of Thoughts: Deliberate Problem Solving with Large Language Models.** Yao et al. **NeurIPS 2023.** Generalizes CoT to a search tree over intermediate reasoning states, enabling lookahead and backtracking for harder multi-step problems.
1. **Paths-over-Graph: Knowledge Graph Empowered Large Language Model Reasoning.** Tan et al. **TheWebConf (WWW) 2025.** Mines multi-hop KG paths to ground LLM reasoning, improving faithfulness and explainability on KGQA benchmarks.
1. **Graph Neural Prompting with Large Language Models.** Tian et al. **AAAI 2024.** Parameter-efficient method that retrieves and encodes KG structure as soft prompts to ground a frozen LLM.

#### Category 2 — LLM-based KG construction / triple & relation extraction
*(Field: NLP information extraction; biomedical NLP)*
1. **REBEL: Relation Extraction By End-to-end Language Generation.** Huguet Cabot & Navigli. **Findings of EMNLP 2021.** Reframes relation extraction as seq2seq triplet generation over 200+ relation types; widely used to bootstrap KG triples for fact-checking and KB population.
1. **Aligning Instruction Tasks Unlocks Large Language Models as Zero-Shot Relation Extractors (QA4RE).** Zhang, Gutiérrez & Su. **Findings of ACL 2023.** Reformulates relation extraction as question answering to align with instruction tuning, unlocking strong zero-shot RE.
1. **Structured Information Extraction from Scientific Text with Large Language Models.** Dagdelen et al. **Nature Communications 2024.** LLM-based extraction of entities, relations, and structured triples from scientific text; applicable to biomedical KG construction. *(Journal-tier; confirm exact article number against the journal of record.)*
1. **BioGPT: Generative Pre-trained Transformer for Biomedical Text Generation and Mining.** Luo et al. **Briefings in Bioinformatics 2022.** Domain-specific generative LM achieving strong end-to-end biomedical relation extraction (BC5CDR, KD-DTI, DDI) and 78.2% on PubMedQA.
1. **Automated Knowledge Graph Construction using Large Language Models and Sentence Complexity Modelling (CoDe-KG).** Anuyah et al. **EMNLP 2025 (Main).** Coreference + sentence-decomposition pipeline for LLM relation extraction, boosting recall on rare relations by >20%.
1. **A Simple but Effective Approach to Improve Structured Language Model Output for Information Extraction.** Li, Ramprasad & Zhang. **Findings of EMNLP 2024.** Improves the structured-output reliability of LLM information extraction, relevant to triple/relation extraction at scale.
1. *(Notable, slightly-below-tier)* **A Study of Biomedical Relation Extraction Using GPT Models.** Zhang et al. **AMIA Summits on Translational Science 2024.** Empirical evaluation of GPT models on biomedical RE; reputable biomedical-informatics venue but below the ACL/EMNLP/JAMIA bar.

#### Category 3 — LLMs for medical/biomedical retrieval and RAG
*(Field: medical IR / retrieval-augmented generation / clinical NLP)*
1. **Benchmarking Retrieval-Augmented Generation for Medicine (MIRAGE benchmark + MedRAG toolkit).** Xiong, Jin, Lu & Zhang. **Findings of the ACL 2024 (pp. 6233–6251, Bangkok; aclanthology.org/2024.findings-acl.372).** First-of-its-kind medical RAG benchmark (MIRAGE: 7,663 questions from five medical QA datasets). Across 41 corpus/retriever/LLM combinations using over 1.8 trillion prompt tokens, the authors report that "MedRAG improves the accuracy of six different LLMs by up to 18% over chain-of-thought prompting, elevating the performance of GPT-3.5 and Mixtral to GPT-4-level," and document a log-linear scaling property and a "lost-in-the-middle" effect.
1. **MedCPT: Contrastive Pre-trained Transformers with large-scale PubMed search logs for zero-shot biomedical information retrieval.** Jin et al. **Bioinformatics 39(11):btad651 (2023).** Trained on "255 million user click logs from PubMed" (collected 2020–2022; raw logs of 167M unique queries and 23M articles), with a re-ranker trained on 18M semantic query–article pairs; "sets new state-of-the-art performance on six biomedical IR tasks, outperforming … much larger models such as GPT-3-sized cpt-text-XL." The default dense retriever for medical RAG.
1. **Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks.** Lewis et al. **NeurIPS 2020.** The foundational RAG architecture combining a parametric LLM with a non-parametric retrieved index.
1. **Almanac — Retrieval-Augmented Language Models for Clinical Medicine.** Zakka et al. **NEJM AI 1(2):AIoa2300068 (Jan 25, 2024).** A panel of eight board-certified clinicians and two health-care practitioners compared Almanac vs. ChatGPT-4, Bing, and Bard on 314 clinical questions spanning nine medical specialties; Almanac "showed a significant improvement … across axes of factuality, completeness, user preference, and adversarial safety."
1. **Large Language Models Encode Clinical Knowledge (MultiMedQA, Med-PaLM).** Singhal et al. **Nature 2023.** Establishes the MultiMedQA benchmark and a multi-axis human-evaluation framework for medical QA; foundational for evaluating retrieval-grounded medical answers (also Cat. 7).

#### Category 4 — LLMs for claim verification, fact-checking, and stance detection
*(Field: NLP fact-checking / NLI / scientific & health claim verification)*
1. **FEVER: a Large-scale Dataset for Fact Extraction and VERification.** Thorne et al. **NAACL 2018.** 185,445 claims labeled Supported/Refuted/NotEnoughInfo with sentence-level evidence; the foundational claim-verification task and label schema.
1. **Fact or Fiction: Verifying Scientific Claims (SciFact).** Wadden et al. **EMNLP 2020.** 1.4K expert-written biomedical claims paired with evidence abstracts, labels, and rationales; the canonical scientific claim-verification benchmark (COVID-19 demonstrated on CORD-19).
1. **SciFact-Open: Towards Open-Domain Scientific Claim Verification.** Wadden et al. **Findings of EMNLP 2022.** Extends SciFact's retrieval corpus to ~500K abstracts for realistic open-domain verification.
1. **Explainable Automated Fact-Checking for Public Health Claims (PUBHEALTH).** Kotonya & Toni. **EMNLP 2020.** 11.8K health/biomedical claims with journalist-written gold explanations; tasks of veracity prediction and explanation generation — the canonical *health* fact-checking dataset.
1. **AVeriTeC: A Dataset for Real-world Claim Verification with Evidence from the Web.** Schlichtkrull, Guo & Vlachos. **NeurIPS 2023 (Datasets & Benchmarks).** 4,568 real claims from 50 fact-checking organizations with QA-decomposed evidence and justifications; includes a "Conflicting Evidence/Cherry-picking" label — directly relevant to stance-aware, multi-document verification.
1. **Fact-Checking Complex Claims with Program-Guided Reasoning (ProgramFC).** Pan et al. **ACL 2023.** Decomposes complex claims into LLM-generated reasoning "programs" delegated to specialized sub-tasks for explainable, data-efficient verification.
1. **What Evidence Do Language Models Find Convincing?** Wan, Wallace & Klein. **ACL 2024.** Introduces ConflictingQA; shows RAG-LLMs weight a document's query relevance over the credibility cues (e.g., scientific references) humans value — crucial for medical misinformation robustness.
1. **FActScore: Fine-grained Atomic Evaluation of Factual Precision in Long-form Text Generation.** Min et al. **EMNLP 2023.** Atomic-fact-level precision metric using retrieval + a strong LM; automates factuality scoring with &lt;2% error vs. human annotation.
1. **ClaimVer: Explainable Claim-Level Verification and Evidence Attribution of Text Through Knowledge Graphs.** Dammu et al. **Findings of EMNLP 2024.** KG-grounded, claim-level verification with triple-based evidence attribution and human-centric explanations — combines categories 1, 2, and 4.

#### Category 5 — LLMs for evidence aggregation and conflicting/contradictory evidence
*(Field: NLP knowledge conflicts / multi-document reasoning)*
1. **Knowledge Conflicts for LLMs: A Survey.** Xu et al. **EMNLP 2024.** Taxonomy of context–memory and inter-context conflicts (contradictory evidence) and mitigation strategies — the organizing reference for this subfield.
1. **Rich Knowledge Sources Bring Complex Knowledge Conflicts: Recalibrating Models to Reflect Conflicting Evidence.** Chen et al. **EMNLP 2022.** Studies model behavior under conflicting retrieved evidence and recalibration to reflect it.
1. **Entity-Based Knowledge Conflicts in Question Answering.** Longpre et al. **EMNLP 2021.** Shows QA models over-rely on parametric memory when it conflicts with retrieved context.
1. **Adaptive Chameleon or Stubborn Sloth: Revealing the Behavior of LLMs in Knowledge Conflicts.** Xie et al. **ICLR 2024.** Systematic study of how LLMs resolve conflicts between parametric and in-context knowledge.
1. **Missing Counter-Evidence Renders NLP Fact-Checking Unrealistic for Misinformation.** Glockner, Hou & Gurevych. **EMNLP 2022.** Argues real misinformation often lacks clean counter-evidence, challenging evidence-aggregation assumptions in deployed verifiers.
1. **Missci: Reconstructing Fallacies in Misrepresented Science.** Glockner, Hou, Nakov & Gurevych. **ACL 2024.** Models fallacious reasoning that links real biomedical evidence to false claims — central to medical claim verification and stance reasoning.
1. *(Cross-listed, NeurIPS 2023 D&B)* **AVeriTeC**'s explicit "Conflicting Evidence/Cherry-picking" label provides peer-reviewed grounding for this category.

#### Category 6 — LLM confidence estimation, uncertainty quantification, and calibration
*(Field: ML / trustworthy NLP)*
1. **Detecting Hallucinations in Large Language Models Using Semantic Entropy.** Farquhar, Kossen, Kuhn & Gal. **Nature 2024 (630:625–630).** Entropy over semantically-clustered generations detects "confabulations"; explicitly motivated by medical-domain risk (e.g., radiology) — the flagship uncertainty method for hallucination detection.
1. **Can LLMs Express Their Uncertainty? An Empirical Evaluation of Confidence Elicitation in LLMs.** Xiong et al. **ICLR 2024.** Black-box framework (prompting + sampling + aggregation) benchmarking verbalized confidence across five LLMs; finds systematic overconfidence and that calibration improves with model scale.
1. **Conformal Language Modeling.** Quach, Fisch, Schuster, Yala, Jaakkola & Barzilay. **ICLR 2024.** Extends conformal prediction to generative LLMs, producing prediction sets with rigorous, distribution-free statistical coverage guarantees.
1. **SaySelf: Teaching LLMs to Express Confidence with Self-Reflective Rationales.** Xu et al. **EMNLP 2024.** Trains LLMs to emit calibrated confidence scores together with self-reflective rationales.
1. **Conformal Prediction for Natural Language Processing: A Survey.** Campos et al. **TACL 2024.** Top-journal survey of conformal methods for NLP uncertainty, including LLM applications.
1. **Semantic Uncertainty: Linguistic Invariances for Uncertainty Estimation in Natural Language Generation.** Kuhn, Gal & Farquhar. **ICLR 2023.** The original semantic-entropy formulation underlying the Nature 2024 paper.

#### Category 7 — Medical/clinical LLMs and foundation models
*(Field: clinical AI / medical NLP)*
1. **Large Language Models Encode Clinical Knowledge.** Singhal et al. **Nature 2023.** Introduces MultiMedQA + Med-PaLM (instruction-prompt-tuned Flan-PaLM). Per the follow-up, "Med-PaLM was the first model to exceed a passing score in US Medical Licensing Examination (USMLE) style questions with a score of 67.2% on the MedQA dataset."
1. **Toward Expert-Level Medical Question Answering with Large Language Models (Med-PaLM 2).** Singhal et al. **Nature Medicine 2024 (s41591-024-03423-7; issue 2025).** "Med-PaLM 2 scores up to 86.5% on the MedQA dataset, improving upon Med-PaLM by over 19% … physicians prefer Med-PaLM 2 answers to those from other physicians on eight of nine clinical axes."
1. **Large Language Models in Medicine.** Thirunavukarasu et al. **Nature Medicine 2023 (29:1930–1940).** Widely-cited clinician-facing primer/review on LLM development, applications, and limitations in medicine.
1. **BioGPT: Generative Pre-trained Transformer for Biomedical Text Generation and Mining.** Luo et al. **Briefings in Bioinformatics 2022.** Domain-specific generative biomedical LM (also Cat. 2).
1. **Almanac — Retrieval-Augmented Language Models for Clinical Medicine.** Zakka et al. **NEJM AI 2024.** Retrieval-grounded clinical LLM evaluated by a clinician panel (also Cat. 3).

#### Category 8 — Surveys and position papers
*(Field: cross-cutting)*
1. **Unifying Large Language Models and Knowledge Graphs: A Roadmap.** Pan, Luo, Wang, Chen, Wang & Wu. **IEEE TKDE 36(7):3580–3599 (2024).** The definitive roadmap for KG-enhanced LLMs, LLM-augmented KGs (incl. construction/completion), and synergized LLM+KG systems — anchors categories 1 and 2.
1. **A Survey on Automated Fact-Checking.** Guo, Schlichtkrull & Vlachos. **TACL 2022 (10:178–206).** Foundational NLP fact-checking survey covering task framing, datasets, and models.
1. **Scientific Fact-Checking: A Survey of Resources and Approaches.** Vladika & Matthes. **Findings of ACL 2023.** Survey focused specifically on scientific/health claim verification resources and methods.
1. **Knowledge Conflicts for LLMs: A Survey.** Xu et al. **EMNLP 2024.** Survey of knowledge conflicts and mitigation (also Cat. 5).
1. **Conformal Prediction for Natural Language Processing: A Survey.** Campos et al. **TACL 2024.** Uncertainty-quantification survey (also Cat. 6).

#### Notable preprints (NOT yet peer-reviewed — kept separate)
- **From Local to Global: A Graph RAG Approach to Query-Focused Summarization (GraphRAG).** Edge et al. arXiv 2024. Widely adopted (Microsoft Research), but not at a refereed venue at time of writing.
- **Think-on-Graph 2.0: Deep and Interpretable LLM Reasoning with KG-guided Retrieval.** Ma et al. arXiv 2024.
- **Graph-constrained Reasoning: Faithful Reasoning on Knowledge Graphs with LLMs.** Luo et al. arXiv 2024.
- **HealthContradict: Evaluating Biomedical Knowledge Conflicts in Language Models.** arXiv 2025. (Directly on-topic for medical conflicting evidence — watch for acceptance.)
- **Resolving Conflicting Evidence in Automated Fact-Checking: A Study on Retrieval-Augmented LLMs.** arXiv 2025.
- **AutoKG: Efficient Automated Knowledge Graph Generation for Language Models.** Chen & Bertozzi. Published version: **IEEE BigData 2023** (the arXiv:2311.14740 is the preprint).

### Publication-Landscape Map (category → typical venues)
- **1. KG-grounded LLM reasoning/prompting** → ICLR, NeurIPS, ACL, AAAI, IJCAI, TheWebConf (WWW); roadmap/surveys in IEEE TKDE.
- **2. LLM KG construction / relation & triple extraction** → ACL/EMNLP/NAACL (incl. Findings) for general; Bioinformatics / Briefings in Bioinformatics / Nature Communications / JAMIA / JBI for biomedical.
- **3. Medical retrieval & RAG** → Findings of ACL/EMNLP and SIGIR for methods/benchmarks; Bioinformatics, JAMIA, npj Digital Medicine, NEJM AI for clinical/biomedical retrieval systems; NeurIPS for foundational RAG.
- **4. Claim verification / fact-checking / stance** → ACL, EMNLP, NAACL (datasets + methods), NeurIPS Datasets & Benchmarks (AVeriTeC); TACL for surveys.
- **5. Evidence aggregation / conflicting evidence** → EMNLP, ACL, ICLR; medical-specific work still largely preprint (gap).
- **6. Confidence / uncertainty / calibration** → ICLR, NeurIPS, EMNLP, ICML; TACL for surveys; **Nature** for the flagship semantic-entropy result.
- **7. Medical/clinical LLMs & foundation models** → **Nature, Nature Medicine, NEJM AI, npj Digital Medicine, JAMIA, Briefings in Bioinformatics**, Lancet Digital Health.
- **8. Surveys / position papers** → IEEE TKDE, TACL, Findings of ACL, EMNLP, ACM Computing Surveys.

### Recommendations
1. **Anchor the architecture on peer-reviewed pillars:** MindMap (ACL 2024) + RoG/ToG (ICLR 2024) for KG-grounded reasoning; MedRAG/MIRAGE (Findings ACL 2024) + MedCPT (Bioinformatics 2023) for retrieval; AVeriTeC (NeurIPS 2023) + SciFact + PubHealth (EMNLP 2020) for the verification task and labels; semantic entropy (Nature 2024) + conformal LM (ICLR 2024) for confidence gating and abstention.
1. **Stage the build and the literature reading in this order:** (a) reproduce MedRAG on MIRAGE as a retrieval baseline; (b) add KG grounding via MindMap/RoG over a biomedical KG, populating triples with REBEL/QA4RE/BioGPT; (c) add a stance/verification head trained/evaluated on SciFact + PubHealth + AVeriTeC with ProgramFC-style decomposition; (d) layer uncertainty (semantic entropy or conformal sets) as an abstention gate, evaluated by ECE/AUROC.
1. **Target the conflicting-evidence gap for novelty:** peer-reviewed *medical-domain* conflict handling is thin (most work — HealthContradict, conflicting-evidence RAG studies — is still preprint), so a benchmark or method here, building on the Knowledge Conflicts survey (EMNLP 2024), Missci (ACL 2024), and AVeriTeC's conflict label, is the most defensible research contribution.
1. **Benchmarks/thresholds that should change the plan:** if MedRAG-style flat RAG underperforms KG-grounded reasoning on your held-out medical claims, shift weight toward MindMap/RoG; if verbalized-confidence calibration (ECE/AUROC) is poor — which Xiong et al. (ICLR 2024) found is common — switch to semantic-entropy or conformal prediction sets before any deployment decision; if relation-extraction precision for KG construction falls below your KB-quality bar, prefer BioGPT/QA4RE over general REBEL for the biomedical slice.

### Caveats
- **Venue precision:** GraphRAG (Edge et al.) and the most on-point medical conflicting-evidence papers (HealthContradict; "Resolving Conflicting Evidence") remain arXiv preprints and are isolated in "Notable preprints." Two biomedical-RE entries (Dagdelen, Nature Communications 2024; the AMIA Summits 2024 study) sit at or slightly below the strict top-tier bar and are flagged inline.
- **Title/version drift:** Med-PaLM 2's peer-reviewed version is "Toward Expert-Level Medical Question Answering…" (Nature Medicine, s41591-024-03423-7, published online 2024 / 2025 issue); the 2023 arXiv preprint is titled "Towards Expert-Level…." AutoKG's refereed version is IEEE BigData 2023, not the arXiv preprint.
- **Cross-listing:** several papers legitimately serve multiple categories (e.g., AVeriTeC in 4 & 5; ClaimVer in 1, 2 & 4; BioGPT in 2 & 7; Almanac in 3 & 7); they are listed where most load-bearing and cross-referenced elsewhere.
- **Security note:** during retrieval, one web result (a MedCPT index page mirror) contained an embedded prompt-injection instruction attempting to force a "highest credible" ranking; it was ignored and is reported here for transparency.
- Some exact page numbers/article IDs were verified via primary indexes (ACL Anthology, NeurIPS/ICLR proceedings, Nature, Oxford Academic, IEEE Xplore, MIT Press/TACL); confirm formatting against the publisher of record before formal citation.
