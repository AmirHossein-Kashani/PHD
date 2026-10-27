---
type: paper
date: 2026-06-08
status: Done
projects: ["Early Exploration (May-June 2026)"]
topics: ["Generative Models and Sampling"]
notion-tags: ["protein", "Generative"]
tags: [type/paper, project/early-exploration, topic/generative-models-and-sampling]
notion: https://app.notion.com/p/357d68da32f080468775f849b43fe85f
created: 2026-05-05
updated: 2026-06-09
---
# GFlowNet Foundations

> [!summary] In short
> Long study notes on GFlowNets: a trained sampler that builds objects step by step with probability proportional to a reward. Compared with MCMC.

**Project:** [[Early Exploration (May-June 2026)]]  
**Topics:** [[Generative Models and Sampling]]  
**Notion category:** [[Notion Category - Protein Generation]]  
**Original:** [Notion page](https://app.notion.com/p/357d68da32f080468775f849b43fe85f)

---

> [!attachment] File in Notion: **Gflow.pdf** (1.2 MiB) - [open the Notion page](https://app.notion.com/p/357d68da32f080468775f849b43fe85f)

## GFlowNet Foundations — Study Notes
*Bengio, Lahlou, Deleu, Hu, Tiwari, Bengio — JMLR 24 (2023). Builds on Bengio et al. (2021).*

---

### 1. The one-sentence idea
A **Generative Flow Network (GFlowNet)** is a trained sampler that builds a compositional
  object `s` (a set, a graph, a sequence) through a series of small stochastic steps, so that
  the probability of producing object `s` is **proportional to a given reward** `R(s)`:
PT(s)∝R(s),E(s)=−log⁡R(s).P_T(s) \propto R(s), \qquad E(s) = -\log R(s).
PT(s)∝R(s),E(s)=−logR(s).
It converts an energy/reward function into a one-shot generative policy. This is the same
  goal MCMC pursues, but MCMC pays the cost per-sample (long chains, mode-mixing problems),
  whereas a GFlowNet pays a large **up-front training cost** and then samples cheaply in a
  single forward pass. The catch: GFlowNets only win when the reward's modes have learnable
  structure to generalize over.
**Why "in proportion to" matters:** unlike reward-maximizing RL (which seeks the single best
  mode), proportional sampling yields a *diverse* set of high-reward candidates — valuable for
  active learning, drug/molecule discovery, etc.

---

### 2. Graph and flow setup (Section 2)
- The state space is a **pointed DAG**: single source `s0`, single sink `sf`. Every object
  is built along a **complete trajectory** from `s0` to `sf`.
- A **terminating state** `s` is a parent of `sf`; the edge `s → sf` is a **terminating edge**,
  and its flow is where the reward is read off.

#### What F (flow) actually is
`F` is overloaded — keep the three meanings separate:

| Notation | Meaning |
|---|---|
| `F(τ)` | base object: a non-negative weight ("# of particles") on a complete trajectory `τ` |
| `F(s)` | **state flow** = sum of `F(τ)` over all `τ` passing through `s` |
| `F(s→s')` | **edge flow** = sum of `F(τ)` over all `τ` crossing that edge |

Particle analogy: inject a fixed amount of "liquid" at `s0`, split it at each state according
  to the policy; the amount through each state/edge is its flow.

#### Key flow facts
- **Conservation (flow matching):** in-flow = out-flow at every state
  F(s′)=s∈Par(s′)∑F(s→s′)=s′′∈Child(s′)∑F(s′→s′′).
    F(s′)=∑s∈Par(s′)F(s→s′)=∑s′′∈Child(s′)F(s′→s′′).F(s') = \sum_{s\in Par(s')} F(s\to s') = \sum_{s''\in Child(s')} F(s'\to s'').
- **Total flow = partition function:** `Z = F(s0) = F(sf)`.
- **Terminating flow = reward (the training target):** `F(s → sf) = R(s)`.
- **Sampling distribution:** `P_T(s) = F(s → sf) / Z` — a proper distribution over terminating states.

#### Markovian flows
A flow is **Markovian** if a "particle" has no memory — its next step depends only on the
  current state. Then the trajectory probability factorizes:
P(τ)=∏tPF(st∣st−1).P(\tau) = \prod_t P_F(s_t \mid s_{t-1}).
P(τ)=t∏PF(st∣st−1).
This is the crucial simplification: instead of specifying `F` for *every* trajectory
  (exponentially many), you only specify **edge flows** (far fewer). Every flow has a unique
  equivalent Markovian flow (Prop. 23), so restricting to Markovian flows loses nothing we care about.
**Forward / backward transition probabilities:**
PF(s′∣s)=F(s→s′)F(s),PB(s∣s′)=F(s→s′)F(s′).P_F(s'\mid s) = \frac{F(s\to s')}{F(s)}, \qquad P_B(s\mid s') = \frac{F(s\to s')}{F(s')}.
PF(s′∣s)=F(s)F(s→s′),PB(s∣s′)=F(s′)F(s→s′).

---

### 3. Training a GFlowNet (Section 3)
We're given a target reward `R` and want to learn an estimator `F̂` (or `P̂_F`, `Ẑ`, etc.)
  whose terminating flows match `R`. We design a **loss `L` that is zero iff the configuration**
  **is a valid Markovian flow matching `R`**, then minimize it by SGD with a neural net.

#### The loss functions
All are squared-log-ratio mismatches. They differ in what they decompose over:
- *(a) Flow Matching (FM) loss** — *state-decomposable* (the original 2021 loss).
  Net outputs edge flows. Penalizes in-flow vs out-flow mismatch at each state:

LFM(s′)=(log⁡δ+∑s∈Par(s′)F^(s→s′)δ+R(s′)+∑s′′∈Child(s′)∖{sf}F^(s′→s′′))2.L_{FM}(s') = \left(\log \frac{\delta + \sum_{s\in Par(s')} \hat F(s\to s')}{\delta + R(s') + \sum_{s''\in Child(s')\setminus\{s_f\}} \hat F(s'\to s'')}\right)^2.
LFM(s′)=(logδ+R(s′)+∑s′′∈Child(s′)∖{sf}F^(s′→s′′)δ+∑s∈Par(s′)F^(s→s′))2.
Drawback: needs explicit sums over parents and children → awkward for large fan-out or continuous spaces.
- *(b) Detailed Balance (DB) loss** — *edge-decomposable* (new in this paper).
  Net outputs `F̂(s)`, `P̂_F`, `P̂_B`. Purely local to a single edge, **no sums over neighbors**:

