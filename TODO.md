# KB build — CS329A (Self-Improving AI Agents, Stanford, Autumn 2025)

The Cairn catalog lists **9 recorded lectures** ("Part 1" … "Part 9"). The course publishes
**no slides** (they are on Canvas only), so the course material this KB ingests is the
**paper readings** the course website lists per lecture — exactly that list, nothing else. Paper
PDFs are downloaded to `raw/pdfs/` on disk and are **gitignored**; every paper is cited at its
original URL as the course website links it.

- **Run 1** (in progress): lecture 1 — Course Overview. The site lists **no readings** for it, so
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
- [ ] 01 edited transcript (Sonnet copy-edit; parent checks timestamps, numbers, per-paragraph ratio)
- [ ] 02 Part 2 | Test-Time Compute Scaling — video -Ggc37xLj_Y
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
- [x] 01 — none listed; nothing to ingest
- [ ] 02 — 4 readings
- [ ] 03 — 4 readings
- [ ] 04 — 3 readings
- [ ] 05 — 5 readings
- [ ] 06 — 3 readings
- [ ] 07 — resolve site row 7 vs 8 first
- [ ] 08 — 3 readings (site row 17)
- [x] 09 — none listed; nothing to ingest

## Wiki
- [x] wiki/01-course-overview.md
- [x] Topic pages (cross-lecture concepts lecture 1 establishes): test-time-scaling, verifiers, chain-of-thought, reasoning-models, llm-training-pipeline, scaling-laws, agentic-workflows, self-improvement, course-logistics
- [x] INDEX.md table of contents
- [x] AGENTS.md — CS329A conventions (no slides, papers as material, numbering table)

## Publish
- [x] kb.json — coverage, materials.method, provenance caveats
- [x] SEE_ALSO.md, if a sibling KB is genuinely relevant
- [ ] verify_kb.py clean, and its review section read
- [ ] Commit and push
- [ ] PATCH kbUrl onto the catalog entry
