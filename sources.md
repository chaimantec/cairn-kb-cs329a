# Sources — CS329A (Stanford, Autumn 2025)

Every document the course website (<https://cs329a.stanford.edu/>) links as course material, with its original URL. The site was fetched on 2026-09-13; it notes that "paper readings may be updated closer to the class date", so a reading list here is the list as of that date.

**The course publishes no slides** on its public site — the instructors upload lecture materials to Canvas — so the course material this knowledge base draws on is the **paper readings** listed per lecture in the site's schedule. **Cite every paper at the original URL below.** The PDFs were downloaded to `raw/pdfs/papers/` on the machine that built this KB, but that directory is **gitignored and not in this repo**: the local filenames are recorded so a future build can find them, and are not links.

**Ingested so far:** the four row-2 readings (catalog lecture 2); Weaver, the one CC BY 4.0 reading of row 3 (lecture 3); row 4's three readings (lecture 4); LATS and SWiRL, the two CC BY 4.0 readings of row 5 (lecture 5); AlphaCode, from row 8 (lecture 7); and row 17's three readings (lecture 8), in the versions current on the lecture date — their full text, transcribed from the arXiv LaTeX source, is linked in the tables below. The other readings of rows 3, 5 and 8, and all three readings of row 6 (lecture 6), are linked only, for licence reasons. **Lectures 1 and 9 list no readings**, and both are built from their transcripts alone.

## Schedule rows and catalog positions

The site's schedule has 20 rows; the Cairn catalog has 9 videos. Files in this KB are named by **catalog position**. Positions 2–8 are confirmed against their transcripts: positions 2, 3, 4 and 6 each discuss every reading of their row, position 5 discusses three of its row's five, position 7 discusses all three readings of row 8, and position 8 all three of row 17. Position 9 is row 20, confirmed by its transcript — it is the quarter's last lecture, it opens by recapping the whole course and says it will "cover some future research areas" (≈3:57). See [INDEX](INDEX.md).

| Site row | Date | Topic | Catalog position | Readings |
|---|---|---|---|---|
| 1 | Mon Sep 22 | Course Overview | 1 | none listed |
| 2 | Fri Sep 26 | Test-time Compute Scaling | 2 (confirmed) | 4 — ingested |
| 3 | Mon Sep 29 | Robust Verification | 3 (confirmed) | 4 — Weaver ingested; 3 linked only (licence) |
| 4 | Fri Oct 3 | Learning from feedback with tools/code | 4 (confirmed) | 3 — ingested |
| 5 | Mon Oct 6 | Multi-step Reasoning/Planning | 5 (confirmed) | 5 — LATS and SWiRL ingested; 3 linked only (licence) |
| 6 | Fri Oct 10 | Train Time Scaling/Scaling RL | 6 (confirmed) | 3 — linked only (licence) |
| 7 | Mon Oct 13 | Open-Ended Evolution of Self-Improving Agents | — (no video in catalog) | 3 |
| 8 | Fri Oct 17 | Self improvement with Search & Deep Research Agents | 7 (confirmed) | 3 — AlphaCode ingested; 2 linked only (licence) |
| 13 | Mon Nov 3 | Agentic Frameworks for Software Engineering | — (no video in catalog) | 3 |
| 14 | Fri Nov 7 | Augmenting Agents with Memory (guest lecturer Junchen Jiang, LMCache, UChicago) | — (no video in catalog) | 3 |
| 17 | Mon Nov 17 | Agentic Evaluations & Long-Horizon Tasks | 8 (confirmed) | 3 — ingested (METR v2, DeepScholar-Bench v1) |
| 20 | Fri Dec 5 | Future Research Areas | 9 (confirmed) | none listed |

Rows 9, 15, 16, 18 and 19 are guest lectures and rows 10–12 are midterm presentations; none of them lists readings or appears in the catalog.

Position 7 is row 8, confirmed by its transcript: it discusses AlphaCode, AlphaCode 2 and Search-o1 in order, and none of row 7's readings. Lecture 6 (row 6, Fri Oct 10) had previewed AlphaCode for "next Friday", row 8's date (lecture 6 transcript, ≈12:29). Row 7 has no video in the catalog; lecture 7 refers to "a scientist style of work that folks covered last lecture" (≈45:31–46:18), which fits row 7's *The AI Scientist*.

## Row 2 — Test-time Compute Scaling (Fri Sep 26) — 4 readings, ingested

All four are licensed CC BY 4.0. Their LaTeX sources were fetched from `https://arxiv.org/e-print/<id>` on 2026-09-13 to `raw/pdfs/papers/src/<id>/` (gitignored, not in repo).

| Paper | Original URL | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|
| Large Language Monkeys: Scaling Inference Compute with Repeated Sampling (Brown et al. 2024) | <https://arxiv.org/abs/2407.21787> | [main body](raw/papers/02-large-language-monkeys.md) · [appendix](raw/papers/02-large-language-monkeys-appendix.md) | `raw/pdfs/papers/2407.21787-large-language-monkeys.pdf` | 2026-09-13 |
| Archon: An Architecture Search Framework for Inference-Time Techniques (Saad-Falcon et al. 2024) | <https://www.arxiv.org/abs/2409.15254> | [main body](raw/papers/02-archon.md) (appendices not transcribed) | `raw/pdfs/papers/2409.15254-archon.pdf` | 2026-09-13 |
| Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters (Snell et al. 2024) | <https://arxiv.org/abs/2408.03314> | [main body](raw/papers/02-scaling-test-time-compute-optimally.md) (appendices not transcribed) | `raw/pdfs/papers/2408.03314-scaling-llm-test-time-compute-optimally-can-be-mor.pdf` | 2026-09-13 |
| How Do Large Language Monkeys Get Their Power (Laws)? | <https://arxiv.org/abs/2502.17578> | [main body](raw/papers/02-monkeys-power-laws.md) (appendices not transcribed) | `raw/pdfs/papers/2502.17578-how-do-large-language-monkeys-get-their-power-laws.pdf` | 2026-09-13 |

## Row 3 — Robust Verification (Mon Sep 29) — 4 readings, lecture built

Catalog position 3, confirmed: its transcript discusses all four readings. Licences were checked on each arXiv abstract page on 2026-09-13. **Only Weaver is CC BY 4.0**, and only its text and figures are in this KB (LaTeX source fetched from `https://arxiv.org/e-print/2506.18203` to `raw/pdfs/papers/src/2506.18203/`, gitignored). The other three carry arXiv's non-exclusive licence, which does not permit republishing them: they are linked, discussed and cited in [the lecture page](wiki/03-robust-verification.md), and not transcribed.

| Paper | Original URL | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|
| Shrinking the Generation-Verification Gap with Weak Verifiers | <https://arxiv.org/abs/2506.18203> | CC BY 4.0 | [main body](raw/papers/03-weaver.md) (appendices not transcribed) | `raw/pdfs/papers/2506.18203-shrinking-the-generation-verification-gap-with-wea.pdf` | 2026-09-13 |
| Training Verifiers to Solve Math Word Problems (Cobbe et al. 2021) | <https://arxiv.org/abs/2110.14168> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2110.14168-training-verifiers-to-solve-math-word-problems-cob.pdf` | 2026-09-13 |
| Let's Verify step by step (Lightman et al. 2023) | <https://arxiv.org/abs/2305.20050> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2305.20050-let-s-verify-step-by-step-lightman-et-al-2023.pdf` | 2026-09-13 |
| Math-Shepherd: Verify and Reinforce LLMs Step-by-step without Human Annotations (Wang et al. 2023) | <https://arxiv.org/abs/2312.08935> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2312.08935-math-shepherd.pdf` | 2026-09-13 |

## Row 4 — Learning from feedback with tools/code (Fri Oct 3) — 3 readings, lecture built

Catalog position 4, confirmed: its transcript discusses all three readings, in order. Licences were checked on each arXiv abstract page on 2026-09-14: **all three are CC BY 4.0**. Their LaTeX sources were fetched from `https://arxiv.org/e-print/<id>` on 2026-09-14 to `raw/pdfs/papers/src/<id>/` (gitignored, not in repo); each main body is transcribed in this KB, and the appendices are not.

| Paper | Original URL | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|
| ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al. 2022) | <https://arxiv.org/abs/2210.03629> | CC BY 4.0 | [main body](raw/papers/04-react.md) (appendices not transcribed) | `raw/pdfs/papers/2210.03629-react.pdf` | 2026-09-13 |
| RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning | <https://arxiv.org/abs/2410.02089> | CC BY 4.0 | [main body](raw/papers/04-rlef.md) (appendices not transcribed) | `raw/pdfs/papers/2410.02089-rlef.pdf` | 2026-09-13 |
| Constitutional AI: Harmlessness from AI Feedback | <https://arxiv.org/abs/2212.08073> | CC BY 4.0 | [main body](raw/papers/04-constitutional-ai.md) (appendices not transcribed) | `raw/pdfs/papers/2212.08073-constitutional-ai.pdf` | 2026-09-13 |

