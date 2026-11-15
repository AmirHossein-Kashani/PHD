---
type: project-log
project: Auto Researcher
tags: [type/project-log, project/auto-researcher]
---
# AR 7 - Meeting Log

Part of [[AR 0 - Start Here|Auto Researcher]].

> [!summary] In short
> Everything that was said or reported in meetings about the **Auto Researcher**, newest first. Each entry is one meeting (date + title) with a link to the original Notion page. Meetings are **not separate notes**: their content lives here and in the topic notes. If a meeting also talked about other things, the entry links to the other parts (for example in [[MS - Meeting Log]]).

> [!tip] Add a new meeting
> Copy the `Meeting entry` template to the **top** of the entries below (or into [[MS - Meeting Log]] / a topic note, depending on what the meeting was about).

---

## Entries

### 2026-10-20 One-on-One

> [!summary] This meeting in short
> Introduction of the research paper done; AMG-RAG target benchmarks updated. To discuss: BC Children meeting, exploration vs expectation (jumping ideas vs refinement), research steps, move notes to Obsidian.

*Original: [Notion page](https://app.notion.com/p/3ffd68da32f080759fc4f51afeba7a6b)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-10-20 One-on-One|MS - Meeting Log]], [[Skills and Tools#2026-10-20 One-on-One|Skills and Tools]]

#### What have done
- [ ] introduction completed for research paper completed

#### what to talk and todo
- [ ] Exploration and Expectation 
    - combining jumping idea with refinement
    - how much refinement required how much new idea required
- [ ] I plan for definition of research steps

> [!attachment] File in Notion: **Autoresearcher (6).pdf** (205.3 KiB) - [open the Notion page](https://app.notion.com/p/3ffd68da32f080759fc4f51afeba7a6b)

fix this version and also we want to make it  improve

### 2026-10-14 One-on-One

> [!summary] This meeting in short
> Kaggle results: our approach led in the first cycles, Claude Code won at the end. Blocking rules made agents less ambitious. Related work notes on AI Scientist-v2, AI-Researcher and Kosmos.

*Original: [Notion page](https://app.notion.com/p/3f9d68da32f08078b783ebb810e6a469)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-10-14 One-on-One|MS - Meeting Log]]

#### next week plan
- [ ] [[AR 6 - Paper Draft (Introduction)]]  
- [ ] Finalizing baselines for researcher paper

#### prev week achievments
- Result experiments
    - our approach in the first 4 cycles was much better and improved the base code and reach 17th ranking  with 0.18 + compare to Claude code and arbor base
    - But in the round 10th Claude code win the final with 0.03+ compare to all other and the three other approach converge equally
    - logs shows that our blocking idea criteria lead that model to don’t work so ambitious and pause ideas
    - This competition also is not a very good idea because the amount of final test limited, and even though they get good results at validation they do not get good score in final submission
    - as the agents was able to search they seems cheating a lot rather than be creative, but we have to consider that they improve the public avaialbe answer with 0.24 point which is good to consider.
    - now claude code has ranking of 60(two days ago was 40th) and other are something around 200th. these scores are higher than the first person at two weeks ago  

Related works
- AI-Scientist 2
    - they tried to forced steps of research
        - Stage 1 – Preliminary Investigation: Establishing initial feasibility and correctness through a minimal working prototype based on the generated research idea.
        - Stage 2 – Hyperparameter Tuning: Refining the initial implementation by optimizing critical hyperparameters (e.g., learning rate, epochs) to create a robust experimental baseline.
        - Stage 3 – Research Agenda Execution: Systematically implementing the core research agenda based on the tuned baseline.
        - Stage 4 – Ablation Studies: Systematically assessing the importance of various research components, providing rigorous support for the main experimental findings.
    - They have consider each Stage as seperate graph and then select the best node as root of next stage
    - They believe that the most important section is that let the model create ideas without considering implementation and freely generate topics or abstract and high level solutions 
    ![[2026-10-14-one-on-one-1.webp]]

- AI-Researcher
    Scientist-Bench tests research agents on 22 well-known AI papers (2022–2024). It gives them references and datasets, then uses LLM reviewers to compare the paper they produce against the original. Its main weaknesses:
    - Data contamination: the target papers are likely in the models’ training data.
    - Reference lists can identify the original paper even after anonymization.
    - Agents can search GitHub and arXiv, and nothing blocks them from finding the original paper.
    - Judging is done only by LLMs, with no human evaluation.
    - Results vary widely depending on which LLM judges.

    Steps
    - 15-20 scientific papers as input
    - Literature review: create atomic section for ideas and choose datasets and planning , 5 idea generation to solve the problem but only the best go for implimentation
    - Implementation: develop and run for small number of epoches and then talk with advisor to continue or not
    - Scientific writing 

I think the most important idea of this paper is giving initial paper to llms to start the research, but for the other cases specially refinement of idea is too weak

- Kosmos
    focuses on multi agent running 200 agents (they have similar steps of research) 
    shared memory: summary
    citation and steps logging
    the main contribution is separation i of context

### 2026-09-29 One-on-One

> [!summary] This meeting in short
> More cycles, Claude Code baseline, Phase 2 (plan, implement, plan, implement). AMG-RAG: make repo, add trajectory. Benchmarks: PGxQA, HealthContradict, PGxCorpus.

*Original: [Notion page](https://app.notion.com/p/3f1d68da32f0807585b6fa7786e244a9)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-09-29 One-on-One|MS - Meeting Log]]

- [ ] what have done , 
    - increasing the cycles, improved the speed of reaching the goal
    - running claude code baseline
    - running phase 2 , which is plan → implementation → plan → implementation

### 2026-09-21 One-on-One

> [!summary] This meeting in short
> First results of our Phase 1 on Arbor vs Arbor default on a Kaggle task: same final score, but Phase 1 was faster, deeper and more efficient. Shared memory of findings and failures was the most useful feature.

*Original: [Notion page](https://app.notion.com/p/3ead68da32f080a3839afa4c8f6bead8)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-09-21 One-on-One|MS - Meeting Log]]

Our new running phase 1 on Arbor

phase 1 include
- revoking ideas
- merging idea (finding duplicate ideas)
- leaf idea and parent history
-  better support of concurrency as it is multi seperate agents support
- full history
- long term memory
- when we are woking on leafs the agent can make a node from somewhere in parental history

- Arbor defualt setting
    - only step planning and coding
    - each round include 12 cycle in each cycle we will add a node 
    - if cycle win it will merge by trunk
    - after each round it will delete tree of though 

results :
- **Final score:** a tie. Base scored 0.325 and Phase 1 0.322, a difference inside leaderboard noise. Neither reached the 0.35 target.
- **Experience:** base had 6 rounds, Phase 1 only 3, and one of those was a leaky first try.
- **Efficiency:** Phase 1 merged 62 % of its cycles against base's 23 %, and used about half the hours on the same proxy.
- **Speed from scratch:** Phase 1 went from a raw baseline to 0.287 in one round. Base needed three rounds to pass 0.275.
- **Key discovery:** Phase 1 found that the Kaggle test spectra are copies of training rows with wrong labels, worth +0.028. Base saw the signal but did not act on it.
- **Honest self-correction:** base found its own data leak and retrained honestly in round 6, which gave its best score.
- **Depth of reasoning:** base trees stayed 2 layers deep. Phase 1 chains went 3 to 5 layers deep.
- **Stopping:** base always used all 12 cycles. Phase 1 stopped after 9 when no active ideas were left.
- **Most useful Phase 1 feature:** shared memory of findings and failures, given to every new idea.
- **Partly useful:** blocking with reopening conditions, which saved cycles once an honest test existed, and evidence verdicts, a clear record that matched the agent's next choices.
- **Never used:** reactivation, duplicate merging, concurrent ideas, parallel workers, and the confidence-based selection policy.
- **No effect:** the automatic evidence router, which found 0 candidate leaves in 10 tries.
- **Why unused:** each round had one agent and started with a fresh tree.
- **Shared limits:** the local proxy was easier than Kaggle, and the neural network is weak on unseen natural products. Both limits held back both versions equally.
- **Next step:** carry the Phase 1 tree and memory across rounds, and add a leaf-selection tool, so the unused parts get tested.

++ Arbor give all entire graph to make decision and it limits ability to deepend the ideas, we only give history of parents and compressed shared knowledge
++ phase 1 can stop when there is no idea

### 2026-09-01 One-on-One

> [!summary] This meeting in short
> Baselines for research-idea agents (IDEAgent, Co-Scientist, RepurAgent, Robin, Biomni). Benchmark idea: DrugMechDB inverted. AMG-RAG reports as sub-notes.

*Original: [Notion page](https://app.notion.com/p/3ced68da32f0800f93efc2133ea62a8b)*  
*Other parts of this meeting:* [[MS - Meeting Log#2026-09-01 One-on-One|MS - Meeting Log]]

| Question | Best representative baseline |
|---|---|
| **Quality + diversity of research ideas** | **IDEAgent** |
| **High-end biomedical hypothesis generation** | **Co-Scientist** |
| **Chemistry hypothesis rediscovery** | **MOOSE-Chem / MOOSE-Chem2** |
| **Tree-search scientific creativity** | **MC-NEST** |
| **Drug repurposing agent** | **RepurAgent** |
| **End-to-end therapeutic discovery** | **Robin** |
| **General biomedical agent capability** | **Biomni** |

What I plan to do
- [ ] summary of what happen to us
- [ ] RepurAgent — extremely useful because it is directly drug repurposing
  Human used

Robin — state-of-the-art end-to-end scientific discovery baseline

The benchmark is **DrugMechDB inverted**. DrugMechDB is a curated set of drug → mechanism → disease paths. We take a disease, **hide every curated drug and every mechanism path for it**, and ask the model to reconstruct them. The hidden material becomes evaluator-only labels.

### 2026-08-25 One-on-One

> [!summary] This meeting in short
> Benchmark ranking for drug repurposing (TRACES DRR, SerenQA, DrugMechDB...). Gap idea: quality-diversity as the main rule of a long-horizon research agent on Arbor. An agent that knows when to go down, up, sideways, merge or return.

*Original: [Notion page](https://app.notion.com/p/3c7d68da32f080afbd74d869eda9829a)*  
*Other parts of this meeting:* [[Creativity in Multi-Agent Debate#2026-08-25 One-on-One|Creativity in Multi-Agent Debate]], [[Benchmarks and Datasets#2026-08-25 One-on-One|Benchmarks and Datasets]], [[Agent Architectures and Frameworks#2026-08-25 One-on-One|Agent Architectures and Frameworks]], [[Creativity and Exploration#2026-08-25 One-on-One|Creativity and Exploration]], [[MS - Meeting Log#2026-08-25 One-on-One|MS - Meeting Log]]

- [ ] Use RL for long horizons
- [ ] loop back to our base model
- [x] finalize the method representations
- [ ] do the fixes

#### learned

##### The original GoT representation (still the conceptual anchor)

If you can do that, you're done: those five sentences *are* your novelty argument and your related-work section. If you can't name the five, that gap is the *only* reading you still owe — and it's finite, maybe a day.
To start you off, here are the nearest neighbors I'd put on the list, none of which is your idea:
- **Nova** — enhances novelty/diversity of LLM-generated *research ideas* via iterative planning and search; it reports increasing the number of unique novel ideas by 3.4× over baseline, and notes prior work found LLMs lack diversity in idea generation. Closest to you on the "diversity of ideas" axis, but not QD, not archive-based, not drug-specific. [arxiv](https://arxiv.org/pdf/2410.14255)
- **Diverse molecular generation** — the "can LLMs generate diverse molecules" line of work operates at the *molecule* level, not the *research-hypothesis* level.
- **QADD** — multi-objective molecular design that fights exactly your diversity-vs-potency tension, but as an RL generator, not an autonomous research agent.
- **Drug-discovery agent systems** — MADD, Mozi, AgentD, CLADD. These orchestrate pipelines; Mozi even uses a parallel dual-stream strategy explicitly to bridge chemical novelty and synthetic realizability, clustering for diversity. They're your closest *applied* neighbors, but they don't treat illumination of a hypothesis space as the core mechanism the way you would. [arXiv](https://arxiv.org/html/2603.03655v1)

Your gap is the vertical bar between all of these: **quality-diversity as the governing principle of a long-horizon autonomous *research* agent, instantiated on Arbor, aimed at a diverse portfolio of drug hypotheses.** I couldn't find that combination, and I looked.
