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

## A model-labelled reward

[Lecture 3](03-robust-verification.md) shows the loop applied to the verifier itself. Math-Shepherd
labels each solution step automatically, by sampling continuations from it and checking whether they
reach the known answer; trains a process reward model on those labels; and uses that model both to
rerank the generator's answers at test time and as the reward for PPO fine-tuning of the generator.
The lecturer calls it "multiple levels" of the model generating data and reward signal for itself
(≈46:50), while noting that the gains seem to plateau (≈47:38). Its labels are noisy in predictable
ways, and a generator optimised against a model-labelled reward can learn to satisfy the reward
rather than reason (≈40:34–42:54, ≈35:54–36:41). See [verifiers](verifiers.md).

## What is not understood

A student asks why RL yields such a large jump if the underlying ability should already be in the
pre-trained model. Chowdhery says this is an active research question with no consensus on whether RL
or pre-training's diverse data does the work, and both help (≈51:24). If repeated sampling of the
pre-trained model already finds a correct answer, feedback should raise pass@1 without really
improving the model — yet "that whole loop is not completely well understood", and there are signs
that continuing RL keeps improving models (≈52:12).

## Where the feedback comes from

[Lecture 4](04-learning-from-feedback-with-tools-code.md) presents three ways for a model to improve
itself and sorts them by the source of the feedback (≈0:50–1:36):

- **The environment.** ReAct grounds a model's reasoning in observations from tool calls. Fine-tuning
  small PaLM models on 3,000 of ReAct's own trajectories with correct answers made them beat much
  larger prompted models (Yao et al. 2023, §3.3, Figure 3).
- **Execution.** RLEF runs generated code against public tests during generation and rewards the final
  solution on private tests, training the model with PPO. The lecturer's lesson is that "the
  self-improvement loop works", at least on simple enough problems with a binary reward (≈39:43);
  whether harder problems need richer feedback, such as error traces, is left open (≈38:09).
- **Principles.** Constitutional AI has a model critique and revise its own responses, and label its
  own preference data, against a human-written constitution; humans write only the principles
  (≈47:36). In the paper the harmlessness labels come entirely from AI while the helpfulness labels are
  still human (Bai et al. 2022, §1.2).

The recap puts the common idea plainly: when a feedback loop on top of a model has enough signal, the
model can improve beyond the data it was trained on (≈1:02:21–1:03:06). Its limits appear in the same
lecture. Self-critique can be hard, and a consensus of other models sometimes critiques better, because
a model can be overconfident and not know what it knows (≈59:16–1:00:01). And holding a model to fixed
principles trades some helpfulness for harmlessness (≈50:46–51:32).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the test-time-to-training loop, test
  generation in coding, the generator–verifier gap, and the open question about RL.
- [Lecture 3 — Robust Verification](03-robust-verification.md): Math-Shepherd's automatically
  labelled process reward model, used for verification and for PPO on the generator.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md):
  feedback from the environment (ReAct), from code execution (RLEF) and from a constitution
  (Constitutional AI).