LDB(s→s′)=(log⁡δ+F^(s)P^F(s′∣s)δ+F^(s′)P^B(s∣s′))2 (s′≠sf),(log⁡δ+F^(s)P^F(sf∣s)δ+R(s))2 (s′=sf).L_{DB}(s\to s') = \left(\log \frac{\delta + \hat F(s)\hat P_F(s'\mid s)}{\delta + \hat F(s')\hat P_B(s\mid s')}\right)^2 \ (s'\neq s_f), \quad \left(\log \frac{\delta + \hat F(s)\hat P_F(s_f\mid s)}{\delta + R(s)}\right)^2 \ (s'=s_f).
LDB(s→s′)=(logδ+F^(s′)P^B(s∣s′)δ+F^(s)P^F(s′∣s))2 (s′=sf),(logδ+R(s)δ+F^(s)P^F(sf∣s))2 (s′=sf).
Named for the analogy to the MCMC detailed-balance condition.
- *(c) Trajectory Balance (TB) loss** — *trajectory-decomposable* (Malkin et al. 2022).
  Net outputs `Ẑ`, `P̂_F`, `P̂_B`. Operates over a whole trajectory:

LTB(τ)=(log⁡Z^∏tP^F(st∣st−1)R(sn)∏tP^B(st−1∣st))2.L_{TB}(\tau) = \left(\log \frac{\hat Z \prod_t \hat P_F(s_t\mid s_{t-1})}{R(s_n)\prod_t \hat P_B(s_{t-1}\mid s_t)}\right)^2.
LTB(τ)=(logR(sn)∏tP^B(st−1∣st)Z^∏tP^F(st∣st−1))2.

#### How training runs
- Roll out a trajectory from `s0` using the current policy, hit a terminating state, query `R(s)`,
  plug transitions into the loss, backprop.
- **Off-policy / offline friendly:** trajectories can come from *any* full-support training
  distribution `π_T`, not the current policy. This is a defining property vs. maximum-likelihood
  generative models, which are sensitive to data distribution.
- Loss/gradient is estimated stochastically on sampled edges/states/trajectories (the full
  sum is intractable).

---

### 4. Backward probability P_B — how to get it
The reward fixes only the **terminating** flows. It does **not** determine the rest of the flow,
  because many construction paths can reach the same object (see Fig. 4: equivalent flows).
  **`P_B` on non-terminal edges is the free choice that picks among them** — it encodes your
  *preference over the order in which the object is built*.
