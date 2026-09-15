# How this knowledge base is organized

This repo is the knowledge base for **CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)**,
taught by Aakanksha Chowdhery and Azalia Mirhoseini. It is read by Cairn's in-extension AI chat,
which fetches files over raw.githubusercontent.com and follows relative markdown links.

It is written by **Claude Opus 5** and **Claude Sonnet 5** running as agents. Opus 5 writes the
wiki prose and makes the editorial calls; Sonnet 5 does script-checkable bulk work — the copy-edit of the auto-generated
captions, which the parent verifies mechanically (timestamp sequence, number inventory,
per-paragraph word-count ratio), and the transcription of paper readings from LaTeX source, which
`check_paper_file.py` verifies against that source. Keep that split if you extend it: the
wiki prose is not delegated, because nothing scores it.

## Layout

| Path                | Contents                                                      |
| ------------------- | ------------------------------------------------------------- |
| `INDEX.md`          | Entry point. Course summary + annotated table of contents.    |
| `wiki/`             | Durable pages: one per lecture, plus cross-lecture topics.    |
| `raw/transcripts/`  | Edited lecture transcripts with `[MM:SS]` paragraph marks.    |
| `raw/transcripts/original/` | Verbatim captions. Reference only — prefer the edited ones. |
| `raw/papers/`       | Full text of the ingested paper readings: `NN-<paper>.md` (main body) and `NN-<paper>-appendix.md`. |
| `raw/images/NN-<lecture>/` | Figures cropped from those papers' PDFs, embedded in the paper files and the wiki. |
| `sources.md`        | Every paper reading on the course site, with its original URL. |
| `kb.json`           | Machine-readable coverage and provenance. Read this to know what this KB does and does not cover, and how far to trust a citation. |
| `SEE_ALSO.md`       | Sibling KBs the chat may read with `kb_read(kb: ...)`.        |
| `TODO.md`           | Build tracker. Unchecked boxes are outstanding work.          |

There is no `raw/slides/`: the course publishes no slides publicly. Paper readings take their place,
in `raw/papers/`.

### This course has papers, not decks

CS329A's lecture materials go to Canvas, and its public site links no slides. What it does publish
is a **paper reading list per lecture**, and that list is the course material this KB draws on —
the role slide decks play in other Cairn KBs.

- **Ingest exactly the papers the site lists for a lecture.** Not papers from another lecture's
  list, and not papers a lecture happens to cite or show. A lecture with no readings listed is
  built from its transcript alone — lecture 1 (Course Overview) is the first such case, and the
  site also lists none for Future Research Areas (catalog position 9).
- **Cite every paper at its original URL**, as the site links it (arXiv abstract pages, and two
  PDFs on `storage.googleapis.com` for AlphaEvolve and AlphaCode 2). `sources.md` lists them all.
- **PDFs are not committed.** They are downloaded to `raw/pdfs/papers/` on the build machine,
  which is gitignored, so a relative link into `raw/pdfs/` resolves locally and 404s for every
  reader. Never write one.
- When a lecture **previews** a paper that the site lists under a later lecture, link it at its
  original URL and say which schedule row lists it; do not ingest it for the earlier lecture.
  Lecture 1's page has a table of five such papers.
