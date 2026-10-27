---
type: paper
date: 2026-05-25
status: Done
projects: ["Early Exploration (May-June 2026)", "Claim-Level Conflict in Debate"]
topics: ["Multi-Agent Debate"]
tags: [type/paper, project/early-exploration, project/claim-conflict, topic/multi-agent-debate]
notion: https://app.notion.com/p/36cd68da32f0802cb286e15a2c7fdacd
created: 2026-05-26
updated: 2026-05-26
---
# MAPoRL

> [!summary] In short
> Co-train debating agents with RL and extra rewards so they really cooperate instead of pretending.

**Project:** [[Early Exploration (May-June 2026)]], [[Claim-Level Conflict in Debate]]  
**Topics:** [[Multi-Agent Debate]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Multi-Agent LLM Systems 2024-2026]]  
**Original:** [Notion page](https://app.notion.com/p/36cd68da32f0802cb286e15a2c7fdacd)

---

The authors argue that when developing communicative multi-agent systems, the agents must be explicitly co-trained. Without this, standard out-of-the-box LLMs often only simulate or 'pretend' to cooperate, relying on their innate pre-trained behaviors rather than engaging in genuine strategic teamwork. By implementing a post-training co-optimization paradigm, the models learn to adapt to one another, eliciting authentic collaborative behaviors and fully unleashing the real power of multi-agent communication.

they try to add extra reward to system to convince them to influence of the society of llms in cooeprtion

**2. Applying** $\alpha_{0}$  **and** $\alpha_{1}$ **(Agent's Own Revision)**
  These incentives are applied to an agent's answer at Turn $t+1$ if the agent successfully corrects their own wrong answer. The system checks the agent's past and current answer against the past majority opinion:  
  • **Trigger** $\alpha_{0}$ **(Critical Reasoning):**  If Agent's Answer at Turn t was **Wrong**
      ◦ AND Agent's Answer at Turn $t+1$ is **Right**
      ◦ AND the Majority Opinion at Turn t was **Wrong** 
  *Result:* Add $\alpha_{0}$ to the agent's reward.  
  • **Trigger** $\alpha_{1}$ **( Persuadability ):** * If Agent's Answer at Turn t was **Wrong**
      ◦ AND Agent's Answer at Turn t+1 is **Right**
      ◦ AND the Majority Opinion at Turn t was **Right**
  *Result:* Add $\alpha_{1}$ to the agent's reward.  
  **3. Applying** $\beta_{0}$ **and** $\beta_{1}$ **(Influence on Other Agents)**
  These incentives (and penalties) are applied to an agent's answer at Turn t based on whether the overall majority opinion improves or degrades in the *next* turn.  
  • **Trigger** $\beta_{0}$ **(Constructive Influence):**
      ◦ If Majority Opinion at Turn t was **Wrong**
      ◦ AND Majority Opinion at Turn t+1 is **Right**
      ◦ AND Agent's Answer at Turn t was **Wrong**
  *Result:* Add $\beta_{0}$ to the agent's reward.  
  • **Trigger** $\beta_{1}$ **(Persuasive Ability):**
      ◦ If Majority Opinion at Turn t was **Wrong**
      ◦ AND Majority Opinion at Turn t+1 is **Right**
      ◦ AND Agent's Answer at Turn t was **Right**
      ◦ *Result:* Add $\beta_{1}$ to the agent's reward.  

  **4. The Approach**
  Rather than relying on standard Supervised Fine-Tuning (SFT)—which the authors show is inadequate for teaching genuine collaboration—MAPoRL utilizes Multi-Agent Reinforcement Learning (MARL). The approach consists of three main pillars:  
  • **Multi-Turn Interaction Framework:** Multiple LLMs independently generate responses to a question and then engage in a multi-turn discussion, updating their answers based on the historical context of the debate.  
  • **Verifier-Based Reward System:** A separate "Verifier" model evaluates the agents' responses. An agent's reward is an "Influence-aware Verification Reward," meaning it is calculated based not only on the immediate correctness of their current answer, but on how their response impacts the future answers of the entire group.  
  • **Multi-Agent PPO (Proximal Policy Optimization):** The agents' policies are continuously updated using multi-agent PPO. Each agent optimizes its value function to maximize its cumulative expected reward, forcing them to learn how to strategically engage with the other models over time.  
  **5. Important Aspects & Key Findings**
  • **Co-Training Over Single-Agent Training:** The paper provides game-theoretic analysis and empirical evidence showing that training a single agent is insufficient for collaboration. Agents must adapt their policies *simultaneously* to overcome the temptation of short-term independent rewards and reach a cooperative equilibrium.  
  • **Transferable Collaboration Skills:** A major breakthrough of MAPoRL is "meta-capability" generalization. Models trained to collaborate on one specific task (like mathematical reasoning) can successfully transfer those collaborative skills to completely different, unseen domains (like natural language inference).  
  • **Heterogeneous Synergy:** MAPoRL successfully trains foundation models of varying sizes and architectures (e.g., a 3B parameter model and an 8B parameter model) to work together. The framework proves that diverse model partnerships yield better outcomes than the models operating individually