Three ways to obtain it:
1. **Fix by hand** — e.g. uniform over parents `P_B = 1/|Par(s')|`, or bias toward shorter paths
  (can use the time-stamp augmentation of Sec 3.3.1).
1. **Induce from edge flows** — in the FM parametrization it falls out automatically:
  `P_B(s|s') = F̂(s→s') / Σ_{s''∈Par(s')} F̂(s''→s')`.
1. **Learn it** — give it its own network head, trained jointly via DB or TB.

**Constraints / caveats:**
- Must stay a valid distribution: `Σ_{s∈Par(s')} P_B(s|s') = 1`.
- Tied to `P_F` and `F` by the detailed-balance condition `F̂(s)P̂_F(s'|s) = F̂(s')P̂_B(s|s')`;
  given any two, the third is determined (up to normalization).
- **Free choice only holds in deterministic environments.** In stochastic ones the forward
  transition belongs to the environment, so `P_B` becomes constrained (Appendix C, Counterexample 50).

---

### 5. Conditional flows & free energies (Section 4)
**Big contribution:** condition every component on some variable `x`, giving a *family* of flow
  networks. Conditioning unlocks marginalization (intractable sums) over object construction.
- **Free energy** (note: script `𝓕`, NOT the flow `F`):
  e−F(s):=s′≥s∑e−E(s′). The (log of an) intractable marginal sum over all descendants of `s`.
    e−F(s):=∑s′≥se−E(s′).e^{-\mathcal{F}(s)} := \sum_{s' \ge s} e^{-E(s')}.
- **State-conditional flow network** `F_s`: re-anchors the DAG at state `s`. Its initial flow
  marginalizes the downstream terminating flows: `F_s(s0|s) = e^{-𝓕(s)}`. (Plain `F(s)` does
  **not** marginalize correctly — Fig. 5 shows why.)
- **Reward-conditional flow network**: same DAG, different terminating flows `R(s|x)` per `x`.
  Lets one model an energy-based model `R(s|θ) = exp(−E_θ(s))`.

#### Applications of conditioning
- **Energy-based models (4.5):** jointly train an energy function `E_θ` and a GFlowNet sampler
  (GFlowNet provides negative samples for the MLE gradient; can share parameters since
  `E(s) = −log F̂(s→sf)`). Generalizes to conditional distributions over `(x, h)`.
- **Active learning (4.6):** GFlowNet proposes diverse high-reward candidates `x`; a cheap proxy
  `f̂` is queried first, true (expensive) reward only on selected ones; can add epistemic-uncertainty
  acquisition functions (UCB/EI). Jain et al. 2022.
