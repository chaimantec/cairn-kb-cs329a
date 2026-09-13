# How this knowledge base is organized

This repo is the knowledge base for **CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)**,
taught by Aakanksha Chowdhery and Azalia Mirhoseini. It is read by Cairn's in-extension AI chat,
which fetches files over raw.githubusercontent.com and follows relative markdown links.

It is written by **Claude Opus 5** and **Claude Sonnet 5** running as agents. Opus 5 writes the
wiki prose and makes the editorial calls; Sonnet 5 does script-checkable bulk work — currently the
copy-edit of the auto-generated captions, which the parent verifies mechanically (timestamp
sequence, number inventory, per-paragraph word-count ratio). Keep that split if you extend it: the
wiki prose is not delegated, because nothing scores it.

## Layout

| Path                | Contents                                                      |
| ------------------- | ------------------------------------------------------------- |
| `INDEX.md`          | Entry point. Course summary + annotated table of contents.    |
| `wiki/`             | Durable pages: one per lecture, plus cross-lecture topics.    |
| `raw/transcripts/`  | Edited lecture transcripts with `[MM:SS]` paragraph marks.    |
| `raw/transcripts/original/` | Verbatim captions. Reference only — prefer the edited ones. |
| `sources.md`        | Every paper reading on the course site, with its original URL. |
| `kb.json`           | Machine-readable coverage and provenance. Read this to know what this KB does and does not cover, and how far to trust a citation. |
| `SEE_ALSO.md`       | Sibling KBs the chat may read with `kb_read(kb: ...)`.        |
| `TODO.md`           | Build tracker. Unchecked boxes are outstanding work.          |

There is no `raw/slides/` and no `raw/images/`: the course publishes no slides publicly.

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
- When paper material is ingested for a later lecture, give it its own `raw/papers/` directory
  and cite it by section, figure or table, the way slide files are cited by slide number. As of
  2026-09-13, 32 of the 34 listed papers are arXiv-hosted; of nine arXiv licences spot-checked,
  eight were CC BY 4.0 while *Training Verifiers to Solve Math Word Problems* (Cobbe et al. 2021) is
  arXiv's non-exclusive licence. Check each paper's licence before committing any of its text.

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
  position 9 is row 20, and position 7 may be row 7, row 8 or both. Confirm each against its
  transcript before ingesting that lecture's readings, and record the resolution in the lecture page
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
- **Prose over fragments.** The chat quotes these pages to learners; bullet fragments quote badly.
- **Never rank lectures against each other.** State the measurement for the lecture in front of you.

## Rebuilding

Built and updated by the `cairn-kb` skill. To add newly released lectures, append entries to
`TODO.md` and re-run the skill — it only does unchecked work.
