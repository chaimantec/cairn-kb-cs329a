# KB build — CS329A (Self-Improving AI Agents, Stanford, Autumn 2025)

The Cairn catalog lists **9 recorded lectures** ("Part 1" … "Part 9"). The course publishes
**no slides** (they are on Canvas only), so the course material this KB ingests is the
**paper readings** the course website lists per lecture — exactly that list, nothing else. Paper
PDFs are downloaded to `raw/pdfs/` on disk and are **gitignored**; every paper is cited at its
original URL as the course website links it.

- **Run 1** (complete): lecture 1 — Course Overview. The site lists **no readings** for it, so
  it is transcript-only. Also: site crawl, download of every listed reading to disk, `sources.md`.

## Catalog position ↔ site schedule row

The catalog's 9 videos do not map 1:1 onto the site's 20-row schedule (which includes midterm
presentations and guest lectures that are not in the playlist). Tentative, by title — confirm each
against its transcript before ingesting that lecture's readings:

| catalog | video title | site row | readings listed |
|---|---|---|---|
| 1 | Course Overview | 1 Course Overview | none |
| 2 | Test-Time Compute Scaling | 2 Test-time Compute Scaling | 4 |
| 3 | Robust Verification | 3 Robust Verification | 4 |
| 4 | Learning from Feedback with Tools/Code | 4 Learning from feedback with tools/code | 3 |
| 5 | Planning and Multi-Step Reasoning | 5 Multi-step Reasoning/Planning | 5 |
| 6 | Train Time Scaling/Scaling RL | 6 Train Time Scaling/Scaling RL | 3 |
| 7 | Self-Improvement and Deep Research Agents | 7 Open-Ended Evolution… and/or 8 Self improvement with Search & Deep Research Agents | 3 + 3 — **unresolved** |
| 8 | Agentic Evaluations and Long Horizon Tasks | 17 Agentic Evaluations & Long-Horizon Tasks | 3 |
| 9 | Future Research Areas | 20 Future Research Areas | none |

Site rows 13 (Agentic Frameworks for Software Engineering) and 14 (Augmenting Agents with Memory)
list readings but have no video in the catalog.

## Transcripts
- [x] 01 Part 1 | Course Overview — video 6YnLB0XbTnI (verbatim → raw/transcripts/original/)
- [x] 01 edited transcript (Sonnet copy-edit; parent checks timestamps, numbers, per-paragraph ratio)
- [x] 02 Part 2 | Test-Time Compute Scaling — video -Ggc37xLj_Y (verbatim → raw/transcripts/original/)
- [x] 02 edited transcript (Sonnet copy-edit, last paragraph by parent after a session-limit kill; 81/81 timestamps, numbers match except deliberate GPT-4.0→GPT-4o and pass@1, ratio outliers 26:08/47:23 are [Ed] notes)
- [ ] 03 Part 3 | Robust Verification — video p7TdPUcPoik
- [ ] 04 Part 4 | Learning from Feedback with Tools/Code — video Lxh9RF5S-K0
- [ ] 05 Part 5 | Planning and Multi-Step Reasoning — video Ml_fp9XkB8Y
- [ ] 06 Part 6 | Train Time Scaling/Scaling RL — video yVnmHSAy3ck
- [ ] 07 Part 7 | Self-Improvement and Deep Research Agents — video Uni9dqyuuDM
- [ ] 08 Part 8 | Agentic Evaluations and Long Horizon Tasks — video 8JAqLnTaZu4
- [ ] 09 Part 9 | Future Research Areas — video AyO6wyu4DEg

## Crawl
- [x] Fetch course site index: https://cs329a.stanford.edu/ (excluding pastprojects.html — student work)
- [x] Download every listed paper reading to raw/pdfs/papers/ (on disk, gitignored)
- [x] Write sources.md (original URLs, grouped by site schedule row)

## Papers (course material — per lecture, only the site's own list)

Budget rules from the user (2026-09-13): **one subagent at a time**; for papers after Large Language Monkeys, **main bodies only** (appendices stay at arXiv).

