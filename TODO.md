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
- [x] 03 Part 3 | Robust Verification — video p7TdPUcPoik (verbatim → raw/transcripts/original/, 94 paragraphs, [0:05]–[1:12:45])
- [x] 03 edited transcript (Sonnet copy-edit; 94/94 timestamps, ratios 0.93–1.01, numbers match except pass@1's added "1"s; parent turned the agent's "hacking" restoration at 35:08 back into an [Ed: unclear])
- [x] 04 Part 4 | Learning from Feedback with Tools/Code — video Lxh9RF5S-K0 (verbatim → raw/transcripts/original/, 92 paragraphs, [0:05]–[1:10:52])
- [ ] 04 edited transcript (Sonnet copy-edit; parent checks timestamps, numbers, per-paragraph ratio)
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
- [x] 02 — raw/papers/02-archon.md (main body only; written before a session-limit kill, checker clean except the spec-mandated italic `\multirow` note, a known checker artifact)
- [x] 02 — figures: KB-written descriptions REMOVED by user decision (show the chart; say only what the caption and paper text say). Before removal an audit had found errors in 4 of 20. All 31 crops verified by a text-layer check (no text cut by the edge, caption complete, no body text below).
- [x] 03 — mapping confirmed: the transcript covers all four row-3 readings in order (Cobbe et al. ≈0:52, Lightman et al. ≈21:06, Math-Shepherd ≈37:27, Weaver ≈51:33)
- [x] 03 — licences checked on the arXiv abstract pages (2026-09-13): only Weaver (2506.18203) is CC BY 4.0. Cobbe et al. (2110.14168), Lightman et al. (2305.20050) and Math-Shepherd (2312.08935) are arXiv non-exclusive licence → linked and discussed, NOT transcribed, no figures committed (AGENTS.md rule)
- [x] 03 — Weaver LaTeX source fetched to raw/pdfs/papers/src/2506.18203/ (gitignored)
- [x] 03 — Weaver figure crops 1–6 (main body) → raw/images/03-robust-verification/weaver-figure-N; text-layer check clean on all six
- [x] 03 — raw/papers/03-weaver.md (main body only). check_paper_file.py --main-only: all structural, figure, table, macro and wording checks pass except known artifacts, confirmed in the source — 5 "missing" numbers are `\colXwidth` column-width macros and a `MATH\\500` header split; 3 "unmatched" paragraphs are the two spec-mandated italic table-flattening notes and Figure 3's caption (`\weaver{}` dropped on the LaTeX side)
- [x] 03 — non-CC papers (Cobbe, Lightman, Math-Shepherd): offered keep / drop quotes / lecture-only; user said "continue", so kept as is — summarised and cited by section, figure and table with numbers and a few short quotes, never transcribed, no images
- [x] 04 — mapping confirmed: the transcript covers all three row-4 readings in order (ReAct ≈0:50, RLEF ≈27:18, Constitutional AI ≈46:02)
- [x] 04 — licences checked on the arXiv abstract pages (2026-09-14): all three CC BY 4.0 (ReAct 2210.03629, RLEF 2410.02089, Constitutional AI 2212.08073)
- [x] 04 — LaTeX sources fetched to raw/pdfs/papers/src/<id>/ (gitignored)
- [x] 04 — figure crops, main bodies only → raw/images/04-learning-from-feedback-with-tools-code/: react-figure-1–3, rlef-figure-1–4, constitutional-ai-figure-1–10 (17, 2.3MB); text-layer check clean on all. Constitutional AI's captions use `labelsep=quad` (no colon), so they were cropped with a copy of extract_paper_figures.py whose caption regex also accepts "Figure N" + capitalised text
- [ ] 04 — raw/papers/04-react.md (main body only; one Sonnet agent, after the transcript agent)
- [ ] 04 — raw/papers/04-rlef.md (main body only; one Sonnet agent)
- [ ] 04 — raw/papers/04-constitutional-ai.md (main body only; one Sonnet agent)
- [ ] 05 — 5 readings
- [ ] 06 — 3 readings
- [ ] 07 — resolve site row 7 vs 8 first
- [ ] 08 — 3 readings (site row 17)
- [x] 09 — none listed; nothing to ingest

## Wiki
- [x] wiki/03-robust-verification.md (Opus; all four readings, Weaver figures 1–6 embedded, lecture-vs-paper discrepancies stated)
- [x] Topic pages updated for lecture 3 (verifiers: training a verifier, ORM/PRM evidence, step labels without humans, weak-verifier ensembles; test-time-scaling: selection sets the ceiling; self-improvement: model-labelled reward; no new pages)
- [x] INDEX.md — lecture 3, coverage 3 of 9, verifiers one-liner, raw/papers and raw/images
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
- [x] raw/images/03-robust-verification/ — Weaver Figures 1–6 (main body); embedded in raw/papers/03-weaver.md and all six in wiki/03. No images of the three non-CC readings.
- [x] AGENTS.md — images now lectures 2 and 3; lecture 3 licence findings; positions 2 and 3 confirmed

## Publish
- [x] Lecture 2: kb.json updated, verify_kb.py clean, committed and pushed (kbUrl already set)
- [x] KaTeX pass (2026-09-14, user request for lectures 1–2): every formula in wiki/ and raw/papers/ rendered through the extension's own marked 18 + KaTeX 0.18.1 pipeline — 542 formulas, 0 errors, all tokenized as written. Fixed two multi-`\tag` `aligned` blocks in raw/papers/02-monkeys-power-laws.md (Eqs 5–6, 8–9; split one block per tag); lecture 1–2 prose pass@k / best-of-N / N× converted to inline math; AGENTS.md and PAPER_FILE_SPEC.md now state the KaTeX rules
- [x] GitHub math pass (2026-09-14, user: formulas must also work on github.com): github.com runs markdown over math before MathJax, so 175 of 545 formulas were unrecognised or altered there (backslash-punctuation escapes, `*`/`_` italics pairing, double-escaped `<`, `$` after a hyphen or quote, math inside italics, `%`). Rewrote the math to forms both renderers accept — rules now in AGENTS.md and PAPER_FILE_SPEC.md — and checked every formula against GitHub's markdown API with MathJax parsing: 536 of 537 matched at first; the one left, power-laws Eq. 10, was a `_` after punctuation pairing into italics inside a `$$` block (which double-escaped its `&`s), so the underscore rule now covers display math too (32 more underscores spaced). Final: every formula renders through GitHub's markdown API and MathJax, confirmed on the published github.com file pages. Checked by the skill's new `check_math.mjs` (chat KaTeX + GitHub rules + `--github` / `--github-published`), which `verify_kb.py` now runs. Chat KaTeX check still 0 errors. Paper checkers unchanged except one new power-laws wording flag, from `averaged-over-$P$` moving into `\text{}`
- [x] Lecture 3: kb.json updated (coverage 3, 5 readings ingested, 37 images, new caveats), sources.md row 3, verify_kb.py clean and review read, committed and pushed (kbUrl already set)
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