- **Check each paper's licence before committing any of its text or figures.** As of 2026-09-13,
  32 of the 34 listed papers are arXiv-hosted. The four lecture 2 readings are all CC BY 4.0, checked
  on each abstract page, and so are lecture 4's three (ReAct, RLEF, Constitutional AI), checked on
  2026-09-14. Of lecture 3's four, only Weaver is CC BY 4.0: *Training Verifiers to Solve
  Math Word Problems* (Cobbe et al. 2021), *Let's Verify Step by Step* (Lightman et al. 2023) and
  *Math-Shepherd* (Wang et al. 2023) carry arXiv's non-exclusive licence, which does not permit
  republishing their text or figures here. A paper like that is linked at arXiv, discussed and cited
  by section, figure and table in the wiki, and not transcribed; no image of it is committed.
  Lecture 5's five were checked on 2026-09-14: LATS and SWiRL are CC BY 4.0 and transcribed; ADaPT and
  *Wider or Deeper?* carry the non-exclusive licence and are linked only. SPRINT is **CC BY-NC-SA
  4.0**, whose non-commercial and share-alike terms would attach to a transcription in a KB that a
  commercial product reads, so it too is linked and discussed only, with no figures. Lecture 6's three (STaR,
  DeepSeekMath, DAPO) were checked on 2026-09-15 and all carry the non-exclusive licence, so all three are linked,
  discussed and cited only. Lecture 7's three were checked on 2026-09-15. AlphaCode's arXiv abstract page lists
  **CC BY 4.0**, but the published PDF prints "© 2022 DeepMind. All rights reserved". The user decided to rely on
  the arXiv licence grant, so its main body is transcribed and its figures committed. The AlphaCode 2 Technical
  Report is a PDF on Google DeepMind's storage that prints "All rights reserved", and Search-o1 carries the
  non-exclusive licence, so both are linked, discussed and cited only. Lecture 8's three (METR's *Measuring AI
  Ability to Complete Long Tasks*, GDPval, DeepScholar-Bench) were checked on 2026-09-15 and are all CC BY 4.0, so
  their main bodies are transcribed and their figures committed.
- **Transcribe the version the lecture was taught from.** A reading can be revised on arXiv after the lecture, and
  the abstract link then serves text whose numbers and names no longer match what the lecturer quotes. Two of
  lecture 8's readings were: METR's paper is at v4 (retitled *…Long Software Tasks*) and DeepScholar-Bench at v2.
  By user decision the KB transcribes the version current on the lecture date — METR's v2 and DeepScholar-Bench's
  v1 — fetched from `https://arxiv.org/e-print/<id>v<n>` into `raw/pdfs/papers/src/<id>v<n>/`, and says so in the
  paper file's front matter, the lecture page and `sources.md`. Check a reading's arXiv submission history against
  the lecture date before transcribing it.

### Paper files (`raw/papers/`)

Each ingested reading is two files, named by the catalog lecture that lists it:
`raw/papers/NN-<paper>.md` (abstract and main body) and `raw/papers/NN-<paper>-appendix.md`.

- **They are the paper's full text, not a summary.** Prose, equations, tables and footnotes are
  transcribed from the **arXiv LaTeX source** (the e-print), so equations and table cells are exact
  rather than read off page images. Custom macros are expanded so the math renders. Citations are
  resolved to author–year and the bibliography is omitted.
- **Headings carry the paper's printed numbering** (`## 3 …`, `### 3.1 …`, `## A …`), and each file
  opens with a Contents table mapping sections to their figures and tables. Cite a paper by section,
  figure or table — "Brown et al. (2024), Figure 7" — linking to the file, the way a slide file is
  cited by slide number.
- **Every figure** appears where the LaTeX places it: the verbatim caption as `**Figure N.**`, then
  the image. **This KB writes no figure descriptions of its own.** What a figure shows is what its
  caption and the paper's text say. To answer a question about a chart, show the image and quote the
  caption and the passage that discusses it — never read values off the chart. (Model-written chart
  readings were tried for lecture 2 and removed: an audit found errors in 4 of the 20 it checked.)
- Transcribed by Claude Sonnet 5 from the source and checked by `check_paper_file.py`: section,
  figure and table counts against the LaTeX; every number inside the LaTeX tables present in the
  markdown; every paragraph's wording against the source, so a paraphrased or dropped paragraph
  fails; image links that resolve; no leftover `\cite`, `\ref` or macros. Every figure crop was checked
  against the PDF's text layer: no line of text cut by the crop edge, the caption's last words inside
  the crop, and no body text below the caption.

### Images

