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
pre-trained model already finds a correct answer, feedback should raise $\text{pass@}1$ without really
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

## Training on its own multi-step trajectories

[Lecture 5](05-planning-and-multi-step-reasoning.md) applies the loop to multi-step tool use. **SWiRL**
has a model generate its own multi-step trajectories offline, calling tools along the way. Another model
judges each step (Gemini 1.5 Pro in the paper), and the first is trained with step-wise RL on those
judgements, with no golden labels (lecture 5, ≈54:48–1:01:08; Goldie et al. 2025, §2). Two findings bear
on what a model can learn from its own data. It learned best from trajectories whose steps were judged
sound **whether or not** their final answers were correct; trajectories it already solved taught it less
(≈1:06:44–1:08:14; §4.2, Figure 4). And what it learned transferred: training on math with a calculator
improved question answering with search, and the reverse (≈1:08:14–1:09:03; Table 2). The fine-tuned
model even beats its own reward model on some out-of-distribution benchmarks, which the authors read as
more than distillation (§4.2, Figure 8).

**SPRINT** uses models to restructure training data rather than to judge it. GPT-4o splits a reasoning
model's traces into plans and executions, and another model works out which steps depend on which. The
result is data that teaches the model to reason in parallel (≈27:24–29:45; Biju et al. 2025, §3.2). The
lecturer offers this as a general pattern: to teach a model a behaviour, use LLMs to create the
fine-tuning data (≈27:24).

## Bootstrapping reasoning from its own answers

[Lecture 6](06-train-time-scaling-scaling-rl.md) opens with the loop in its barest form. **STaR** few-shot prompts a
model to write a rationale and an answer for many problems whose answers are known, keeps only the rationales that
reached the correct answer, fine-tunes on them, and repeats (lecture 6, ≈17:54). Problems the model cannot solve give
it no signal, so STaR adds **rationalization**: show the model the correct answer as a hint, ask it to explain its way
there, and train on that rationale as if it had been produced without the hint (≈18:42–19:29). On CommonsenseQA a 6B
model trained this way reached 72.5% using 86.7% of the training data, against 73.0% for a GPT-3 model 30 times larger
fine-tuned to answer directly (Zelikman et al. 2022, §4.4, Table 1).

The loop rests on assumptions that bound every method in this family: that a correct final answer means sound
reasoning, that the model can produce a valid rationale once it is given the answer, and that it is strong enough to
start (≈19:29–21:02). It learns only from successes: "learning from negative examples has not been nailed. Learning
from positive examples has been" (≈23:24).

## More consistent, not more capable

Lecture 6 also marks a limit on what these loops achieve. In DeepSeekMath's analysis, RL raised majority-vote accuracy,
$\text{Maj@}K$, but not $\text{Pass@}K$ — "the model actually became more consistent, not fundamentally smarter"
(≈52:19; Shao et al. 2024, §5.2.2, Figure 7). The lecturer extends this to all three of the lecture's methods: they
improve majority voting, formatting and coherence over multiple steps, and none yet improves fundamental capability or
generalizes far out of domain (≈1:03:08). RL gets better at what a model can already do, exploring "the design space of
what it knows" (≈1:10:06–1:10:52). This sharpens the open question from lecture 1 above; see
[reinforcement learning](reinforcement-learning.md).

## Search first, then build it in

[Lecture 7](07-self-improvement-and-deep-research-agents.md), titled *Self-Improvement and Deep Research
Agents*, improves results by **searching** a model's outputs rather than updating the model: "the solutions
lie in the search space of the models" (≈0:52). Its link to self-improvement runs through the class
discussion. The lecturer describes a line of research that first builds a system that reasons well and closes
the loop — like AlphaCode 2, with one family of models generating and another scoring — and then distils that
knowledge into a single model, which becomes a large reasoning model (≈34:37). Asked how to cut AlphaCode 2's
wasted samples, the lecturer offers refinement with feedback and a model improved with RL, each of which
reduces what test-time search has to do (≈32:18–33:51). Asked how to build reasoning into a code model, the
class reaches for chain-of-thought training data and a STaR-style loop (≈43:59). AlphaCode also generates part
of its own verification: a model trained to write new test inputs, whose outputs are used to cluster the
samples (Li et al. 2022, §4.6).

## The three things that stop it

[Lecture 9](09-future-research-areas.md), the closing lecture, organizes the open problems of
self-improvement into three (≈3:57–6:17): the loop is "still limited to narrow domains like math and
coding", so **diversity** of reasoning chains is what keeps it from stalling; **verification** is what
decides whether the loop can run at all outside those domains; and the **prompts** the loop trains on
are still "selected very statically and require humans to select them". Each of the lecture's three
papers is a direction on one of the three.

### Diversity: why one model's own outputs stop helping

