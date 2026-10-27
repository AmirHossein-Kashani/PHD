---
type: paper
date: 2026-05-14
date-end: 2026-05-15
status: Done
projects: ["Early Exploration (May-June 2026)"]
topics: ["Agent Architectures and Frameworks", "Biomedicine and Drug Discovery", "Autonomous Research Agents"]
notion-tags: ["protein", "paper", "Generative", "LLM"]
tags: [type/paper, project/early-exploration, topic/agent-architectures-and-frameworks, topic/biomedicine-and-drug-discovery, topic/autonomous-research-agents]
notion: https://app.notion.com/p/360d68da32f080c78e42f7c21fb130ff
created: 2026-05-14
updated: 2026-05-15
---
# Virtual Lab

> [!summary] In short
> AI agents (PI, scientists, critic) hold meetings to design SARS-CoV-2 nanobodies (Nature 2025).

**Project:** [[Early Exploration (May-June 2026)]]  
**Topics:** [[Agent Architectures and Frameworks]], [[Biomedicine and Drug Discovery]], [[Autonomous Research Agents]]  
**Notion category:** [[Notion Category - Protein Generation]]  
**Original:** [Notion page](https://app.notion.com/p/360d68da32f080c78e42f7c21fb130ff)

---

> [!attachment] File in Notion: **The Virtual Lab of AI agents_compressed.pdf** (3.4 MiB) - [open the Notion page](https://app.notion.com/p/360d68da32f080c78e42f7c21fb130ff)

### Swanson, Wu, Bulaong, Pak, Zou,
[https://www.nature.com/articles/s41586-025-09442-9](https://www.nature.com/articles/s41586-025-09442-9)

### The Virtual Lab: AI Agents Designing SARS-CoV-2 Nanobodies

## points

#### what human do
- **Define the PI and Scientific Critic** agents (Title, Expertise, Goal, Role)
- **Write a short project description** — the PI uses this to auto-generate the scientist agent team
- **Write meeting agendas** with optional questions and rules
- **Read final summaries and verify decisions** — they don't read the full agent discussions

#### steps of communication
Human agenda → PI opens discussion → Scientist agents respond in turn
  → Scientific Critic critiques → PI synthesizes + asks follow-ups
  → Repeat for N rounds → PI writes final summary → Human reads summary
consider that the human cooperation is not only for pi but also has description of meetings and scientist definition that required

#### General Overview
This Nature paper (Swanson et al., 2025) introduces the **Virtual Lab**, an AI-human research collaboration framework that uses LLM agents to conduct sophisticated, interdisciplinary scientific research. Rather than treating LLMs as simple Q&A tools, the authors created a system where multiple specialized AI agents collaborate (under human guidance) to tackle open-ended research problems.
**The demonstration problem:** Designing new nanobodies (small single-domain antibodies) that bind to recent SARS-CoV-2 variants (KP.3 and JN.1), since the virus has been rapidly evolving resistance to existing therapies.
**Key results:**
- The Virtual Lab built a novel computational pipeline combining three tools: **ESM** (protein language model), **AlphaFold-Multimer** (structure prediction), and **Rosetta** (binding energy calculation)
- It designed **92 mutant nanobodies** starting from 4 known nanobodies (Ty1, H11-D4, Nb21, VHH-72)
- Experimental validation showed **>90% of nanobodies were expressed and soluble**
- **Two standout candidates**: a Nb21 mutant (I77V/L59E/Q87A/R37Q) gained binding to JN.1 and KP.3 variants, and a Ty1 mutant (V32F/G59D/N54S/F32S) gained binding to JN.1
- The whole AI design phase took only **~1–2 hours of agent time** (~$10–20 in token costs), versus weeks for a human team

---

#### The LLM Architecture
The architecture is built around **specialized agents interacting through structured meetings**, powered by GPT-4o.

#### 1. The Agents
Every agent is defined by a prompt specifying four attributes:
- **Title** — the agent's name
- **Expertise** — scientific background
- **Goal** — objective in the project
- **Role** — specific responsibilities

There are three categories of agents:
**Principal Investigator (PI)** — The leader. Defined by the human researcher. Guides discussions, synthesizes input, asks follow-up questions, and writes meeting summaries.
**Scientific Critic** — A dedicated critic agent that catches errors, challenges weak reasoning, and pushes other agents to improve their answers.
**Scientist Agents** — Automatically created by the PI based on a short project description from the human. For the nanobody project, the PI spawned:
- Immunologist
- Machine Learning Specialist
- Computational Biologist

#### 2. The Meetings (Two Types)
**Team Meetings** — All agents discuss a broad research question over *N* rounds:
1. Human writes an agenda
1. PI opens with initial thoughts and guiding questions
1. Each scientist agent responds in turn
1. Scientific Critic critiques
1. PI synthesizes, asks follow-ups, runs next round
1. After *N* rounds, PI summarizes and answers the agenda

**Individual Meetings** — A single scientist agent tackles a specific task (e.g., writing code), with the Scientific Critic providing iterative feedback. Used for focused, implementation-level work.

#### 3. Parallel Meetings (Quality Booster)
To improve answer quality, the *same* meeting is run **multiple times in parallel** with high LLM temperature (0.8) for creative diversity. Then a single "merge meeting" with low temperature (0.2) consolidates the best elements from each parallel run into one optimal answer. This is a more sophisticated variant of majority voting.

#### 4. The Five-Phase Workflow Applied to Nanobody Design

| Phase | Meeting Type | Purpose |
|---|---|---|
| 1. Team Selection | Individual (PI) | PI creates the scientist agent team |
| 2. Project Specification | Team | Decide nanobodies vs. antibodies, which to modify |
| 3. Tools Selection | Team | Pick computational tools (chose ESM + AlphaFold-Multimer + Rosetta) |
| 4. Tools Implementation | Individual meetings | Each scientist writes code for one tool, critiqued by Scientific Critic |
| 5. Workflow Design | Individual (PI) | PI designs the overall pipeline and scoring formula |

#### Key Design Insights
- **Human role is minimal but vital** — the human wrote just 1,596 words (1.3% of total content); agents wrote 122,462 words (98.7%). Humans provide high-level guidance, agenda-setting, and verification.
- **Distinct agent identities matter** — ablation experiments showed generic agents without specific roles argued more and produced worse answers.
- **Multi-agent ≈ enhanced chain-of-thought** — extended conversations between differently-specialized agents extract reasoning capabilities similar to chain-of-thought prompting but with the added benefit of multiple perspectives.

#### Limitations Acknowledged
- **Knowledge cutoffs** — agents may not know the latest literature (e.g., they used AlphaFold-Multimer instead of the newer AlphaFold 3)
- **Prompt engineering required** — vague agendas yield vague answers
- **Hallucinations** — agents can invent incorrect facts, requiring human verification

### The Paper Has No Training Phase
This is an important and often misunderstood point. **The Virtual Lab does not train any LLM or neural network.** It is purely an *inference-time orchestration framework* built on top of pre-existing, pre-trained models.

#### What's Actually Used (All Pre-Trained / Off-the-Shelf)

| Component | Role | Origin |
|---|---|---|
| **GPT-4o** (`gpt-4o-2024-08-06`) | Powers all agents (PI, Scientist agents, Critic) | Pre-trained by OpenAI; used via API |
| **ESM** | Scores nanobody mutations (log-likelihood ratios) | Pre-trained protein language model |
| **AlphaFold-Multimer** | Predicts nanobody–spike complex structure | Pre-trained by DeepMind, run via LocalColabFold v1.5.5 |
| **Rosetta** (v3.14) | Computes binding energy via REF15 scoring function | Classical biophysics software — not ML, no training |