Only **lectures 2, 3, 4, 5, 7 and 8** have images, each named `<paper>-figure-N` after the paper file it
belongs to. Lecture 2: every figure in the transcribed parts of its four readings, in
`raw/images/02-test-time-compute-scaling/`. Lecture 3: **Weaver's Figures 1–6 only** (its main body),
in `raw/images/03-robust-verification/` — the other three lecture 3 readings are not licensed for
republication, so none of their figures is here. Lecture 4: every main-body figure of its three
readings — ReAct Figures 1–3, RLEF Figures 1–4, Constitutional AI Figures 1–10 — in
`raw/images/04-learning-from-feedback-with-tools-code/`. Lecture 5: the main-body figures of its two CC BY
readings only — LATS Figures 1–2 and SWiRL Figures 1–8 — in `raw/images/05-planning-and-multi-step-reasoning/`;
SPRINT, ADaPT and *Wider or Deeper?* have none. Lecture 7: **AlphaCode's main-body figures only** — Figures 1–4
and 6–13 — in `raw/images/07-self-improvement-and-deep-research-agents/`; AlphaCode's Figure 5 is a text listing,
transcribed as text, and the AlphaCode 2 Technical Report and Search-o1 have none. Lecture 8: the main-body figures of all three
readings, from the versions transcribed — METR's v2 Figures 1–13, GDPval Figures 1–9 and DeepScholar-Bench v1
Figures 1–3 — in `raw/images/08-agentic-evaluations-and-long-horizon-tasks/`; their numbers follow those versions,
not the current arXiv ones. Lectures 1, 6 and 9 have no images; lecture 6's three readings are not licensed for republication.

Constitutional AI prints its figure labels with no colon ("Figure 1" then the caption), which the
skill's `extract_paper_figures.py` did not recognise as a caption when lecture 4 was built. Its crops
were made with a copy whose caption pattern also accepts "Figure N" followed by capitalised text, and
they passed the same text-layer check as every other crop. The skill's script has since taken the same
pattern, regression-tested to give identical crops on the other ten lecture 2–4 paper PDFs.

AlphaCode's figures needed hand-set boxes for four of the twelve. The script found no graphics near the captions
of Figures 1, 3 and 6, and grew Figure 7's box downward over a table. Those four were cropped with explicit
`--clip` boxes taken from the page's text and drawing positions, and all twelve passed the text-layer check.
Figure 3's caption sits beside its code listing and includes a numbered list, so the check's "text below the
caption" flag on it is the caption's own list, confirmed in the LaTeX.

Lecture 8's crops needed two hand-set boxes of 25. The script found no graphics near the caption of METR's (v2)
Figure 9, and GDPval's Figure 2 box began inside the last line of the paragraph above it. Both were re-cropped with
explicit `--clip` boxes taken from the page geometry, and all 25 passed the text-layer check.

- Each image is **one figure as published, cropped from the paper's PDF together with its
  caption** — not the whole page. They are reproduced under the papers' CC BY 4.0 licences (AlphaCode's as
  listed on its arXiv page), and the
  attribution is the paper file's front matter (authors, arXiv URL, licence), under which every image
  sits beside its own caption.
- **Link images relatively**, like every other file here: `../images/02-…` from `raw/papers/`,
  `../raw/images/02-…` from `wiki/`. To show one in chat, read that path; the read returns the
  renderable URL.
- **Use an image path you have actually read in a file.** Never construct one from the naming
  pattern, and never assume a figure has an image because its neighbours do: the power-laws paper's
  Figure 8 is an algorithm box, transcribed as text, with no image, and so is AlphaCode's Figure 5, a text
  listing. The paper files carry every
  image; `grep -o 'raw/images/[^)]*' wiki/02-*.md` lists the ones a wiki page uses.
- For numbers, use a paper's transcribed tables and text. Never state a value read off a figure.

## Conventions

- **INDEX.md is the front door.** The chat reads it first on every conversation. Every wiki page
  must appear there with a one-line description of what it holds. An unindexed page is effectively
  invisible.
- **Relative links between KB pages**, resolved from the linking file — from this root file
  `[verifiers](wiki/verifiers.md)` and `[transcript](raw/transcripts/01-course-overview.md)`,
  from a wiki page `verifiers.md` and `../raw/transcripts/01-course-overview.md`. Absolute GitHub URLs break when the
  repo is renamed or forked. Papers and the course site are the exception: link those at their
  original URLs, since nothing of theirs is committed.
- **Cite everything.** Use an **`[MM:SS]` timestamp**, written `(≈MM:SS)` in the wiki, for what is
  said in a lecture — it is the start of the transcript paragraph containing the statement. Cite the
  course website for logistics it states, and say when the two disagree.
