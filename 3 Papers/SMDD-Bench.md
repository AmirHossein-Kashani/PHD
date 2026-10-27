---
type: paper
date: 2026-08-14
status: Done
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
notion-tags: ["bio"]
tags: [type/paper, project/creativity-debate, project/auto-researcher, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bcd68da32f08017a308cb2c99a64350
created: 2026-08-14
updated: 2026-08-14
---
# SMDD-Bench

> [!summary] In short
> 502 medicinal-chemistry tasks over 102 protein targets (pharmacophore, interaction points, scaffold hopping, ...).

**Project:** [[Creativity in Multi-Agent Debate]], [[Auto Researcher]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Creativity Measuring Benchmarks]]  
**Original:** [Notion page](https://app.notion.com/p/3bcd68da32f08017a308cb2c99a64350)

---

**The SMDD-Bench Tasks**
  The benchmark consists of 502 distinct tasks across 102 unique protein targets. These are broken down into five core task types that mimic the daily workflows of medicinal chemists:  
  1. **2D Pharmacophore Identification:**
      ◦ **The Goal:** Given a list of active (effective) and inactive (ineffective) molecules for a specific target, the agent must derive a 2D map (pharmacophore) of the essential features required for a molecule to be active.  
  2. **Interaction Point Discovery:**
      ◦ **The Goal:** Given the 3D structure of a protein and a specific binding pocket, the agent must predict the three most conserved 3D interaction points across a diverse set of binders. This tests the model's 3D spatial intuition.  
  3. **Scaffold Hopping:**
      ◦ **The Goal:** Given a known active drug, the agent must design a completely new molecule with a fundamentally different core structure (scaffold) while still preserving the original drug's binding interactions. This is commonly used in pharma to discover novel drugs while avoiding existing patents.  
  4. **Lead Optimization:**
      ◦ **The Goal:** The most complex task type. The agent must take a starting molecule and improve it along multiple simultaneous objectives (e.g., lowering binding affinity) while holding up to four other properties strictly constant and adhering to hard constraints.  
  5. **Fragment Assembly:**
      ◦ **The Goal:** The agent is given one or two small chemical fragments along with their 3D poses inside a protein pocket. It must successfully assemble them into a single, cohesive, and drug-like binder. 

---

Here is exactly how the evaluation works:
  1. **3D Structure and Binding Prediction (Boltz2):** The evaluator uses Boltz2 (a biomolecular structure prediction model) to simulate how your newly designed molecule actually folds and fits into the target protein's binding pocket. It also predicts the molecule's binding affinity to ensure it binds strongly.  
  2. **Interaction Matching:** Once the 3D pose is predicted, the evaluator checks the specific geometric interactions between the new molecule and the protein. It verifies that the new molecule successfully preserves the critical interactions (the pharmacophore) that made the original drug effective, despite having a completely different structural scaffold.
