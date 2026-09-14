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

Only **lectures 2, 3 and 4** have images, each named `<paper>-figure-N` after the paper file it
belongs to. Lecture 2: every figure in the transcribed parts of its four readings, in
`raw/images/02-test-time-compute-scaling/`. Lecture 3: **Weaver's Figures 1–6 only** (its main body),
in `raw/images/03-robust-verification/` — the other three lecture 3 readings are not licensed for
republication, so none of their figures is here. Lecture 4: every main-body figure of its three
readings — ReAct Figures 1–3, RLEF Figures 1–4, Constitutional AI Figures 1–10 — in
`raw/images/04-learning-from-feedback-with-tools-code/`. Lectures 1 and 5–9 have no images.

Constitutional AI prints its figure labels with no colon ("Figure 1" then the caption), which the
skill's `extract_paper_figures.py` did not recognise as a caption when lecture 4 was built. Its crops
were made with a copy whose caption pattern also accepts "Figure N" followed by capitalised text, and
they passed the same text-layer check as every other crop. The skill's script has since taken the same
pattern, regression-tested to give identical crops on the other ten lecture 2–4 paper PDFs.

- Each image is **one figure as published, cropped from the paper's PDF together with its
  caption** — not the whole page. They are reproduced under the papers' CC BY 4.0 licences, and the
  attribution is the paper file's front matter (authors, arXiv URL, licence), under which every image
  sits beside its own caption.
- **Link images relatively**, like every other file here: `../images/02-…` from `raw/papers/`,
  `../raw/images/02-…` from `wiki/`. To show one in chat, read that path; the read returns the
  renderable URL.
- **Use an image path you have actually read in a file.** Never construct one from the naming
  pattern, and never assume a figure has an image because its neighbours do: the power-laws paper's
  Figure 8 is an algorithm box, transcribed as text, with no image. The paper files carry every
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
  nine videos and the site twenty rows; by title, positions 1–6 are rows 1–6, position 8 is row 17,
  position 9 is row 20, and position 7 may be row 7, row 8 or both. Positions 2, 3 and 4 are confirmed against their
  transcripts, each of which discusses every reading of its row; confirm each later position the
  same way before ingesting its readings, and record the resolution in the lecture page
  and `sources.md`. Rows 13 and 14 list readings but have no video in the catalog.
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
