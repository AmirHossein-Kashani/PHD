---
type: paper
date: 2026-08-13
status: Done
projects: ["Creativity in Multi-Agent Debate", "Auto Researcher"]
topics: ["Creativity and Exploration", "Benchmarks and Datasets"]
tags: [type/paper, project/creativity-debate, project/auto-researcher, topic/creativity-and-exploration, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3bbd68da32f080a2abaafa51d32bf45b
created: 2026-08-13
updated: 2026-08-14
---
# EscapeBench

> [!summary] In short
> Text-only room-escape games to measure creative intelligence of LLM agents. Top models reach only about 15% progress without hints.

**Project:** [[Creativity in Multi-Agent Debate]], [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Creativity and Exploration]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Creativity Measuring Benchmarks]]  
**Original:** [Notion page](https://app.notion.com/p/3bbd68da32f080a2abaafa51d32bf45b)

---

## EscapeBench — Study Notes

### General Idea
EscapeBench (Qian et al., UIUC, ACL 2025) is a **purely text-based benchmark** for measuring the *creative intelligence* of LLM agents — no vision component involved, so it is directly usable for evaluating an LLM's creativity without any VLM dependency. It is built on room escape game scenarios where an agent must escape by discovering non-obvious tool uses, inferring implicit goals, and reasoning over very long action chains. The core finding: even top models (GPT-4o, Claude-3.5-Sonnet) achieve only ~15% average progress without hints and need 6–7x the optimal number of steps, far behind average humans — exposing a gap in creative reasoning that standard analytical benchmarks never test.

### Details
**Structure & environment.** 36 hand-annotated games across 3 difficulty levels (same solution logic; difficulty only changes description clarity and feedback granularity). The world is a graph of *Scenes* containing *Items* (puzzles) and *Tools* (collectibles), all defined in YAML files hidden from the agent. The agent only sees text: a scene description, interactable items, and tools in its bag.
**Action space (5 verbs).** `move(scene)`, `click(item)`, `apply(tool, item)`, `input(string, item)`, `craft(tool, tool)`. *Apply*, *Input*, and *Craft* are the creativity-heavy actions with the highest hint usage.
**Key point on crafting (our main discussion).** The agent knows **neither** which tools can combine **nor** what the result would be — both are hidden in the game config. Its only path to understanding is: read tool descriptions → hypothesize a connection → try `craft(A, B)` → learn from the engine's success/failure feedback. There is **no tutorial by design**; a recipe list would collapse the task into memorization, which is exactly what the benchmark tries to avoid.
**Combinations are rational, not surreal.** Every recipe follows real-world physical causality — just an unconventional application of it (affordance-based creativity, not magic). Examples: oily rag + rusty key → clean usable key; battery + controller → charged controller; wooden stick repurposed as a pry bar due to its hard texture. Arbitrary results (e.g., "oil + wood = wall-destroying weapon") never occur; a `check_data.py` script and human annotation enforce logical coherence. This grounding is what makes the test fair — a creative agent has a real signal to exploit rather than brute-force luck.
**Metrics.** *Hints Used* (a hint is force-given after 50 stalled actions, revealing the next target location + action) and *Total Steps*, plus Early Exit Progress. Human baseline: 4.33 hints, ~258 steps; oracle chain: ~108 steps; best model (Claude-3.5-Sonnet): ~9 hints, ~690 steps.
**EscapeAgent (their proposed fix).** Two modules on top of a CoT BaseAgent: **Reflection** maintains a task list (new / update-with-failures / delete) to avoid repeating mistakes; **Foresight** fires on each new tool or task, forcing the agent to explicitly hypothesize creative uses ("find the connection between descriptions") before acting. Result: ~50% fewer hints and ~40% fewer steps — but still about 2x worse than humans.
**Practical notes for my own evaluation.**
- Repo: `github.com/qiancheng0/EscapeBench` — supports OpenAI-style APIs and open-source models via vLLM (`deploy_vllm_model.py`).
- Minimum viable model scale is ~7B (smaller models behave near-randomly); over-specialized models (e.g., math-tuned) fail badly.
- Expect low absolute scores; compare models relatively.
- A human-player mode (`scripts/run_human.sh`) exists — worth playing one game first to understand the metrics.
