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
- [ ] 02 — raw/papers/02-archon.md (main body only)
- [ ] 02 — figure-description audit of every figure in all four papers (one agent; Snell Fig 9 already checked by parent)
- [ ] 03 — 4 readings
- [ ] 04 — 3 readings
- [ ] 05 — 5 readings
- [ ] 06 — 3 readings
- [ ] 07 — resolve site row 7 vs 8 first
- [ ] 08 — 3 readings (site row 17)
- [x] 09 — none listed; nothing to ingest

## Wiki
- [ ] wiki/02-test-time-compute-scaling.md
- [ ] Topic pages updated/added for lecture 2
- [ ] INDEX.md — lecture 2, raw/papers
- [x] wiki/01-course-overview.md
- [x] Topic pages (cross-lecture concepts lecture 1 establishes): test-time-scaling, verifiers, chain-of-thought, reasoning-models, llm-training-pipeline, scaling-laws, agentic-workflows, self-improvement, course-logistics
- [x] INDEX.md table of contents
- [x] AGENTS.md — CS329A conventions (no slides, papers as material, numbering table)

## Images (paper figures, cropped from the PDFs — user opted in for lecture 2)
- [x] raw/images/02-test-time-compute-scaling/ — figure crops for every figure in a transcribed part: Monkeys 1–10, power laws 1–7, Snell 1–9, Archon 1–5 (31). Appendix-only crops deleted when appendices were descoped.
- [ ] Wire figures into wiki/02 (paper files embed them as written)
- [ ] AGENTS.md — Images and raw/papers conventions

## Publish
- [x] kb.json — coverage, materials.method, provenance caveats
- [x] SEE_ALSO.md, if a sibling KB is genuinely relevant
- [x] verify_kb.py clean, and its review section read
- [x] Commit and push
- [x] PATCH kbUrl onto the catalog entry