## Row 5 — Multi-step Reasoning/Planning (Mon Oct 6) — 5 readings, lecture built

Catalog position 5, confirmed: its transcript discusses LATS, SPRINT and SWiRL, in that order, and does not discuss ADaPT or *Wider or Deeper?*. Licences were checked on each arXiv abstract page on 2026-09-14. **LATS and SWiRL are CC BY 4.0**, and their main bodies are transcribed in this KB (the appendices are not). **SPRINT is CC BY-NC-SA 4.0**; it is linked and discussed in [the lecture page](wiki/05-planning-and-multi-step-reasoning.md), not transcribed, and none of its figures is committed. ADaPT and *Wider or Deeper?* carry arXiv's non-exclusive licence and are linked only. LaTeX sources for all five were fetched from `https://arxiv.org/e-print/<id>` on 2026-09-14 to `raw/pdfs/papers/src/<id>/` (gitignored, not in repo).

| Paper | Original URL | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|
| SWiRL: Synthetic Data Generation & Multi-Step RL for Reasoning & Tool Use | <https://arxiv.org/abs/2504.04736> | CC BY 4.0 | [main body](raw/papers/05-swirl.md) (appendices not transcribed) | `raw/pdfs/papers/2504.04736-swirl.pdf` | 2026-09-13 |
| Language Agent Tree Search Unifies Reasoning Acting and Planning in Language Models (Zhou et al. 2023) | <https://arxiv.org/abs/2310.04406> | CC BY 4.0 | [main body](raw/papers/05-lats.md) (appendices not transcribed) | `raw/pdfs/papers/2310.04406-language-agent-tree-search-unifies-reasoning-actin.pdf` | 2026-09-13 |
| SPRINT: Enabling Interleaved Planning and Parallelized Execution in Reasoning Models | <https://arxiv.org/abs/2506.05745> | CC BY-NC-SA 4.0 | not transcribed (licence) | `raw/pdfs/papers/2506.05745-sprint.pdf` | 2026-09-13 |
| ADaPT: As-Needed Decomposition and Planning with Language Models (Prasad et al. 2024) | <https://arxiv.org/abs/2311.05772> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2311.05772-adapt.pdf` | 2026-09-13 |
| Wider or Deeper? Scaling LLM Inference-Time Compute with Adaptive Branching Tree Search | <https://arxiv.org/abs/2503.04412> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2503.04412-wider-or-deeper-scaling-llm-inference-time-compute.pdf` | 2026-09-13 |