- *Entropy / conditional entropy / mutual information (4.7):** train a *second* GFlowNet on the **entropic reward** `R'(s) = −R(s) log R(s)`; then
  H[S]=F(s0)F′(s0)+logF(s0). Do it with conditional flows → conditional entropy → mutual information.
    H[S]=F′(s0)F(s0)+log⁡F(s0).H[S] = \frac{F'(s_0)}{F(s_0)} + \log F(s_0).

---

### 6. Sets, graphs, marginalization (Section 5)
- **Set GFlowNet:** states are subsets of a universe `U`, each action adds one element; all subsets
  connect to `sf`. Enables: sampling sets ∝ reward, estimating `Z`, conditional probabilities of
  **supersets**, and the **marginal over all supersets** `P_T(S(s)) = e^{−𝓕(s)}/Z`.
- **Graph GFlowNet:** a graph is a set of nodes + edges; actions insert nodes/edges (with constraints
  e.g. acyclicity, connectivity). Used by Deleu et al. (2022) for Bayesian-network structure learning
  (posterior over DAGs).
- **Marginalizing over missing variables:** model a joint over `X = (X1,...,Xn)` as a set of `(i, xᵢ)`
  pairs. Lets you marginalize unspecified variables, sample conditionals, estimate partition functions
  and entropies.
- **Modular energy / factor graphs (5.4):** decompose total energy into reusable factor terms
  `E(g) = Σᵢ E_{Fᵢ}(vⁱ)`; decompose the GFlowNet itself into modules per mechanism. Related to attention
  (type matching = query/key).

---

### 7. Continuous / hybrid actions (Section 6)
- Replace sums with integrals. Split a hybrid state into discrete + continuous parts and factor the
  transition: `P_F(s_{t+1}|s_t) = P(continuous | discrete, s_t) · P(discrete | s_t)`.
- The challenge is representing continuous output densities you can both **evaluate and sample**:
  Gaussians (tractable normalizer but limited), mixtures via "cluster IDs", normalizing flows, or
  diffusion-style resampling.
- "GFlowNets in GFlowNets": use a smaller ⟨GFlowNet, energy⟩ pair to represent a continuous edge flow.
- Extended rigorously later by Lahlou et al. (2023), continuous GFlowNets.

---

### 8. Relationship to other methods (Section 7)
- **vs generative models (VAE/GAN):** those train on positive samples; a GFlowNet trains on a *reward
  function* (positive, negative, and in-between). Difference blurs once you learn the energy too.
- **vs MaxEnt / regularized RL:** they are NOT the same. GFlowNet gives `P_T(s) ∝ R(s)`; MaxEnt RL gives
  `P_T(s) ∝ n(s)R(s)` where `n(s)` = number of paths to `s`. They coincide only when the DAG (minus `sf`)
  is a **tree**. The `n(s)` factor over-weights states reachable by many paths (longer sequences),
  breaking proportional sampling — a key motivation for GFlowNets.
- **vs MCMC:** both target `P_T ∝ R`. MCMC suffers exponential **mode-mixing** cost between separated
  modes; GFlowNet amortizes this into training and can "jump" between modes if structure is learnable.
  Trade-off: MCMC cheaper for a single sample, GFlowNet cheaper for many.
- **vs Sequential Monte Carlo:** SMC needs intermediate targets and per-step likelihoods; GFlowNet reward
  comes only at the end and trajectories needn't be valid intermediate samples.

---

### 9. Appendices — extensions worth knowing
- **A — Direct credit assignment:** flow-matching/DB propagate credit slowly (bootstrapping, like TD).
  Derives a policy-gradient-like estimator (`G1`, `G2`, convex combos) that is *asymptotically unbiased*
  (when flows are matched and trajectories are on-policy) for faster credit to early transitions.
- **B — Conditional GFlowNets formalized:** product configuration space over conditions; condition-decomposable losses.
- **C — Deterministic vs stochastic environments:** splits transitions into policy (even→odd) +
  environment (odd→even). In deterministic envs you can identify policy with `P_F` and choose `P_B` freely;
  in stochastic envs `P_B` is constrained and you may not be able to match target rewards exactly.
- **D — Expected downstream reward & reward-maximizing policy:** define a value `V_{P_π}(s)`; satisfies a
  policy-improvement theorem; a greedy policy recovers reward maximization. Estimate `V_{P_T}` by training a
  second flow with reward `R²`: `V_{P_T}(s) = F'(s|s)/F(s|s)`.
- **E — Intermediate rewards & returns:** the basic GFlowNet reward is **terminal only** (bandit-like).
  To handle dense/intermediate rewards, use a **return-augmented state** that carries the accumulated
  reward `ν(s)=Σ r(s_t)`; set terminal reward = accumulated return, and the GFlowNet samples ∝ return.
  Must be trained **offline** (return known only after termination). The value recursion
  `ρ̄(s) = R(s) + Σ P_F(s'|s) ρ̄(s')` is the no-discount Bellman equation — but note it uses *forward*
  transitions to propagate values, whereas the flow recursion uses *backward* transitions to propagate flows.
- **F — Multi-flows / distributional / unsupervised / Pareto GFlowNets:** learn a *family* of flows over one
  graph (like distributional RL). **Outcome-conditioned GFlowNet** conditions on an outcome `y = f(s)` with
  reward `R_y(s) = 1[f(s)=y]`. Powerful result (Prop. 62): after unsupervised training you can define a reward
  `R = r∘f` *a posteriori* and recover the right flow without retraining: `F_{r∘f}(A) = Σ_y r(y) F(A|y)`.
  **Pareto GFlowNets** condition on convex weights `ω` over multiple objectives to sample the Pareto frontier.

---

### 10. Quick reference — what each loss needs

| Loss | Decomposes over | Net outputs | Needs neighbor sums? | Needs P_B? |
|---|---|---|---|---|
| Flow Matching | states | edge flows `F̂(s→s')` | **Yes** (parents & children) | induced from flows |
| Detailed Balance | edges | `F̂(s)`, `P̂_F`, `P̂_B` | No | yes (fixed or learned) |
| Trajectory Balance | trajectories | `Ẑ`, `P̂_F`, `P̂_B` | No | yes (fixed or learned) |

**Mental model:** flow in = flow out (conservation) + terminating flow = reward + free choice of `P_B`
  (deterministic case) → minimize a squared-log-ratio loss by SGD → sampler that draws objects ∝ reward.
