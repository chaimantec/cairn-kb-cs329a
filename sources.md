# Sources — CS329A (Stanford, Autumn 2025)

Every document the course website (<https://cs329a.stanford.edu/>) links as course material, with its original URL. The site was fetched on 2026-09-13; it notes that "paper readings may be updated closer to the class date", so a reading list here is the list as of that date.

**The course publishes no slides** on its public site — the instructors upload lecture materials to Canvas — so the course material this knowledge base draws on is the **paper readings** listed per lecture in the site's schedule. **Cite every paper at the original URL below.** The PDFs were downloaded to `raw/pdfs/papers/` on the machine that built this KB, but that directory is **gitignored and not in this repo**: the local filenames are recorded so a future build can find them, and are not links.

**Ingested so far:** the four row-2 readings, for catalog lecture 2 — their full text, transcribed from the arXiv LaTeX source, is linked in the Row 2 table below. Lecture 1 lists no readings.

## Schedule rows and catalog positions

The site's schedule has 20 rows; the Cairn catalog has 9 videos. Files in this KB are named by **catalog position**. Position 2 is confirmed against its transcript, which discusses all four row-2 readings; the rest of the mapping below is by title and tentative. See [INDEX](INDEX.md).

| Site row | Date | Topic | Catalog position | Readings |
|---|---|---|---|---|
| 1 | Mon Sep 22 | Course Overview | 1 | none listed |
| 2 | Fri Sep 26 | Test-time Compute Scaling | 2 (confirmed) | 4 — ingested |
| 3 | Mon Sep 29 | Robust Verification | 3 | 4 |
| 4 | Fri Oct 3 | Learning from feedback with tools/code | 4 | 3 |
| 5 | Mon Oct 6 | Multi-step Reasoning/Planning | 5 | 5 |
| 6 | Fri Oct 10 | Train Time Scaling/Scaling RL | 6 | 3 |
| 7 | Mon Oct 13 | Open-Ended Evolution of Self-Improving Agents | 7 (tentative — shares a video with row 8) | 3 |
| 8 | Fri Oct 17 | Self improvement with Search & Deep Research Agents | 7 (tentative — shares a video with row 7) | 3 |
| 13 | Mon Nov 3 | Agentic Frameworks for Software Engineering | — (no video in catalog) | 3 |
| 14 | Fri Nov 7 | Augmenting Agents with Memory (guest lecturer Junchen Jiang, LMCache, UChicago) | — (no video in catalog) | 3 |
| 17 | Mon Nov 17 | Agentic Evaluations & Long-Horizon Tasks | 8 | 3 |
| 20 | Fri Dec 5 | Future Research Areas | 9 | none listed |

Rows 9, 15, 16, 18 and 19 are guest lectures and rows 10–12 are midterm presentations; none of them lists readings or appears in the catalog.

## Row 2 — Test-time Compute Scaling (Fri Sep 26) — 4 readings, ingested

All four are licensed CC BY 4.0. Their LaTeX sources were fetched from `https://arxiv.org/e-print/<id>` on 2026-09-13 to `raw/pdfs/papers/src/<id>/` (gitignored, not in repo).

| Paper | Original URL | Full text in this KB | Local PDF (gitignored, not in repo) | Fetched |
|---|---|---|---|---|
| Large Language Monkeys: Scaling Inference Compute with Repeated Sampling (Brown et al. 2024) | <https://arxiv.org/abs/2407.21787> | [main body](raw/papers/02-large-language-monkeys.md) · [appendix](raw/papers/02-large-language-monkeys-appendix.md) | `raw/pdfs/papers/2407.21787-large-language-monkeys.pdf` | 2026-09-13 |
| Archon: An Architecture Search Framework for Inference-Time Techniques (Saad-Falcon et al. 2024) | <https://www.arxiv.org/abs/2409.15254> | [main body](raw/papers/02-archon.md) · [appendix](raw/papers/02-archon-appendix.md) | `raw/pdfs/papers/2409.15254-archon.pdf` | 2026-09-13 |
| Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters (Snell et al. 2024) | <https://arxiv.org/abs/2408.03314> | [main body](raw/papers/02-scaling-test-time-compute-optimally.md) · [appendix](raw/papers/02-scaling-test-time-compute-optimally-appendix.md) | `raw/pdfs/papers/2408.03314-scaling-llm-test-time-compute-optimally-can-be-mor.pdf` | 2026-09-13 |
| How Do Large Language Monkeys Get Their Power (Laws)? | <https://arxiv.org/abs/2502.17578> | [main body](raw/papers/02-monkeys-power-laws.md) · [appendix](raw/papers/02-monkeys-power-laws-appendix.md) | `raw/pdfs/papers/2502.17578-how-do-large-language-monkeys-get-their-power-laws.pdf` | 2026-09-13 |

## Row 3 — Robust Verification (Mon Sep 29) — 4 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Shrinking the Generation-Verification Gap with Weak Verifiers | <https://arxiv.org/abs/2506.18203> | `raw/pdfs/papers/2506.18203-shrinking-the-generation-verification-gap-with-wea.pdf` | 2026-09-13 |
| Training Verifiers to Solve Math Word Problems (Cobbe et al. 2021) | <https://arxiv.org/abs/2110.14168> | `raw/pdfs/papers/2110.14168-training-verifiers-to-solve-math-word-problems-cob.pdf` | 2026-09-13 |
| Let's Verify step by step (Lightman et al. 2023) | <https://arxiv.org/abs/2305.20050> | `raw/pdfs/papers/2305.20050-let-s-verify-step-by-step-lightman-et-al-2023.pdf` | 2026-09-13 |
| Math-Shepherd: Verify and Reinforce LLMs Step-by-step without Human Annotations (Wang et al. 2023) | <https://arxiv.org/abs/2312.08935> | `raw/pdfs/papers/2312.08935-math-shepherd.pdf` | 2026-09-13 |

## Row 4 — Learning from feedback with tools/code (Fri Oct 3) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al. 2022) | <https://arxiv.org/abs/2210.03629> | `raw/pdfs/papers/2210.03629-react.pdf` | 2026-09-13 |
| RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning | <https://arxiv.org/abs/2410.02089> | `raw/pdfs/papers/2410.02089-rlef.pdf` | 2026-09-13 |
| Constitutional AI: Harmlessness from AI Feedback | <https://arxiv.org/abs/2212.08073> | `raw/pdfs/papers/2212.08073-constitutional-ai.pdf` | 2026-09-13 |

## Row 5 — Multi-step Reasoning/Planning (Mon Oct 6) — 5 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| SWiRL: Synthetic Data Generation & Multi-Step RL for Reasoning & Tool Use | <https://arxiv.org/abs/2504.04736> | `raw/pdfs/papers/2504.04736-swirl.pdf` | 2026-09-13 |
| Language Agent Tree Search Unifies Reasoning Acting and Planning in Language Models (Zhou et al. 2023) | <https://arxiv.org/abs/2310.04406> | `raw/pdfs/papers/2310.04406-language-agent-tree-search-unifies-reasoning-actin.pdf` | 2026-09-13 |
| SPRINT: Enabling Interleaved Planning and Parallelized Execution in Reasoning Models | <https://arxiv.org/abs/2506.05745> | `raw/pdfs/papers/2506.05745-sprint.pdf` | 2026-09-13 |
| ADaPT: As-Needed Decomposition and Planning with Language Models (Prasad et al. 2024) | <https://arxiv.org/abs/2311.05772> | `raw/pdfs/papers/2311.05772-adapt.pdf` | 2026-09-13 |
| Wider or Deeper? Scaling LLM Inference-Time Compute with Adaptive Branching Tree Search | <https://arxiv.org/abs/2503.04412> | `raw/pdfs/papers/2503.04412-wider-or-deeper-scaling-llm-inference-time-compute.pdf` | 2026-09-13 |

## Row 6 — Train Time Scaling/Scaling RL (Fri Oct 10) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| STaR: Bootstrapping Reasoning With Reasoning (Zelikman et al. 2022) | <https://arxiv.org/pdf/2203.14465> | `raw/pdfs/papers/2203.14465-star.pdf` | 2026-09-13 |
| DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models | <https://arxiv.org/abs/2402.03300> | `raw/pdfs/papers/2402.03300-deepseekmath.pdf` | 2026-09-13 |
| DAPO: An Open-Source LLM Reinforcement Learning System at Scale | <https://arxiv.org/abs/2503.14476> | `raw/pdfs/papers/2503.14476-dapo.pdf` | 2026-09-13 |

## Row 7 — Open-Ended Evolution of Self-Improving Agents (Mon Oct 13) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Automated design of agentic systems | <https://arxiv.org/pdf/2505.22954> | `raw/pdfs/papers/2505.22954-automated-design-of-agentic-systems.pdf` | 2026-09-13 |
| The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery (Lu et al. 2024) | <https://arxiv.org/abs/2408.06292> | `raw/pdfs/papers/2408.06292-the-ai-scientist.pdf` | 2026-09-13 |
| AlphaEvolve: A Gemini-powered coding agent for designing advanced algorithms | <https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/AlphaEvolve.pdf> | `raw/pdfs/papers/alphaevolve-alphaevolve.pdf` | 2026-09-13 |

## Row 8 — Self improvement with Search & Deep Research Agents (Fri Oct 17) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Competition-Level Code Generation with AlphaCode | <https://arxiv.org/pdf/2203.07814> | `raw/pdfs/papers/2203.07814-competition-level-code-generation-with-alphacode.pdf` | 2026-09-13 |
| AlphaCode 2 Technical Report | <https://storage.googleapis.com/deepmind-media/AlphaCode2/AlphaCode2_Tech_Report.pdf> | `raw/pdfs/papers/alphacode2-tech-report-alphacode-2-technical-report.pdf` | 2026-09-13 |
| Search-o1: Agentic Search-Enhanced Large Reasoning Models | <https://arxiv.org/pdf/2501.05366> | `raw/pdfs/papers/2501.05366-search-o1.pdf` | 2026-09-13 |

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

## Row 17 — Agentic Evaluations & Long-Horizon Tasks (Mon Nov 17) — 3 readings

| Paper | Original URL | Local file (gitignored, not in repo) | Fetched |
|---|---|---|---|
| Measuring AI Ability to Complete Long Tasks | <https://arxiv.org/abs/2503.14499> | `raw/pdfs/papers/2503.14499-measuring-ai-ability-to-complete-long-tasks.pdf` | 2026-09-13 |
| GDPVal: Evaluating AI Model Performance on Real-World Economically Valuable Tasks | <https://arxiv.org/abs/2510.04374> | `raw/pdfs/papers/2510.04374-gdpval.pdf` | 2026-09-13 |
| DeepScholar-Bench: A Live Benchmark and Automated Evaluation for Generative Research Synthesis | <https://arxiv.org/abs/2508.20033> | `raw/pdfs/papers/2508.20033-deepscholar-bench.pdf` | 2026-09-13 |
