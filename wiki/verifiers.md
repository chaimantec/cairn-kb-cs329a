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

## Verifiable domains

[Lecture 2](02-test-time-compute-scaling.md) lists where automatic verification is available (≈12:43–15:10):
formal proof checkers for some math, unit tests for code (which humans can often write more easily than
the program), "AI as a compiler" — checking generated CUDA against its PyTorch source by comparing outputs
— and translation between programming languages. Even these verifiers are imperfect: 11.3% of SWE-bench
Lite problems have flaky tests, and 35 of 122 CodeContests problems have reference solutions that fail
their own tests (Brown et al. 2024, §4.2).

## The generation–verification gap, measured

Lecture 2 names the gap between coverage — what a perfect selector could reach — and what practical
selectors reach the **generation verification gap** (≈17:30), the same problem lecture 1 calls the
generator–verifier gap. In *Large Language Monkeys*, majority voting and reward-model selection plateau
before about 100 samples while coverage keeps rising: on MATH with Llama-3-8B-Instruct, coverage goes
from 82.9% to 98.44% between 100 and 10,000 samples while the best selector moves from 40.50% to 41.41%
(Brown et al., §1, §4.1). Majority voting fails because the hardest problems' correct answers are rare
(lecture 2, ≈17:30–19:04).

## Outcome and process reward models

An **outcome reward model (ORM)** scores a final answer; a **process reward model (PRM)** scores each
step of a solution — per step, not per token (lecture 2, ≈28:27–33:13). A PRM makes search possible:
beam search keeps the top-scoring partial solutions at each step (Snell et al. 2024, Figure 2). PRMs are
fine-tuned from language models and work best in-domain, with some generalization (lecture 2, ≈31:36).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): verifiers in repeated sampling, the
  no-verifier question, verifiers vs judges in workflows, and the generator–verifier gap.
- [Lecture 2 — Test-Time Compute Scaling](02-test-time-compute-scaling.md): verifiable domains, the
  measured generation–verification gap, ORMs and PRMs, and the proposal to ensemble many weak verifiers.
- Catalog lecture 3, *Robust Verification*, is the dedicated lecture; it is not yet in this KB.
  Lecture 2 previews Weaver, a weakly supervised ensemble of verifiers from the lecturer's group
  (≈24:34–25:22); the site lists
  [Shrinking the Generation-Verification Gap with Weak Verifiers](https://arxiv.org/abs/2506.18203)
  among lecture 3's readings — see [sources](../sources.md).
