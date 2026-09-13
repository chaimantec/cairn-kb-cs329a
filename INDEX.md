# CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)

CS329A is a Stanford graduate seminar on AI agents that "continuously improve themselves through
interaction with themselves and the environment", taught by **Aakanksha Chowdhery** and **Azalia
Mirhoseini**. It starts from self-improvement techniques for language models — verifiers, scaling
test-time compute, search, train-time scaling with RL — moves to agents augmented with tools, code
and memory, then multi-step reasoning, planning and evaluation, with guest lectures from frontier
labs. Students read research papers for each lecture, do three homeworks, and carry out an original
research project.

> **Coverage note — partial.** This knowledge base covers **catalog lecture 1 of 9** (Course
> Overview) only. It says nothing about any later lecture. Do not cite it as covering the course.
>
> **No slides; papers are the course material.** The course publishes no slides on its public site
> (lecture materials go to Canvas). The course material this KB will draw on for later lectures is
> the **paper reading list** the site gives for each lecture — exactly that list. **Lecture 1 has no
> readings listed**, so it is built from its transcript alone, plus the course website for
> logistics. Papers are always cited at their **original URL**; see [sources.md](sources.md).
>
> **Numbering.** The Cairn catalog has nine videos, "Part 1" to "Part 9"; the site's schedule has
> twenty rows, including guest lectures and midterm presentations that are not in the playlist.
> Repo files use the **catalog position**. By title, positions 1–6 are site rows 1–6, position 8 is
> row 17 and position 9 is row 20; position 7 ("Self-Improvement and Deep Research Agents") may be
> row 7, row 8 or both. Everything after position 1 is tentative until those lectures are built.

## Lectures

- [Lecture 1 — Course Overview](wiki/01-course-overview.md) — the course's argument in one lecture:
  scaling laws and emergent abilities, chain of thought, the pre-training → instruction tuning → RLHF
  pipeline behind ChatGPT, repeated sampling (Large Language Monkeys) and o1-style test-time scaling,
  feeding test-time outputs back into training, how reasoning models think, the move from LLMs to
  agents and agentic workflow patterns, why coding agents became reliable, the generator–verifier
  gap, applications, and course logistics. Includes the student Q&A and links to five later-lecture
  readings the lecture previews.

## Topics

Cross-lecture concept pages. Each currently draws on lecture 1 only and will gather later lectures
as they are built.

- [Test-time scaling](wiki/test-time-scaling.md) — repeated sampling with a verifier, coverage vs
  pass@1 vs pass@k, the latency, temperature and no-verifier limits, and o1's log-linear curve.
- [Verifiers](wiki/verifiers.md) — what a verifier does, unit tests in verifiable domains, verifiers
  vs LLM judges, what to do without one, and the generator–verifier gap.
- [Chain of thought](wiki/chain-of-thought.md) — the tennis-ball prompting example, why it only works
  in large models, chain-of-thought fine-tuning, and whether it was emergent or trained.
- [Reasoning models](wiki/reasoning-models.md) — the thinking behaviours (analysis, decomposition,
  feedback, self-correction, backtracking), the o1 bash example, where they beat GPT-4o, and the Q&A
  on why they work.
- [The LLM training pipeline](wiki/llm-training-pipeline.md) — pre-training, fine-tuning on
  high-quality data, instruction tuning, and RLHF with reward models, as the lecture presents them.
- [Scaling laws](wiki/scaling-laws.md) — loss vs compute, data and parameters, model sizes from BERT
  to GPT-4, few-shot learning, emergent abilities, and the saturation that turned attention to
  inference.
- [Agents and agentic workflows](wiki/agentic-workflows.md) — what makes an agent, today's static
  workflows, building blocks and orchestration patterns (chaining, routing, parallelization,
  orchestrator, evaluator, verifier), coding agents, and applications.
- [Self-improvement](wiki/self-improvement.md) — the course's central idea: test-time generation as
  training data, test generation in coding, feedback as the limit, and what is not yet understood
  about RL.
- [Course logistics](wiki/course-logistics.md) — format, Canvas/Ed/Gradescope, the grading
  breakdown and due dates (including where the site contradicts itself), project rules, late days
  and audits.

## Raw materials

- [`raw/transcripts/`](raw/transcripts/) — **edited** lecture transcripts with `[MM:SS]` paragraph
  marks. Read these: auto-captions were copy-edited for punctuation and mis-heard terms, and
  unrecoverable passages are marked `[Ed: unclear]` rather than guessed.
- [`raw/transcripts/original/`](raw/transcripts/original/) — the verbatim auto-captions, kept as the
  reference for what was actually said.
- [`sources.md`](sources.md) — every paper reading on the course site, grouped by schedule row, with
  its original URL and the catalog position it tentatively belongs to.
- [`kb.json`](kb.json) — machine-readable coverage and provenance, including known caveats.
- [`SEE_ALSO.md`](SEE_ALSO.md) — sibling knowledge bases (CS336, CS224N) worth reading for this
  course, and what each is good for.
- [`AGENTS.md`](AGENTS.md) — how this KB is organized and the conventions for extending it.

Paper PDFs are **not** in this repo. They were downloaded to `raw/pdfs/` on the build machine, which
is gitignored; link and cite papers at the original URLs in `sources.md`.