The failure is easy to describe. Iterative fine-tuning generates solutions, filters out the wrong
ones by rejection sampling, and trains on the good ones — and variants like STaR add reasoning chains
to the process. But if a **single** model generates that data, "it will generate solutions that will
be very similar", and "the performance increase will stop after a few iterations or after tens of
iterations" (≈7:51). The lecture's explanation compares it with pre-training: the pre-training corpus
is diverse "because it was generated over such a long time by humans", and that diversity is part of
why it helps — whereas one model prompting itself "will not have very diverse responses even at high
temperatures" (≈7:51). This is the same ceiling the course saw in lecture 6, where RL raised majority
voting without raising $\text{pass@}k$ (see
[reinforcement learning](reinforcement-learning.md)).

The paper the lecture presents — described only as "coming from multi-agent finetuning" (≈6:17) —
answers with **multiple specialized agents**. **Generation agents** fine-tuned from the same base
model produce diverse initial answers; a summarization step runs across their answers; a **critic
agent** critiques that updated set and the critique is added to the input; the generation agents
produce updated answers using the summary of the others; and majority voting runs on top
(≈8:38–11:47). The loop can continue as a **debate**. Training data comes out of it two ways: the
generation models are fine-tuned on prompt–response pairs filtered for agreement with the **majority
vote**, and the critics are fine-tuned on trajectories where an answer is correct at the start and
corrected through the debate, so "the critic model is learning how to contrast the correct and the
incorrect answer" (≈10:13–11:47). Diversity therefore exists *before* the critique stage, which is
what "majority voting for free" means (≈10:13).

The lecture reads the results on two axes — negative log likelihood, "just a proxy for performance",
and embedding dissimilarity, higher meaning more diverse (≈12:35). Over math, across three
open-source models, the multi-agent version kept improving across fine-tuning iterations while
single-agent fine-tuning "collapses or doesn't continue to improve", and the responses "continue to
stay quite diverse"; the improvement also carried to an adjacent domain, GSM8K (≈13:22–14:08). The
takeaway: "if you want self-improvement, the reasoning chains that are provided to the model to drive
those need to be diverse in some way" (≈14:08).

### Verification: the loop needs a checker that has no answer key

Lecture 3's problem was picking the right answer; lecture 9's is verifying the **reasoning**, in
domains where the final answer is the only signal available. The lecture's paper — **DeepSeekMath-V2**
(≈14:53)— starts from the fact that reward based on matching the ground truth "enabled saturation of
multiple benchmarks", while "even when you have the correct answer, you might not have the correct
reasoning" (≈15:40). Theorem proving needs "rigorous step-by-step derivation, which the final output
doesn't quite give you" (≈15:40). See [verifiers](verifiers.md) for the meta-verifier this adds.

### Data: letting the model choose what to learn

The third bottleneck is where the training prompts come from. Human-curated reasoning traces and
expert-written question–answer pairs mean that "if you're constructing such a model in math, then you
need math experts. If it's an IMO problems, then you need IMO experts, or if it is [coding], then you
need strong software coders" — and "as the models continue to surpass human intelligence, the ability
to find more and more experts and more and more such tasks starts to be limiting" (≈23:25–24:12). In
the lecture's third paper, "a single model can both propose tasks and then solve them", so that no
external source of data is needed. See
[the LLM training pipeline](llm-training-pipeline.md) and
[reinforcement learning](reinforcement-learning.md) for how the tasks are chosen and validated.

### What the lecture says is still missing

The lecture's own summary of the three is that diversity "continues to be an open problem", that
verification should be broken "in a way where we are not bottlenecked by humans or tasks that are
verified only through human experts", and that "there's only so much data that can be curated by
humans for what prompts go in" (≈32:44–33:32). Its largest open direction is **continual learning**:
humans improve continuously as they solve problems, whereas models learn in "mostly this offline
process" — experiences are generated, and "maybe after some time, there's this fine-tuning process of
the model", which "is not something that happens on the go" (≈53:19–54:05). The lecture frames this
as the mismatch a concept like continual learning could address, and the
[training pipeline page](llm-training-pipeline.md) records the two alternatives the class discussed:
updating weights, or not touching them and extending the effective context instead.

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the test-time-to-training loop, test
  generation in coding, the generator–verifier gap, and the open question about RL.
- [Lecture 3 — Robust Verification](03-robust-verification.md): Math-Shepherd's automatically
  labelled process reward model, used for verification and for PPO on the generator.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md):
  feedback from the environment (ReAct), from code execution (RLEF) and from a constitution
  (Constitutional AI).
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md):
  SWiRL's step-wise RL on self-generated, model-judged trajectories, and SPRINT's LLM-restructured
  training data.
- [Lecture 6 — Train Time Scaling/Scaling RL](06-train-time-scaling-scaling-rl.md): STaR's bootstrapped rationales
  and rationalization, and RL making a model more consistent rather than more capable.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md):
  searching a model's outputs (AlphaCode, AlphaCode 2, Search-o1), model-generated test inputs, and distilling a
  search system into a reasoning model.
- [Lecture 9 — Future Research Areas](09-future-research-areas.md): the closing lecture, which names the three
  bottlenecks — diversity of reasoning chains (multi-agent fine-tuning), verification without a reference
  solution, and the human data bottleneck — and puts continual learning first among the open directions.
