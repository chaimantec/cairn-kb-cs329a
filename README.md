# CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)

A compiled knowledge base for Stanford's CS329A seminar, taught by **Aakanksha Chowdhery** and
**Azalia Mirhoseini**. It is built to be **read by an AI agent answering a question about the
course** — every claim traceable to a lecture timestamp or to the paper it came from — and it
reads just as well for a human following along.

**Start at [`INDEX.md`](INDEX.md).** It carries the course summary and an annotated table of
contents: one line on what each page holds, which is enough to pick the right page without
reading them all.

## What is in here

All **nine recorded lectures**, complete — the whole of the catalog's recorded course. Each has an
edited transcript that keeps its `[MM:SS]` marks, a wiki page, and figures where the course
material supplied them. Eleven cross-lecture topic pages (verifiers, test-time scaling,
reinforcement learning, retrieval and deep research, …) carry the ideas that span lectures.

The course publishes no slides — its material is the **paper reading list**, so the readings the
site lists for each lecture are transcribed in `raw/papers/` and cited at their original URLs in
[`sources.md`](sources.md). [`SEE_ALSO.md`](SEE_ALSO.md) lists sibling Cairn KBs worth reading
alongside this one.

## Reading it from Cairn

Open the extension, play a lecture from this course, and switch the Live tab to **Chat**. The
chat reads `INDEX.md` first and follows relative links from there; its tool chips should show
`kb_read INDEX.md`.

## Reading it from any other agent

Fetch the entry point and follow the links out of it:

`https://raw.githubusercontent.com/chaimantec/cairn-kb-cs329a/main/INDEX.md`

Three things worth knowing before you spend a turn:

- **Every link is relative and resolves.** Resolve it against the repo root, and swap
  `github.com/<path>/blob/` for `raw.githubusercontent.com/<path>/` to get plain markdown instead
  of an HTML page.
- **[`kb.json`](kb.json)** states coverage and provenance in machine-readable form. Read it to
  know what this KB does and does not cover, and how far to trust a citation, before quoting one.
- **Never construct an image path.** Find it in the file that shows the figure and use the path
  that is written there; a guessed path costs a turn and renders nothing.

## How far to trust it

Complete, but not uniformly deep. Lectures 1 and 9 list no readings on the course site and are
built from their transcripts alone. Only papers whose licence permits republication have their
full text and figures here; the rest are linked at their original URL and discussed from the
lecture. `kb.json` carries the full list of caveats — worth reading before quoting a number.

## Provenance

Compiled by Claude agents from the lecture transcripts and the course's paper readings. The edited
transcripts preserve every timestamp, and the verbatim auto-captions are kept beside them in
`raw/transcripts/original/` so the editing can be checked against what was actually said.
[`AGENTS.md`](AGENTS.md) describes how the repo is organised and the conventions to keep if you
extend it.
