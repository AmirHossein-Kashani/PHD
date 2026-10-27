---
type: learning
date: 2026-08-26
status: Not started
topics: ["Skills and Tools"]
tags: [type/learning, topic/skills-and-tools]
notion: https://app.notion.com/p/3c8d68da32f0807c9e63cca5e043e802
created: 2026-08-26
updated: 2026-08-28
---
# What to Learn for AI and LLM Research

> [!summary] In short
> Checklist of topics to become a strong AI/LLM researcher (math, ML, deep learning, LLMs, research skills).

**Topics:** [[Skills and Tools]]  
**Notion category:** [[Notion Category - Documentation]]  
**Original:** [Notion page](https://app.notion.com/p/3c8d68da32f0807c9e63cca5e043e802)

---

If your goal is to become a **strong computer science researcher specializing in AI and LLMs**, focus on these areas:

#### 1. Core foundations
- [ ] Become comfortable with **linear algebra**
- [ ] Probability and statistics
- [ ] Optimization
- [ ] Algorithms and data structures
- [ ] Information theory basics
- [ ] Numerical computing
- [ ] Understand complexity and scalability

#### 2. Machine learning fundamentals
- [ ] Supervised / unsupervised learning
- [ ] Bias–variance and generalization
- [ ] Loss functions and regularization
- [ ] Gradient descent and optimization
- [ ] Evaluation metrics
- [ ] Experimental design
- [ ] Statistical significance and confidence intervals

#### 3. Deep learning
- [ ] Neural network fundamentals
- [x] Backpropagation
- [x] CNNs
- [x] RNNs/LSTMs at least conceptually
- [ ] Attention
- [ ] Transformers
- [ ] Representation learning
- [ ] Embeddings
- [ ] Distributed training basics

#### 4. Understand LLMs deeply
- [ ] Tokenization
- [ ] Transformer architecture
- [ ] Positional encoding
- [ ] Pretraining objectives
- [ ] Instruction tuning
- [ ] Supervised fine-tuning
- [ ] RLHF / preference optimization
- [ ] DPO and related methods
- [ ] In-context learning
- [ ] Chain-of-thought and reasoning methods
- [ ] Tool use / function calling
- [ ] RAG
- [ ] Agents and multi-agent systems
- [ ] Long-context methods
- [ ] Memory systems
- [ ] Model evaluation
- [ ] Hallucination and reliability
- [ ] Safety and alignment

#### 5. Become technically strong
- [ ] Python
- [ ] PyTorch
- [ ] Hugging Face
- [ ] Git
- [ ] Linux
- [ ] Docker
- [ ] CUDA basics
- [ ] GPU debugging/profiling
- [ ] Distributed training with multiple GPUs
- [ ] SLURM or another HPC system
- [ ] Experiment tracking such as W&B
- [ ] Learn to work efficiently on remote GPU servers

You do **not** need to become a CUDA expert, but you should understand what happens between your model and the GPU.

#### 6. Learn to read papers efficiently
For every important paper, answer:
- [ ] What problem are they solving?
- [ ] Why wasn't the previous solution sufficient?
- [ ] What is their main idea?
- [ ] What exactly is novel?
- [ ] What assumptions do they make?
- [ ] What baselines are used?
- [ ] Are the experiments convincing?
- [ ] What ablations are missing?
- [ ] What would make this method fail?
- [ ] What research question naturally follows?

Don't just **read papers**. Criticize them.

#### 7. Reproduce research
- [ ] Pick influential papers
- [ ] Run their official implementation
- [ ] Reproduce at least one important result
- [ ] Change one component
- [ ] Understand why the result changes
- [ ] Compare your implementation with theirs

Reproduction is one of the fastest ways to become a better scientist.

#### 8. Learn how to identify research gaps
Constantly ask:
> **What can current AI systems still not do well?**
Look for:
- [ ] Failure cases
- [ ] Unexplained behaviors
- [ ] Expensive components
- [ ] Brittle assumptions
- [ ] Poor scalability
- [ ] Weak evaluations
- [ ] Contradictory papers
- [ ] Missing benchmarks
- [ ] Tasks humans perform easily but models struggle with

This skill is more important for a researcher than knowing hundreds of architectures.

#### 9. Develop hypotheses, not just ideas
Weak:
> "Maybe we can add memory to an agent."
Better:
> "Persistent episodic memory should improve performance on long-horizon tasks requiring information from previous interactions."
Better still:
> "Retrieving task-specific episodic memories only when uncertainty exceeds a threshold should improve long-horizon performance while reducing unnecessary context consumption."
Train yourself to turn ideas into **testable hypotheses**.

#### 10. Become excellent at experiments
- [ ] Define the research question before running experiments
- [ ] Select strong baselines
- [ ] Control variables
- [ ] Run multiple seeds when appropriate
- [ ] Report variance
- [ ] Perform ablations
- [ ] Analyze failures
- [ ] Check statistical significance where appropriate
- [ ] Avoid cherry-picking
- [ ] Keep experimental logs

A good AI scientist is not someone who gets one impressive number. They understand **why the number changed**.

#### 11. Master LLM evaluation
Study evaluation of:
- [ ] Reasoning
- [ ] Coding
- [ ] Factuality
- [ ] Creativity
- [ ] Diversity
- [ ] Agents
- [ ] Long-context behavior
- [ ] Tool use
- [ ] Robustness
- [ ] Calibration
- [ ] Human preferences

Also understand:
- [ ] Benchmark contamination
- [ ] LLM-as-a-judge
- [ ] Pairwise evaluation
- [ ] Human evaluation
- [ ] Inter-rater agreement
- [ ] Evaluation bias

Evaluation itself is becoming an important AI research field.

#### 12. Build things
Do not become a researcher who only reads papers.
Build:
- [ ] A transformer from scratch
- [ ] A simple language model
- [ ] Fine-tuning pipeline
- [ ] RAG system
- [ ] LLM agent
- [ ] Tool-using agent
- [ ] Multi-agent system
- [ ] LLM evaluation framework
- [ ] Your own research prototype

You learn a huge amount when something **actually fails**.

#### 13. Follow important research
Regularly monitor:
- [ ] arXiv
- [ ] ICLR
- [ ] ICML
- [ ] NeurIPS
- [ ] ACL
- [ ] EMNLP
- [ ] NAACL
- [ ] COLM
- [ ] AAAI
- [ ] AAMAS for agent research

And research from organizations such as:
- [ ] OpenAI
- [ ] Anthropic
- [ ] Google DeepMind
- [ ] Meta AI
- [ ] Microsoft Research
- [ ] NVIDIA
- [ ] Allen Institute for AI
- [ ] Stanford
- [ ] Berkeley
- [ ] MIT
- [ ] CMU

But don't try to read everything.

#### 14. Maintain a research notebook
For every interesting idea, record:
**Problem → Evidence → Gap → Hypothesis → Experiment → Result → Next question**
Over time, this becomes one of your most valuable research assets.

#### 15. Learn scientific writing
- [ ] Write clear research questions
- [ ] Write concise abstracts
- [ ] Explain contributions precisely
- [ ] Separate evidence from speculation
- [ ] Write strong related work
- [ ] Create readable figures
- [ ] Report limitations
- [ ] Avoid exaggerated claims

You should be able to explain your research at three levels:
**30 seconds → 5 minutes → 30 minutes.**

#### 16. Learn how to review papers
Practice reviewing papers as if you were a conference reviewer:
**Summary**
**Strengths**
**Weaknesses**
**Questions**
**Technical correctness**
**Novelty**
**Experimental quality**
**Overall recommendation**
Eventually volunteer/review for workshops and conferences when qualified.

#### 17. Develop a research identity
Avoid randomly moving between every new AI trend.
Eventually you want people to associate you with **1–3 research questions**.
For example:
> **LLM reasoning + agents + memory**
or
> **Creativity + diversity + reasoning in LLMs**
or
> **AI for scientific discovery + agentic systems**
Then individual papers become pieces of a larger research program.

#### 18. Collaborate
- [ ] Talk with researchers outside your immediate group
- [ ] Attend seminars
- [ ] Attend conferences/workshops
- [ ] Discuss unfinished ideas
- [ ] Collaborate with complementary researchers
- [ ] Maintain relationships with researchers you meet
- [ ] Ask good technical questions

Research is significantly accelerated by good collaborators.

#### 19. Learn to communicate your work publicly
- [ ] Maintain Google Scholar
- [ ] Maintain a professional website
- [ ] Maintain GitHub
- [ ] Publish reproducible code
- [ ] Write understandable project pages
- [ ] Give research talks
- [ ] Create good posters
- [ ] Explain your work without jargon

#### 20. Keep asking the scientist's questions
Whenever you see an interesting AI result, don't stop at:
> **"That's cool."**
Ask:
> **Why does it work?**
> **When does it stop working?**
> **What variable actually caused the improvement?**
> **Can I falsify the authors' explanation?**
> **What is the simplest experiment that can test this?**
> **Does this generalize?**
> **What happens at 10× the scale?**
> **What important question did the paper not answer?**
That mindset is ultimately what separates **using AI** from **doing AI science**.
