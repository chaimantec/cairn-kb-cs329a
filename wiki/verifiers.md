# Verifiers

A **verifier** checks whether a model's output is correct. In CS329A verifiers are the hinge
between generating many candidate answers and actually getting a better system: repeated sampling
only helps if something can pick the right sample, and self-improvement only works where there is
reliable feedback to learn from. The course gives verification a lecture of its own; lecture 1
introduces the idea from several directions.

## What a verifier does

In repeated sampling, the model produces many responses and a verifier or selection mechanism picks
a correct one as the system's output ([lecture 1](01-course-overview.md), ≈20:57). In an agentic
workflow, a verifier is a component that "can actually verify the output" (≈46:46).

The canonical example is **unit tests** for code: run each generated program against the tests and
keep the ones that pass (≈20:57–21:44). Software developers already write unit tests to know code is
correct, and verifiers in agent workflows do the same (≈46:46). This works best in **verifiable
domains** — math, code and other rule-based domains — where checking gives feedback the model can use
to correct its steps (≈46:46–47:33).

Lecture 1 distinguishes verifiers from the looser **critic** or **judge** — "effectively LLM as a
judge" — which gives feedback from a model rather than from the real world, such as a user or an
actual test run (≈45:14, ≈46:46). See [agentic workflows](agentic-workflows.md).

## When there is no verifier

Asked how repeated sampling works without a verifiable domain, Mirhoseini answers that it is much
harder, and points to research on training **LLMs as judges**, **LLM reward functions**, and **LLMs
with tools** (≈26:29–27:14). Reward models learned from human preferences, as in
[RLHF](llm-training-pipeline.md), play a related role in training. The course will also cover
**outcome reward models** and **process reward models** as feedback signals (≈39:01).

## The generator–verifier gap

Chowdhery names the core problem: it is easy for models to generate large amounts of content,
sensible or not, but deciding whether it is useful requires a feedback loop (≈50:38). In creative
writing little automatic feedback exists, so **human feedback becomes the bottleneck**; in domains
with good feedback, models can keep improving. "A lot of robust verification is hard", and
verification "continues to be one of the bottlenecks in this space" (≈50:38).

Models can also **generate their own verifiers**. Coding agents can write the tests they need to
pass (≈52:59), and as models improve those tests become more reliable, which drives a
self-improvement loop (≈49:05). CodeMonkeys generates unit tests for model-written code and checks
whether they make things better (≈49:51). See [self-improvement](self-improvement.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): verifiers in repeated sampling, the
  no-verifier question, verifiers vs judges in workflows, and the generator–verifier gap.
- Catalog lecture 3, *Robust Verification*, is the dedicated lecture; it is not yet in this KB.
  Lecture 1 says Mirhoseini will cover a paper from her lab on combining verifiers (≈50:38); the site
  lists [Shrinking the Generation-Verification Gap with Weak Verifiers](https://arxiv.org/abs/2506.18203)
  among that lecture's readings — see [sources](../sources.md).
