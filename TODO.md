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
- [x] 04 edited transcript (Sonnet copy-edit; 92/92 timestamps, ratios 0.92–1.07, number multisets identical; parent turned four guessed restorations back into [Ed: unclear] — "plot/plain" ← "Python" at 46:50, "AI" at 52:18, "more harmless" ← "less harmless" at 54:38 (kept as spoken, noted), "scratchpads" ← "sketches" at 1:06:12 — and trimmed an outside-knowledge note on GSPO)
- [x] 05 Part 5 | Planning and Multi-Step Reasoning — video Ml_fp9XkB8Y (verbatim → raw/transcripts/original/, 96 paragraphs, [0:05]–[1:14:33])
- [x] 05 edited transcript (Sonnet copy-edit; 96/96 timestamps, no ratio outliers, number multisets identical; parent returned the agent's outside-knowledge "AIME" ← "Amy" (≈24:19) to [Ed: unclear], corrected "GPQA domain" to GPQA Diamond (≈43:56), marked both "fork"s (≈20:21, ≈34:29) unclear, and fixed two header claims — a non-existent [Ed] note at ≈47:00, and "Glenn Hughes" being in SWiRL's appendix (it is in Figure 2))
- [x] 06 Part 6 | Train Time Scaling/Scaling RL — video yVnmHSAy3ck (verbatim → raw/transcripts/original/, 94 paragraphs, [0:05]–[1:12:25])
- [x] 06 edited transcript (Sonnet copy-edit; 94/94 timestamps, number multisets identical except the declared CS320A→CS329A and "quantity to be model"→Qwen-32B; ratio outliers 11:42, 33:35, 1:10:06 are inserted [Ed] notes; parent returned the agent's outside-the-captions "Minerva" (≈41:27, no name is spoken) to an [Ed] note, marked "lower updates" (≈50:47) unclear, accepted "DAPO kind of techniques" ← "double" (≈1:02:22) from the passage's STaR→GRPO→DAPO order, and cut outside-knowledge remarks from the header — GPT-3.5's parameter count, the o1-series guess, and a wrong claim that the lecture gives 51.7% as a MATH figure)
- [x] 07 Part 7 | Self-Improvement and Deep Research Agents — video Uni9dqyuuDM (verbatim → raw/transcripts/original/, 93 paragraphs, [0:05]–[1:12:15])
- [x] 07 edited transcript (Sonnet copy-edit; the agent was killed by a session limit after writing all 93 paragraphs but before its header lists, so the parent adjudicated the diff and wrote the header. 93/93 timestamps identical, number multisets identical except the declared "AlphaCode two"→"AlphaCode 2"; ratio outliers 11:01, 20:30, 35:23, 53:17 are inserted [Ed] turn-boundary notes. Restorations accepted: HumanEval, CodeContests (V2), AlphaGo 2→AlphaCode 2, pass@k/pass@1, Azalia, STaR, Reason-in-Documents; parent added MuSiQue and corrected the intro's claim that AlphaCode 2 has a LaTeX source. All wiki/topic-page lecture quotes re-checked: two adjusted (a comma; "bottlenecking" quote paraphrased))
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
- [x] 04 — raw/papers/04-react.md (main body only; Sonnet). check_paper_file.py --main-only: sections, subsections, figure images, all 44 LaTeX paragraphs, macros and leftovers pass; 4 FAILs confirmed artifacts — Figure 2 is `\captionof` (not a figure env), Tables 3 and 4 share one table env, 8 "missing" numbers are minipage/includegraphics widths and a `2023` path, 4 unmatched paragraphs are the three italic `\multirow` notes and Table 3's caption (`BUTLER$_g$` written as GitHub-safe math). check_math clean (25 formulas)
- [x] 04 — raw/papers/04-rlef.md (main body only; Sonnet). check_paper_file.py --main-only: sections, subsections, 4 figures, 3 tables, macros and leftovers pass; 3 FAILs confirmed artifacts — the 1 "missing" number is a `.44\linewidth` subtable width, the 2 "unfound" LaTeX paragraphs are Table 1's NiceTabular rows (all present in the markdown with their n@k and values), the 2 unmatched markdown paragraphs are the italic `\multicolumn` notes. check_math clean (55 formulas)
- [x] 04 — raw/papers/04-constitutional-ai.md (main body only; Sonnet). check_paper_file.py --main-only: every check passes (7 sections, 16 subsections, 10 figures with images, 112/112 LaTeX paragraphs, 121/121 markdown paragraphs traced); check_math clean. Source quirks kept or noted by the agent: a stray `]` after an appendix reference dropped, "may note be" kept verbatim, Scheurer et al. cited as n.d. (no year in the .bbl or the PDF)
- [x] 05 — mapping confirmed: the transcript covers three of the five row-5 readings, in order (LATS ≈0:05, SPRINT ≈23:29, SWiRL ≈50:03); ADaPT and Wider or Deeper? (AB-MCTS) are not discussed — every "adapt" in the captions is the verb
- [x] 05 — licences checked on the arXiv abstract pages (2026-09-14): LATS (2310.04406) and SWiRL (2504.04736) CC BY 4.0; SPRINT (2506.05745) CC BY-NC-SA 4.0 → user chose link and discuss only; ADaPT (2311.05772) and Wider or Deeper? (2503.04412) arXiv non-exclusive → linked only, summarised from their abstracts
- [x] 05 — LaTeX sources for all five fetched to raw/pdfs/papers/src/<id>/ (gitignored)
- [x] 05 — figure crops, main bodies of the two CC BY readings → raw/images/05-planning-and-multi-step-reasoning/: lats-figure-1–2, swirl-figure-1–8 (10); text-layer check clean on all
- [x] 05 — raw/papers/05-swirl.md (main body only; Sonnet). check_paper_file.py --main-only: 5 sections, 5 subsections, 8/8 figures with images, 3/3 tables, 67/67 table numbers, every LaTeX paragraph found; 1 FAIL confirmed artifact — the 3 unmatched markdown paragraphs are the italic table-flattening notes. check_math clean (26 formulas). Parent moved the three table captions above their tables (spec), replaced `<br>` in two-row headers with spaces, and cut a LaTeX-internal remark from a note. Source quirks kept: "Section 4" for a figure label after "Section" (as the PDF prints), "Gemini-2-27b", "GSM8k", two .bbl keys for BeerQA (Qi et al., 2021a/b)
- [x] 05 — raw/papers/05-lats.md (main body only; Sonnet). check_paper_file.py --main-only: every check passes (6 sections, 8 subsections, 2 figures with images, 10 tables, 144/144 table numbers, 61/61 LaTeX paragraphs, 59/59 markdown paragraphs traced); check_math clean (156 formulas). Source quirk kept verbatim: Table 5 cites ReAct's row as (Wei et al., 2022), as the published PDF also prints
- [x] 06 — mapping confirmed: the transcript covers all three row-6 readings in order (STaR ≈15:37, DeepSeekMath ≈40:41, DAPO ≈53:06)
- [x] 06 — licences checked on the arXiv abstract pages (2026-09-15): STaR (2203.14465), DeepSeekMath (2402.03300) and DAPO (2503.14476) all arXiv non-exclusive → linked, discussed and cited by section, figure, table and equation only; NOT transcribed, no figures (AGENTS.md rule, lecture 3 precedent)
- [x] 06 — LaTeX sources fetched to raw/pdfs/papers/src/<id>/ (gitignored) and main bodies read for the wiki; printed figure and table numbers taken from the PDFs' text layer
- [x] 07 — mapping resolved: catalog position 7 is site row 8. The transcript covers all three row-8 readings in order (AlphaCode ≈0:52, AlphaCode 2 ≈24:20, Search-o1 ≈46:18) and none of row 7's; row 7 has no video. ≈45:31–46:18 refers to "scientist style of work that folks covered last lecture" (row 7 lists The AI Scientist)
- [x] 07 — licences checked (2026-09-15): AlphaCode (2203.07814) is CC BY 4.0 on its arXiv page, but the PDF prints "© 2022 DeepMind. All rights reserved" → user chose "Transcribe main body" (rely on arXiv). AlphaCode 2 Technical Report (Google DeepMind storage PDF) prints "All rights reserved" and Search-o1 (2501.05366) is arXiv non-exclusive → linked, discussed and cited only; no figures
- [x] 07 — LaTeX sources for AlphaCode and Search-o1 fetched to raw/pdfs/papers/src/<id>/ (gitignored); AlphaCode 2 read from its PDF text layer
- [x] 07 — AlphaCode main-body figure crops → raw/images/07-self-improvement-and-deep-research-agents/alphacode-figure-1–4, 6–13 (12, 1.7MB). Figures 1, 3, 6 had no graphics found by the script and Figure 7's box grew downward over Table 6, so those four were re-cropped with explicit --clip boxes from the page geometry. Figure 5 is an lstlisting (text) → transcribed as text, no image. Text-layer check clean on all 12 (Figure 3's "below caption" flag is its side caption's own numbered list)
- [ ] 07 — raw/papers/07-alphacode.md (main body only; Sonnet; check with --main-only --no-crop 5)
- [ ] 08 — 3 readings (site row 17)
- [x] 09 — none listed; nothing to ingest

## Wiki
- [x] wiki/07-self-improvement-and-deep-research-agents.md (Opus; drafted from the verbatim captions, AlphaCode and Search-o1 LaTeX, and the AlphaCode 2 text layer while the transcript agent ran; 6 AlphaCode figures embedded — 1, 4, 6, 8, 9, 13; lecture-vs-paper discrepancies stated; numbering note confirms position 7 = row 8)
- [x] New topic page wiki/retrieval-and-deep-research.md (deep research as a workflow, retrieval in lecture 2's discussion, ReAct and SWiRL search, Search-o1 — lectures 1, 2, 4, 5, 7)
- [x] Topic pages updated for lecture 7 (test-time-scaling, verifiers, agentic-workflows, reasoning-models, scaling-laws, self-improvement, reinforcement-learning); wiki/06's AlphaCode preview now links forward to lecture 7
- [x] INDEX.md — lecture 7, coverage 7 of 9, readings note, numbering (positions 2–7 confirmed), topic one-liners, new retrieval page, raw/papers and raw/images
- [x] sources.md rows 7 and 8 (row 7 no video; row 8 licences, full-text link), schedule table, "ingested so far", mapping paragraph; AGENTS.md (licences, images, crop notes, numbering); kb.json (coverage 7, 11 topic pages, 11 readings, 76 images, lecture 7 caveats, mapping caveat) — to re-verify once the AlphaCode paper file exists
- [x] wiki/06-train-time-scaling-scaling-rl.md (Opus; drafted from the verbatim captions and the three LaTeX sources while the transcript agent ran; no images; lecture-vs-paper discrepancies stated)
- [x] 06 — page's quotes and garble-dependent wording re-checked against the edited transcript (every lecture quote in wiki/06, the RL page and the new topic-page sections found; ≈57:47 note reworded to match the transcript)
- [x] New topic page wiki/reinforcement-learning.md (one objective, reward sources, PPO, GRPO, DAPO, outcome vs process, RL vs SFT, what RL improves — lectures 1, 3–6)
- [x] Topic pages updated for lecture 6 (self-improvement, llm-training-pipeline, reasoning-models, chain-of-thought, test-time-scaling, verifiers, scaling-laws)
- [x] INDEX.md — lecture 6, coverage 6 of 9, readings note, positions 2–6, topic one-liners, new RL page
- [x] sources.md row 6 (and schedule table, "ingested so far", AlphaCode evidence for position 7), AGENTS.md (licences, numbering, images), kb.json (coverage 6, 10 topic pages, lecture 6 caveats, mapping caveat)
- [x] wiki/05-planning-and-multi-step-reasoning.md (Opus; drafted from the verbatim captions and the LaTeX sources while the transcript agent ran; 9 figures embedded — LATS 1, 2; SWiRL 1–5, 7, 8; lecture-vs-paper discrepancies stated; ADaPT and Wider or Deeper? from abstracts)
- [x] 05 — page's quotes and garble-dependent wording re-checked against the edited transcript (two paraphrased quotes fixed; AIME name removed; RFT's paper expansion added)
- [x] Topic pages updated for lecture 5 (agentic-workflows, test-time-scaling, self-improvement, llm-training-pipeline, reasoning-models, verifiers; no new pages; llm-training-pipeline also gained its missing lecture 4 entry)
- [x] INDEX.md — lecture 5, coverage 5 of 9, readings, topic one-liners, raw/papers and raw/images
- [x] sources.md row 5 (and the stale row-4 status and "ingested so far" line), AGENTS.md (licences, images, numbering), kb.json (coverage 5, 10 readings, 64 images, caveats) — re-verified by verify_kb.py once both paper files existed
- [x] wiki/04-learning-from-feedback-with-tools-code.md (Opus; drafted from the verbatim captions and the LaTeX sources while the transcript agent ran; 9 figures embedded — ReAct 1, 3; RLEF 1–3; Constitutional AI 1, 2, 5, 8; lecture-vs-paper discrepancies stated)
- [x] 04 — page's quotes re-checked against the edited transcript (all 10 found); the closing-question line on a student's agent definition rewritten after "scratchpads" became [Ed: unclear]
- [x] Topic pages updated for lecture 4 (agentic-workflows: ReAct loop, RLEF coding agents; self-improvement: where the feedback comes from; verifiers: tests inside an RL loop, AI feedback as a judge; chain-of-thought: grounding; llm-training-pipeline: RLAIF; no new pages)
- [x] INDEX.md — lecture 4, coverage 4 of 9, readings, topic one-liners, raw/papers and raw/images
- [x] sources.md row 4, AGENTS.md (licences, images), kb.json (coverage 4, 8 readings, 54 images, caveats) — pending re-verification once the three paper files exist
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
- [x] raw/images/05-planning-and-multi-step-reasoning/ — LATS Figures 1–2, SWiRL Figures 1–8 (main bodies of the two CC BY readings); 9 embedded in wiki/05, all 10 to be embedded in the paper files

## Publish
- [x] Lecture 2: kb.json updated, verify_kb.py clean, committed and pushed (kbUrl already set)
- [x] KaTeX pass (2026-09-14, user request for lectures 1–2): every formula in wiki/ and raw/papers/ rendered through the extension's own marked 18 + KaTeX 0.18.1 pipeline — 542 formulas, 0 errors, all tokenized as written. Fixed two multi-`\tag` `aligned` blocks in raw/papers/02-monkeys-power-laws.md (Eqs 5–6, 8–9; split one block per tag); lecture 1–2 prose pass@k / best-of-N / N× converted to inline math; AGENTS.md and PAPER_FILE_SPEC.md now state the KaTeX rules
- [x] GitHub math pass (2026-09-14, user: formulas must also work on github.com): github.com runs markdown over math before MathJax, so 175 of 545 formulas were unrecognised or altered there (backslash-punctuation escapes, `*`/`_` italics pairing, double-escaped `<`, `$` after a hyphen or quote, math inside italics, `%`). Rewrote the math to forms both renderers accept — rules now in AGENTS.md and PAPER_FILE_SPEC.md — and checked every formula against GitHub's markdown API with MathJax parsing: 536 of 537 matched at first; the one left, power-laws Eq. 10, was a `_` after punctuation pairing into italics inside a `$$` block (which double-escaped its `&`s), so the underscore rule now covers display math too (32 more underscores spaced). Final: every formula renders through GitHub's markdown API and MathJax, confirmed on the published github.com file pages. Checked by the skill's new `check_math.mjs` (chat KaTeX + GitHub rules + `--github` / `--github-published`), which `verify_kb.py` now runs. Chat KaTeX check still 0 errors. Paper checkers unchanged except one new power-laws wording flag, from `averaged-over-$P$` moving into `\text{}`
- [x] Lecture 3: kb.json updated (coverage 3, 5 readings ingested, 37 images, new caveats), sources.md row 3, verify_kb.py clean and review read, committed and pushed (kbUrl already set)
- [x] Lecture 4: kb.json updated (coverage 4, 8 readings ingested, 54 images, new caveats), sources.md row 4, AGENTS.md, verify_kb.py clean (all hard checks, GitHub math included) and review read — the 22 math warnings are pre-existing plain-text pass@k/× in pages from lectures 1–3; committed and pushed (kbUrl already set)
- [x] Math notation pass (2026-09-14, user request): the 22 plain-text notations verify_kb flagged in lecture 1–3 pages (pass@1, pass@k, Pass@K, Pass@100, best-of-N, 30×, 2.6×, 4×, ~14×) converted to inline math in wiki/03, reasoning-models, self-improvement, test-time-scaling; check_math --github 0 errors, 0 warnings
- [x] Lecture 5: kb.json updated (coverage 5, 10 readings ingested, 64 images, new caveats), sources.md row 5, AGENTS.md, verify_kb.py clean (all hard checks, GitHub math included) and review read — numeric claims (images only in lectures 2–5; LATS 1–2 + SWiRL 1–8 = 10 on disk, 9 in wiki) match the measured distribution; committed and pushed (kbUrl already set)
- [x] Lecture 6: kb.json updated (coverage 6, 10 topic pages, readings still 10 ingested — lecture 6's three are linked only, images unchanged at 64, new caveats), sources.md row 6, AGENTS.md, verify_kb.py clean (all hard checks, GitHub math included; 984 formulas) and review read — numeric claims (images only in lectures 2–5, none for lecture 6) match the measured distribution; committed and pushed (kbUrl already set)
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