- [x] 01 — none listed; nothing to ingest
- [x] 02 — mapping confirmed: the transcript covers all four row-2 readings (Monkeys ≈0:52, power laws ≈7:10, Snell et al. ≈26:55, Archon ≈45:03)
- [x] 02 — arXiv LaTeX sources fetched to raw/pdfs/papers/src/ (gitignored); all four CC BY 4.0
- [x] 02 — raw/papers/02-large-language-monkeys.md + -appendix.md (full text; check_paper_file.py clean)
- [x] 02 — raw/papers/02-monkeys-power-laws.md (main body only, by user decision to save budget; checker clean --main-only)
- [x] 02 — raw/papers/02-scaling-test-time-compute-optimally.md (main body only; checker clean; Fig 9 re-cropped and its description rewritten after a parent check found wrong star values)
- [x] 02 — raw/papers/02-archon.md (main body only; written before a session-limit kill, checker clean except the spec-mandated italic \multirow note, a known checker artifact)
- [x] 02 — figures: KB-written descriptions REMOVED by user decision (show the chart; say only what the caption and paper text say). Before removal an audit had found errors in 4 of 20. All 31 crops verified by a text-layer check (no text cut by the edge, caption complete, no body text below).
- [ ] 03 — 4 readings
- [ ] 04 — 3 readings
- [ ] 05 — 5 readings
- [ ] 06 — 3 readings
- [ ] 07 — resolve site row 7 vs 8 first
- [ ] 08 — 3 readings (site row 17)
- [x] 09 — none listed; nothing to ingest

## Wiki
- [x] wiki/02-test-time-compute-scaling.md
- [x] Topic pages updated for lecture 2 (test-time-scaling, verifiers, scaling-laws; no new pages)
- [x] INDEX.md — lecture 2, raw/papers, raw/images
- [x] wiki/01-course-overview.md
- [x] Topic pages (cross-lecture concepts lecture 1 establishes): test-time-scaling, verifiers, chain-of-thought, reasoning-models, llm-training-pipeline, scaling-laws, agentic-workflows, self-improvement, course-logistics
- [x] INDEX.md table of contents
- [x] AGENTS.md — CS329A conventions (no slides, papers as material, numbering table)

## Images (paper figures, cropped from the PDFs — user opted in for lecture 2)
- [x] raw/images/02-test-time-compute-scaling/ — figure crops for every figure in a transcribed part: Monkeys 1–10, power laws 1–7, Snell 1–9, Archon 1–5 (31). Appendix-only crops deleted when appendices were descoped.
- [x] Wire figures into wiki/02 (14 embedded where the lecture discusses them; paper files carry all 31)
- [x] AGENTS.md — Images and raw/papers conventions

## Publish
- [x] Lecture 2: kb.json updated, verify_kb.py clean, committed and pushed (kbUrl already set)
- [x] kb.json — coverage, materials.method, provenance caveats
- [x] SEE_ALSO.md, if a sibling KB is genuinely relevant
- [x] verify_kb.py clean, and its review section read
- [x] Commit and push
- [x] PATCH kbUrl onto the catalog entry

## Resume notes — lecture 2 (written 2026-09-13, before the wiki step)

**Lecture 2 COMPLETE (2026-09-13):** wiki page, topic pages, INDEX, kb.json written; verify_kb.py clean; pushed. The notes below are kept for reference.

**Done and committed locally (not pushed):** both transcripts; paper files for all four row-2 readings
(Monkeys main + appendix; power laws, Snell, Archon main body only); `AGENTS.md` paper/image
conventions; `sources.md` row 2. Images (31 crops) committed; no KB-written figure descriptions (removed). Checker: `check_paper_file.py` passes on all four; the one known
FAIL on archon is its italic `\multirow` note (a spec-mandated editorial line, not invented text).

**Budget rules (user):** one subagent at a time; no appendices for the three later papers.

**Still to do, in order**
1. ~~Figure audit~~ superseded — descriptions removed (see Papers section); ~~Monkeys footnote 4~~ fixed; images committed.
3. Write `wiki/02-test-time-compute-scaling.md` (Opus, not delegated), embedding the
   figures the lecture discusses — describe a figure only through its caption, the paper's text and
   what the lecturer says; never read values off a chart — with relative links `../raw/images/02-test-time-compute-scaling/…`.
