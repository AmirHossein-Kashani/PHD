---
type: topic
aliases: [GFlowNet, MCMC, sampling, generative models]
tags: [type/topic, topic/generative-models-and-sampling]
---
# Generative Models and Sampling

> [!summary] In short
> Background theory from May-June 2026 for molecule and protein generation. **MCMC** samples from a distribution with long random chains. A **GFlowNet** learns a sampler that builds an object step by step, with probability proportional to a reward: it pays a big training cost once, then samples cheaply. Diffusion and VAEs were also on the study list.

## Notes

- [[GFlowNet Foundations]] - long study notes.
- [[MCMC - Markov Chain Monte Carlo]] - Monte Carlo, Markov chains, MCMC formulas.
- [[Molecule Generation for Drug Design - Graph Learning View]] - reading item.
- Idea from 11 May: predict the **whole sequence** with some input noise to be more creative ([[Early Exploration (May-June 2026)#2026-05-11 Weekly Meeting (Summer 1)|2026-05-11 Weekly Meeting (Summer 1)]]).

## Notes from meetings

> [!info] Dated notes from meetings about this subject (newest first). Meetings are not separate notes; each entry links to the Notion page and to the other parts of the same meeting.

### 2026-05-19 One-on-One (Report)

*Original: [Notion page](https://app.notion.com/p/365d68da32f080cf9859df80c1b1e583)*  
*Other parts of this meeting:* [[Early Exploration (May-June 2026)#2026-05-19 One-on-One (Report)|Early Exploration (May-June 2026)]], [[Skills and Tools#2026-05-19 One-on-One (Report)|Skills and Tools]]

#### next week plan
- [x] Diffusion
    - [ ] [https://www.youtube.com/watch?v=EhndHhIvWWw](https://www.youtube.com/watch?v=EhndHhIvWWw)
    - [ ] Return to theoritical aspect of VAE and MCMC
        - [x] [[MCMC - Markov Chain Monte Carlo]] 
            - Proof of how the final distribution is the same as the

## Notes on this topic

### Papers (2)

- [[GFlowNet Foundations]] - Long study notes on GFlowNets: a trained sampler that builds objects step by step with probability proportional to a reward. Compared with MCMC.
- [[Molecule Generation for Drug Design - Graph Learning View]] - Reading item (no notes yet): molecule generation for drug design from a graph learning view.

### Learning (1)

- [[MCMC - Markov Chain Monte Carlo]] - Notes on Monte Carlo simulation, Markov chains and MCMC, with formulas.