- **Files are named by Cairn catalog position**, not by the site's schedule row. The catalog has
  nine videos and the site twenty rows. Positions 1–6 are rows 1–6, position 7 is row 8 and position 8 is row 17;
  by title, position 9 is row 20. Positions 2–8 are confirmed against their transcripts — 2, 3, 4, 6, 7 and 8 each
  discuss every reading of their row, and 5 discusses three of its five. Confirm position 9 the same way
  before ingesting its readings, and record the resolution in the lecture page and `sources.md`. Rows 7, 13 and
  14 list readings but have no video in the catalog. Lecture 7's remark about "scientist style of work that folks
  covered last lecture" fits row 7, which lists *The AI Scientist*.
- **Never invent course content.** If a source is unclear, say so on the page. Do not fill the gap
  from outside knowledge — the chat presents these pages as authoritative material from this course.
  Recovering a mangled term from unambiguous context is reading the source; supplying a model name,
  benchmark or number the sources do not contain is not. Lecture 1's garbled model names in the
  Large Language Monkeys results are the standing example: the wiki leaves them unnamed.
- **Name the lecturer only where the lecture makes it clear.** Chowdhery and Mirhoseini alternate
  within lectures and the captions do not mark every hand-over.
- **Transcripts are edited, and the originals are kept.** `raw/transcripts/` holds copy-edited
  captions — punctuation, sentence boundaries, restored terms, student questions in *italics*,
  genuine ambiguity marked `[Ed: unclear — …]` — with each file's header listing its restorations.
  Every `[MM:SS]` marker is preserved in order and no sentence is moved across one. The verbatim
  captions are in `raw/transcripts/original/`. Mathematical notation stays spelled out in both.
- **Mathematics in the wiki is LaTeX**, `$...$` inline and `$$...$$` displayed on its own lines.
  Never inside a code fence. Define every symbol on first use and follow the course's notation. A
  literal dollar sign in prose must be escaped as `\$`.
- **Math must render in two places, and they disagree.** Cairn's chat renders `$...$` and `$$...$$`
  with marked and KaTeX (MathML output). github.com first runs its markdown parser over the text and
  hands whatever survives to MathJax. So write only what both accept:
  - **No backslash before punctuation** — on GitHub markdown eats it. Write `\lbrace`/`\rbrace`, not
    `\{`/`\}`; `\thinspace`, `\negthinspace`, `\mskip{5mu}`, not `\,`, `\!`, `\;`; `\Vert`, not
    `\|`; `\cr`, not `\\`; `\verb|#|`, not `\#`; and put `%` outside the math (`$\leq 10$%`).
  - **In inline math no `<` or `>`; in any math no `*`, and no `_` straight after a non-letter.**
    GitHub double-escapes `<` and `>` in inline math (write `\lt`, `\gt`, `\ll`, `\gg`), and pairs
    `*` and `_` into italics across formulas — even inside a `$$` block, where it double-escapes every
    `&` of an `aligned` environment (write `\ast`, and `\mathbb{E}_ {k}` with a space after the `_`).
  - **Delimiters.** An inline `$` opens only after a space, `(` or the start of a line, and closes
    only before a space or punctuation: write `$\text{best-of-}N$`, not `best-of-$N$`. No space just
    inside `$`. Never put math inside `*italics*` — close the italics around it. `$$` goes on its own
    lines with a blank line around the block.
  - **At most one `\tag{}` per `$$` block.** KaTeX rejects several in `aligned` and silently drops
    them from `align`, so a multi-line equation with several numbers becomes one block per numbered
    line, continuation lines starting with `\phantom{<left-hand side>}`.
  - **Notation in prose is math too:** $\text{pass@}k$, $\text{best-of-}N$ and $14\times$, not plain
    `pass@k`, `best-of-N`, `14×` (verbatim quotes excepted).

  The cairn-kb skill checks both: every formula is test-rendered in the chat's own KaTeX, and compared
  one by one with what GitHub's markdown API renders and MathJax parses.
- **Prose over fragments.** The chat quotes these pages to learners; bullet fragments quote badly.
- **Never rank lectures against each other.** State the measurement for the lecture in front of you.

## Rebuilding

Built and updated by the `cairn-kb` skill. To add newly released lectures, append entries to
`TODO.md` and re-run the skill — it only does unchecked work.
