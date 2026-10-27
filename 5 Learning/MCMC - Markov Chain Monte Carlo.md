---
type: learning
date: 2026-05-25
status: Done
projects: ["Early Exploration (May-June 2026)"]
topics: ["Generative Models and Sampling"]
tags: [type/learning, project/early-exploration, topic/generative-models-and-sampling]
notion: https://app.notion.com/p/357d68da32f080debfe5ef4af3ef81a8
created: 2026-05-05
updated: 2026-05-25
---
# MCMC - Markov Chain Monte Carlo

> [!summary] In short
> Notes on Monte Carlo simulation, Markov chains and MCMC, with formulas.

**Project:** [[Early Exploration (May-June 2026)]]  
**Topics:** [[Generative Models and Sampling]]  
**Notion category:** [[Notion Category - General knowledge]]  
**Original:** [Notion page](https://app.notion.com/p/357d68da32f080debfe5ef4af3ef81a8)

---

## 🎲 Monte Carlo Simulation & 🔗 Markov Chains

---

### 🎲 Monte Carlo Simulation

#### Definition
A computational algorithm that relies on repeated random sampling to obtain numerical results. It models the probability of different outcomes in a process that is difficult to predict due to the intervention of random variables. By running thousands of simulated trials, it provides an approximate distribution of possible outcomes.

#### The Core Concept
The foundational math behind Monte Carlo simulation relies on the **Law of Large Numbers**. It states that you can approximate the expected value of a random variable by taking the empirical average of independent random samples.

#### The Formula (Expected Value Approximation)

$$
E[f(X)] \approx \frac{1}{N} \sum_{i=1}^{N} f(x_i)
$$
**Variables:**
- N = The total number of random samples.
- $x_i$ = Each randomly generated sample drawn from the probability distribution.
- $f(x_i)$ = The function evaluated at that specific random sample.

---

### 🔗 Markov Chain

#### Definition
A stochastic (random) model describing a sequence of possible events where the probability of moving to the next state depends only on the current state, not on the sequence of events that preceded it.

#### The Core Concept
The defining characteristic of a Markov Chain is **"memorylessness"** (known as the **Markov Property**). The system has no memory of how it arrived at its current state; the future relies strictly on the present.

#### The Formulas
**1. The Markov Property**
This mathematical statement defines the memoryless nature of the chain:

$$
P(X_{n+1} = x \mid X_1 = x_1, X_2 = x_2, \dots, X_n = x_n) = P(X_{n+1} = x \mid X_n = x_n)
$$
**Variables:**
- $X_n$ = The state of the system at time step $n$.
- *Note:* The probability of moving to state $X_{n+1}$ depends only on the immediate previous state $X_{n}$.

**2. The Transition Matrix ($P$)**
A Markov chain is typically defined by a transition matrix, where each entry represents the probability of moving from state $i$ to state $j$:

$$
P_{ij} = P(X_{n+1} = j \mid X_n = i)
$$
- *Note:* In a valid transition matrix, the sum of probabilities in each horizontal row must equal $1$ (100%), because the system must move to some state in the next step, even if it just stays in the same one.

═══════════════ ⟡ ═══════════════

## 🔮 Markov Chain Monte Carlo (MCMC)

---
When a probability distribution $\pi(x)$ is too complex to sample from directly, **MCMC builds a Markov Chain whose stationary distribution IS**  $\pi(x)$. Run the chain long enough, and the states it visits become valid samples.

---

### ⚙️ How It Works
1. **Start** at any state $x_n$.
1. **Propose** a new state $x'$ from $q(x' \mid x_n)$.
1. **Compute** the acceptance probability $\alpha$.
1. **Accept** $x'$ with probability $\alpha$, otherwise stay at $x_n$.
1. **Repeat** — the chain converges to $\pi(x)$.

---

### 📐 The Key Formula — Metropolis-Hastings

$$
\alpha(x_n, x') = \min\left(1, \; \frac{\pi(x') \, q(x_n \mid x')}{\pi(x_n) \, q(x' \mid x_n)}\right)
$$
**Variables:**
- $\pi(x)$ = Target distribution we want to sample from.
- $q(x' \mid x_n)$ = Proposal distribution.
- $\alpha$ = Acceptance probability (always between 0 and 1).

**The intuition:**
- If $x'$ is **more probable** than $x_n$  under $\pi$ → always accept.
- If $x'$ is **less probable** → accept with probability proportional to the ratio.
- This ratio ensures the chain spends more time in high-probability regions, naturally reproducing $\pi(x)$.

═══════════════ ⟡ ═══════════════

## complete proof and theory

> [!attachment] File in Notion: **MCMC_proof_lecture.md.pdf** (483.9 KiB) - [open the Notion page](https://app.notion.com/p/357d68da32f080debfe5ef4af3ef81a8)
