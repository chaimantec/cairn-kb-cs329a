# Self-improvement

**Self-improvement** is the course's central idea: an AI system that gets better by using its own
outputs — its samples, its reasoning, its tests, its interactions with an environment — as the signal
for further learning, rather than relying only on new human-written data. Lecture 1 introduces it as
the point where test-time scaling and training feed each other.

## The loop

Test-time scaling produces far more candidate answers than a model gives in one shot. That makes the
model **a data generator** ([lecture 1](01-course-overview.md), ≈28:53): for math problems where the
answer is known, sample many solutions that reach the golden answer; for coding problems, generate
large amounts of good solutions. That synthetic data goes into the training set and the model is
fine-tuned on it. The lecture credits this combination — fine-tuning plus test-time scaling — as a
core innovation behind DeepSeek (December 2024), the o1 series and Gemini Thinking (≈28:06–28:53).

Bringing test-time scaling "back to the process of training the model or fine tuning the model to
become better is very exciting. And that's the self-improving piece" (≈29:40). It is open-ended:
"there's no boundary in how good the models can become" (≈29:40).

## What it depends on: feedback

Self-improvement is only as good as the signal that says which outputs were right.

- **Verifiable domains** work best. In code, a model can generate unit tests; as models get better
  the tests get more reliable, and "there's this self-improvement loop that kicks in" (≈49:05).
  CodeMonkeys, covered in the previous offering, generates unit tests for model-written code and
  checks whether they improve results (≈49:51).
- **The generator–verifier gap** limits it elsewhere. Generating content is easy; knowing whether it
  is useful needs feedback. In creative writing there is little, so human feedback becomes the
  bottleneck, while "in domains where you can have good feedback, that's where it's possible to
  continue to improve the model" (≈50:38). See [verifiers](verifiers.md).

## Self-correction inside a single task

The same idea operates within one episode. [Reasoning models](reasoning-models.md) try something,
check it (tests, a calculator, a judgment), and correct or backtrack (≈32:00). For agents,
self-improvement means that "when it makes mistakes, it needs to be able to correct itself" — one of
the three capabilities, with planning and multi-step reasoning, that the course is organised around
(≈47:33, ≈59:58).

## What is not understood

A student asks why RL yields such a large jump if the underlying ability should already be in the
pre-trained model. Chowdhery says this is an active research question with no consensus on whether RL
or pre-training's diverse data does the work, and both help (≈51:24). If repeated sampling of the
pre-trained model already finds a correct answer, feedback should raise pass@1 without really
improving the model — yet "that whole loop is not completely well understood", and there are signs
that continuing RL keeps improving models (≈52:12).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the test-time-to-training loop, test
  generation in coding, the generator–verifier gap, and the open question about RL.