## Row 6 — Train Time Scaling/Scaling RL (Fri Oct 10) — 3 readings, lecture built

Catalog position 6, confirmed: its transcript discusses all three readings, in order. Licences were checked on each arXiv abstract page on 2026-09-15: **all three carry arXiv's non-exclusive licence**, which does not permit republishing them. They are linked, discussed and cited by section, figure, table and equation in [the lecture page](wiki/06-train-time-scaling-scaling-rl.md); none is transcribed and none of their figures is committed. LaTeX sources were fetched from `https://arxiv.org/e-print/<id>` on 2026-09-15 to `raw/pdfs/papers/src/<id>/` (gitignored, not in repo) and read for the wiki.

| Paper | Original URL | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|
| STaR: Bootstrapping Reasoning With Reasoning (Zelikman et al. 2022) | <https://arxiv.org/pdf/2203.14465> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2203.14465-star.pdf` | 2026-09-13 |
| DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models | <https://arxiv.org/abs/2402.03300> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2402.03300-deepseekmath.pdf` | 2026-09-13 |
| DAPO: An Open-Source LLM Reinforcement Learning System at Scale | <https://arxiv.org/abs/2503.14476> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2503.14476-dapo.pdf` | 2026-09-13 |

## Row 7 — Open-Ended Evolution of Self-Improving Agents (Mon Oct 13) — 3 readings, no video