4. Topic pages: update test-time-scaling (dedicated lecture now), verifiers (ORM vs PRM, generation–
   verification gap), scaling-laws (inference scaling laws); consider new pages for reward models
   (ORM/PRM) and inference-time architectures (Archon) if the material stands alone.
5. INDEX.md (coverage 2 of 9, lecture 2 entry, raw/papers section), kb.json (coverage, paperReadings,
   materials.method, images: lecturesWithImages 1, files 31, byLecture {"2": 31}, images note: no KB-written figure descriptions exist (removed), crops text-layer verified; figuresAudited false with a note saying why,
   caveats: main-body-only papers, "itest" unclear), verify_kb.py + read review, push.

**Lecture 2 outline (edited transcript timestamps)**
- 0:05 three stages of LLM development; inference scaling changes no parameters.
- 0:52–3:13 Large Language Monkeys: repeated sampling + verifier; Llama 3 8B/70B beat GPT-4o single-attempt.
- 3:13–4:49 SWE-bench: "DeepSeek-V3" (speaker's "I believe") beats Claude 3.5 / o1-preview at 1,000
  samples — this chart is NOT in the Monkeys paper (which uses DeepSeek-Coder-V2, 250 samples); do not
  attribute it to a reading.
- 4:49–7:10 inference scaling laws: coverage vs k follows an exponentiated power law, 70M–70B models.
- 7:10–11:08 why: per-problem pass@k is exponential in k; aggregate power law needs a long tail of hard
  problems (power-laws paper).
- 11:08–11:53 economics: inference spend, offline agents.
- 11:53–15:10 verifiable domains: formal proofs, unit tests, "AI as a compiler" (PyTorch→CUDA),
  KernelBench (site row 13 reading — link forward, do not ingest), language translation.
- 15:10–19:04 no verifier: majority vote and reward models plateau; generation–verification gap; rare
  correct samples (Monkeys Fig 7, Fig 8).
- 19:52–26:55 discussion 1: verifier quality; revisions; RAG; self-study; weak/cheap-to-refute verifiers,
  simulations; 10,000-sample data on Hugging Face; ensembling verifiers → Weaver (row 3 reading, next
  lecture); manual check of math coverage [Ed: unclear %]; unit-test coverage as a failure mode.
- 26:55–34:00 Snell et al.: parallel sampling vs sequential revisions; ORM vs PRM (per step, not per
  token); best-of-N with verifier; PRM beam search (budget 4, keep top 2); PRMs fine-tuned, in-domain.
- 34:45–37:57 setup (MATH 12k train / 500 test, PaLM), 5 difficulty bins from pass@1, revision model;
  majority/ORM/PRM/compute-optimal curves; sequential-to-parallel ratio by bin.
- 37:57–41:53 test-time vs pretraining compute (Snell Fig 9): easy/medium favour test-time, hardest
  favour pretraining; Q&A on pretraining-once vs per-query inference and the ratio R.
- 42:41–45:03 discussion 2: tree search mixes both; easy → sequential, hard → parallel exploration.
- 45:03–47:23 Archon (the course TA is a co-author): inference-time architecture design; inputs
  (benchmarks, inference call budget, LLMs, techniques); the search optimizer ("itest" [Ed: unclear]).
- 48:11–50:38 components: generator, fuser, critic, ranker, verifier; 54:30–56:04 unit test generator /
  evaluator (balanced brackets example).
- 50:38–54:30 fusion/ranking win-rate chart (random, ranker, oracle, fusion, rank-then-fuse; 1–10
  samples from one model vs ensembles of 1–10 models) — this chart is in Archon's APPENDIX, not the
  transcribed main body: cite the transcript and arXiv, no image.
- 56:04–58:24 an optimized architecture; offline pruning of the search space; accuracy vs calls.
- 58:24–59:12 more layers help (Archon Fig 4). 59:12–1:00:43 Bayesian optimization with construction
  rules. 1:00:43–1:03:05 results: open-source Archon matches/exceeds closed models; task-specific vs
  general-purpose; "14.1%" average pass@1 gain as spoken — compare with the paper's number and state both.
