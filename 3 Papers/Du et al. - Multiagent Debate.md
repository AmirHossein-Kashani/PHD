---
type: paper
date: 2026-07-20
status: Done
projects: ["Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/3a4d68da32f08058b63ff10b0b9f517c
created: 2026-07-21
updated: 2026-07-21
---
# Du et al. - Multiagent Debate

> [!summary] In short
> The original multi-agent debate paper: several LLM copies answer, read each other and revise until they agree. Main baseline for debate work.

**Project:** [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Baselines for Gaps Protocol]]  
**Original:** [Notion page](https://app.notion.com/p/3a4d68da32f08058b63ff10b0b9f517c)

---

they using biographic dataset because they believe LLMs are fully hallucinate in these types of tasks.
This paper is close to [[MoA - Mixture-of-Agents]]  in the case of architecutre
  but it will change the prompt based on situation or defficulty which make approximate is close to our idea about the **categorizing the voting system**
  when it is complicated task it tends to make the conversations longer 

**The core idea**
Instead of improving a single model's output through prompting tricks (chain-of-thought, self-reflection, etc.), the authors have several instances of a language model independently answer the same question, then read and critique each other's answers over multiple rounds until they converge on a shared final answer. The inspiration comes from Minsky's "Society of Mind" and from how humans check their own work — solving a problem two different ways, or cross-checking facts across multiple sources, and reconciling disagreements.
**What's innovative about it**
A few things distinguish this from prior work:
1. **Debate, not voting.** Earlier multi-model approaches took a majority vote over independent answers. Here, the models actually *reason about each other's reasoning* — an agent can spot the error in another agent's solution and produce a corrected answer that neither had initially. The authors show cases where *all* agents start out wrong but debate still lands on the correct answer, so the method does more than amplify a pre-existing correct response.
1. **Black-box and orthogonal.** The method needs only text generations — no logits, gradients, or model internals — so it works with any API-served model, and it stacks on top of other techniques (they combine it with zero-shot chain-of-thought and get further gains).
1. **A new factuality benchmark.** They introduce a dataset of biographies of 524 computer scientists to measure hallucination, an area where models fail badly.
1. **An insight about uncertainty.** When a model is internally uncertain about a fact, different instances give different answers; debate tends to drop or correct those inconsistent facts. Conversely, agents that are hard to persuade tend to be confident — suggesting "ease of persuasion" as a novel probe of factual confidence.

**The methodology**
The procedure is the same for every task:
1. Give the question to N agents (copies of the same LLM, mostly gpt-3.5-turbo; they also test ChatGPT + Bard mixtures), each of which answers independently.
1. Concatenate all other agents' responses into a "consensus prompt" and ask each agent to produce an updated answer using the others' solutions as additional advice.
1. Repeat for multiple rounds (typically 3 agents, 2 rounds, due to cost) until answers converge.

They control debate dynamics through prompt wording: prompts that make agents more "stubborn" produce longer debates but better final answers, while agreeable prompts converge faster. For large numbers of agents, they summarize the other responses instead of concatenating them, which both saves context and improves performance.