No recording of this lecture is in the Cairn catalog, so none of its readings is ingested. Catalog position 7 is row 8, below. Lecture 7 refers to "a scientist style of work that folks covered last lecture" (≈45:31–46:18), which fits this row's *The AI Scientist*.

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Automated design of agentic systems | <https://arxiv.org/pdf/2505.22954> | `raw/pdfs/papers/2505.22954-automated-design-of-agentic-systems.pdf` | 2026-09-13 |
| The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery (Lu et al. 2024) | <https://arxiv.org/abs/2408.06292> | `raw/pdfs/papers/2408.06292-the-ai-scientist.pdf` | 2026-09-13 |
| AlphaEvolve: A Gemini-powered coding agent for designing advanced algorithms | <https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/AlphaEvolve.pdf> | `raw/pdfs/papers/alphaevolve-alphaevolve.pdf` | 2026-09-13 |

## Row 8 — Self improvement with Search & Deep Research Agents (Fri Oct 17) — 3 readings, lecture built

Catalog position 7, confirmed: its transcript discusses all three readings, in order. Licences were checked on 2026-09-15. **AlphaCode**'s arXiv abstract page lists CC BY 4.0, although the published PDF prints "© 2022 DeepMind. All rights reserved". The KB relies on the arXiv licence grant (a user decision) and transcribes its main body; the appendices are not transcribed. Its LaTeX source was fetched from `https://arxiv.org/e-print/2203.07814` on 2026-09-15 to `raw/pdfs/papers/src/2203.07814/` (gitignored, not in repo). The **AlphaCode 2 Technical Report** is a PDF on Google DeepMind's storage that prints "© 2023 Google DeepMind. All rights reserved", and **Search-o1** carries arXiv's non-exclusive licence. Both are linked, discussed and cited in [the lecture page](wiki/07-self-improvement-and-deep-research-agents.md), not transcribed, and none of their figures is committed. Search-o1's LaTeX source was fetched to `raw/pdfs/papers/src/2501.05366/` (gitignored) and read for the wiki.

| Paper | Original URL | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|
| Competition-Level Code Generation with AlphaCode | <https://arxiv.org/pdf/2203.07814> | CC BY 4.0 on arXiv; the PDF prints all rights reserved | [main body](raw/papers/07-alphacode.md) (appendices not transcribed) | `raw/pdfs/papers/2203.07814-competition-level-code-generation-with-alphacode.pdf` | 2026-09-13 |
| AlphaCode 2 Technical Report | <https://storage.googleapis.com/deepmind-media/AlphaCode2/AlphaCode2_Tech_Report.pdf> | all rights reserved (PDF notice) | not transcribed (licence) | `raw/pdfs/papers/alphacode2-tech-report-alphacode-2-technical-report.pdf` | 2026-09-13 |
| Search-o1: Agentic Search-Enhanced Large Reasoning Models | <https://arxiv.org/pdf/2501.05366> | arXiv non-exclusive | not transcribed (licence) | `raw/pdfs/papers/2501.05366-search-o1.pdf` | 2026-09-13 |

## Row 13 — Agentic Frameworks for Software Engineering (Mon Nov 3) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| CodeMonkeys: Scaling Test-Time Compute for Software Engineering | <https://arxiv.org/abs/2501.14723> | `raw/pdfs/papers/2501.14723-codemonkeys.pdf` | 2026-09-13 |
| KernelBench: Can LLMs Write Efficient GPU Kernels? | <https://arxiv.org/pdf/2502.10517> | `raw/pdfs/papers/2502.10517-kernelbench.pdf` | 2026-09-13 |
| Improving Parallel Program Performance with LLM Optimizers via Agent-System Interfaces | <https://arxiv.org/abs/2410.15625> | `raw/pdfs/papers/2410.15625-improving-parallel-program-performance-with-llm-op.pdf` | 2026-09-13 |

