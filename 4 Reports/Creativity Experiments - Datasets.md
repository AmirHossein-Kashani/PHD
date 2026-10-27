---
type: report
date: 2026-08-25
status: Not started
projects: ["Creativity in Multi-Agent Debate"]
topics: ["Benchmarks and Datasets", "Creativity and Exploration"]
tags: [type/report, project/creativity-debate, topic/benchmarks-and-datasets, topic/creativity-and-exploration]
notion: https://app.notion.com/p/3c7d68da32f080699c2bff5efb452dc0
created: 2026-08-25
updated: 2026-08-25
---
# Creativity Experiments - Datasets

> [!summary] In short
> The 4 small datasets used in the creativity experiments: med_ddx (real MedQA), med_repurpose (synthetic), gen_aut (alternative uses), gen_problems (synthetic city problems).

**Project:** [[Creativity in Multi-Agent Debate]]  
**Topics:** [[Benchmarks and Datasets]], [[Creativity and Exploration]]  
**From meeting:** [[Creativity in Multi-Agent Debate#2026-08-13 One-on-One (13 and 20 Aug)|2026-08-13 One-on-One (13 and 20 Aug)]]  
**Original:** [Notion page](https://app.notion.com/p/3c7d68da32f080699c2bff5efb452dc0)

---

## Our datasets
We use **4 datasets**. Two are about medicine. Two are about general life problems. Each dataset has **2 items**. So we have **8 items** in total.
All three of our experiments use the **same 8 items**. Nothing changes between experiments. This makes it easy to compare results.
Each item gives the AI some **facts** (small pieces of information) and one **task** (what to do with those facts).

---

### 1. `med_ddx` — guess the diagnosis
**What it asks:** Read a patient's symptoms and test results. List possible diagnoses. Include normal ideas, but also unusual ones that still make sense.
**Example:** A 2-year-old boy has pale skin, low blood platelets, and damaged red blood cells. The correct answer is *Hemolytic uremic syndrome*.
**Where it comes from:** Real exam questions. We took them from **MedQA**, a public dataset built from real USMLE medical exam questions. This is real research data.
**Right answer?** Yes. Each item has one correct diagnosis. We check if the AI's list includes it, and how high it ranks.

---

### 2. `med_repurpose` — find a new use for a drug
**What it asks:** Read a patient's condition and the biology behind it. Suggest a drug that could help, even if the drug is normally used for something else.
**Example:** A woman has a rare lung disease. Normal treatments failed. The AI must suggest a different drug that might work, based on how the disease works in the body.
**Where it comes from:** Nobody. This patient and this disease information were **made up** by an AI just for this study. We call this "synthetic" data.
**Right answer?** No. There is no single correct drug. The AI's ideas are judged only on how creative and how believable they are.

---

### 3. `gen_aut` — creative uses for an object
**What it asks:** Given a simple object, list creative ways to use it.
**Example:** You live on a farm with no money and no electricity. You have 200 bricks. List creative uses for a brick.
**Where it comes from:** The task itself is a **classic psychology test** called the Alternative Uses Task. Scientists have used it for decades to study creativity. But the specific story (the farm, the rules) was written just for this study.
**Right answer?** No. There is no correct list. Ideas are judged on how many there are, how different they are, and how good they are.

---

### 4. `gen_problems` — solve a real-world problem
**What it asks:** Read about a city or organization with a real problem. Suggest different solutions.
**Example:** A city has too much food waste. You know the budget, the rules, and who can help. Suggest ways to fix the problem.
**Where it comes from:** Nobody. We wrote these problems ourselves for this study. They are not from a real city.
**Right answer?** No. Ideas are judged only on creativity and usefulness.

---

### Summary table

| Dataset | Topic | From real research? | Has a correct answer? |
|---|---|---|---|
| `med_ddx` | Medical diagnosis | **Yes** — real exam questions | Yes |
| `gen_aut` | Object uses | **Yes**, | No |
| `med_repurpose` | Drug ideas | No — made up | No |
| `gen_problems` | City/org problems | No — made up | No |

---

### Why this matters
Only `med_ddx` gives us a real correct answer to check against. This is why it is the most trustworthy dataset for checking safety (did the AI keep the right answer, or throw it away?).
The other three datasets only tell us if the AI's ideas are creative and useful. They cannot tell us if the AI is "right" or "wrong," because there is no single right answer to compare with.