## Row 14 — Augmenting Agents with Memory (guest lecturer Junchen Jiang, LMCache, UChicago) (Fri Nov 7) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Cartridges: Lightweight and general-purpose long context representations via self-study | <https://arxiv.org/abs/2506.06266> | `raw/pdfs/papers/2506.06266-cartridges.pdf` | 2026-09-13 |
| MemGPT: Towards LLMs as Operating Systems (Packer et al, 2023) | <https://arxiv.org/abs/2310.08560> | `raw/pdfs/papers/2310.08560-memgpt.pdf` | 2026-09-13 |
| CacheBlend: Fast Large Language Model Serving for RAG with Cached Knowledge Fusion | <http://arxiv.org/abs/2405.16444> | `raw/pdfs/papers/2405.16444-cacheblend.pdf` | 2026-09-13 |

## Row 17 — Agentic Evaluations & Long-Horizon Tasks (Mon Nov 17) — 3 readings, lecture built

Catalog position 8, confirmed: its transcript names all three readings at the start and discusses them in order. Licences were checked on each arXiv abstract page on 2026-09-15: **all three are CC BY 4.0**, and their main bodies are transcribed in this KB (the appendices are not).

**Two of the three were revised after the lecture** (Mon Nov 17, 2025), and the revisions change numbers and names the lecture quotes. By user decision, this KB transcribes the version that was current on the lecture date, which is also the one the lecture quotes. For the METR paper that is **v2** (30 March 2025); its v3 and v4 (2026) retitle it *Measuring AI Ability to Complete Long Software Tasks* and change its headline results. For DeepScholar-Bench it is **v1** (27 August 2025); its v2 (February 2026) renames DeepScholar-base and reports new results. GDPval has only v1. The versioned LaTeX sources were fetched from `https://arxiv.org/e-print/<id>v<n>` on 2026-09-15 to `raw/pdfs/papers/src/<id>v<n>/`, and GDPval's to `raw/pdfs/papers/src/2510.04374/` (gitignored, not in repo). The abstract URLs below serve the latest version. The site lists GDPval as "GDPVal"; the paper prints "GDPval".

| Paper | Original URL | Version transcribed | Licence | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|---|---|
| Measuring AI Ability to Complete Long Tasks (Kwa et al. 2025) | <https://arxiv.org/abs/2503.14499> | v2, <https://arxiv.org/abs/2503.14499v2> | CC BY 4.0 | [main body](raw/papers/08-metr-long-tasks.md) (appendices not transcribed) | `raw/pdfs/papers/2503.14499v2-measuring-ai-ability-to-complete-long-tasks.pdf` (the 2026-09-13 download, `2503.14499-…`, is v4) | 2026-09-15 |
| GDPVal: Evaluating AI Model Performance on Real-World Economically Valuable Tasks (Patwardhan et al. 2025) | <https://arxiv.org/abs/2510.04374> | v1 (the only version) | CC BY 4.0 | [main body](raw/papers/08-gdpval.md) (appendices not transcribed) | `raw/pdfs/papers/2510.04374-gdpval.pdf` | 2026-09-13 |
| DeepScholar-Bench: A Live Benchmark and Automated Evaluation for Generative Research Synthesis (Patel et al. 2025) | <https://arxiv.org/abs/2508.20033> | v1, <https://arxiv.org/abs/2508.20033v1> | CC BY 4.0 | [main body](raw/papers/08-deepscholar-bench.md) (appendices not transcribed) | `raw/pdfs/papers/2508.20033v1-deepscholar-bench.pdf` (the 2026-09-13 download, `2508.20033-…`, is v2) | 2026-09-15 |

## Row 20 — Future Research Areas (Fri Dec 5) — no readings listed

Catalog position 9, confirmed. The site's "Paper Readings" column is empty for this row, so the lecture is **transcript-only** and this KB ingests nothing for it: [lecture 9](wiki/09-future-research-areas.md) is cited to the transcript by timestamp throughout, as [lecture 1](wiki/01-course-overview.md) is.

The lecture itself presents key ideas from **three papers that the site does not list** — as row 20's list, or under any other row: a multi-agent fine-tuning paper the lecture describes only as "coming from multi-agent finetuning" (≈6:17); **DeepSeekMath-V2**, the one paper it names (≈14:53), a different work from the *DeepSeekMath* paper the site lists under row 6; and an unnamed paper on a model that proposes and solves its own tasks (≈22:39), which a student calls "the Absolute Zero paper" (≈1:03:34). None is transcribed here and none is linked — the lecture gives no URL for any of them, and this KB does not supply one from outside the course.
