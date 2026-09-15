# Verifiers

A **verifier** checks whether a model's output is correct. In CS329A verifiers are the hinge
between generating many candidate answers and actually getting a better system: repeated sampling
only helps if something can pick the right sample, and self-improvement only works where there is
reliable feedback to learn from. [Lecture 3](03-robust-verification.md) is verification's own
lecture; lectures 1 and 2 introduce the idea from several directions.

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

## Training a verifier

The first learned verifier the course studies in detail is Cobbe et al.'s (2021), in
[lecture 3](03-robust-verification.md): a language model with a small scalar head that outputs the
probability a solution is correct. It is trained on the generator's own samples — 100 per training
problem, each labelled by whether it reaches the known final answer — with the ordinary
language-modelling loss alongside the correctness loss (lecture 3, ≈3:13–7:03). At test time it ranks
many samples and the top one is returned (≈4:45). Three findings from that paper recur:

- Verification beats plain fine-tuning once the training set is large enough (≈10:58–11:44; Cobbe et
  al., §4.2, Figure 5).
- A large generator with a small verifier does better than a small generator with a large verifier
  (≈12:31–13:18; §4.3, Figure 6c).
- Sampling more helps only up to a point — about 400 completions there — after which the verifier's
  precision falls and it is fooled more often than helped (≈14:05–17:58; §5.1).

A separate verifier also leaves the generator general: instead of fine-tuning the base model onto one
dataset, the verifier guides it (≈19:33–21:06).

## Outcome and process reward models

An **outcome reward model (ORM)** scores a final answer; a **process reward model (PRM)** scores each
step of a solution — per step, not per token (lecture 2, ≈28:27–33:13). A PRM makes search possible:
beam search keeps the top-scoring partial solutions at each step (Snell et al. 2024, Figure 2). PRMs are
fine-tuned from language models and work best in-domain, with some generalization (lecture 2, ≈31:36).

Lecture 3 gives the comparison its evidence. Lightman et al. (2023) trained both kinds from GPT-4 on
MATH, the PRM on human step labels (PRM800K, 800,000 of them), and the PRM beat both the ORM and
majority voting, by a margin that widened as more solutions were sampled (lecture 3, ≈21:51–29:39;
Lightman et al., §3). The case for process supervision is **false positives**: a model can reach a
correct answer through wrong steps, which an outcome label rewards and step labels catch (≈25:00). A
PRM scores a whole solution by combining its step scores — Lightman et al. multiply the step
probabilities (§2.6), while Math-Shepherd takes the minimum (Wang et al., §3.4). The costs are labels,
since a PRM needs one per step, and a score threshold to tune; newer systems often combine PRM and ORM
signals (≈31:10–32:46).

## Step labels without humans

Math-Shepherd (Wang et al. 2023) labels steps automatically. From each step it samples $N$
continuations to a final answer; the **hard estimate** calls the step good if any continuation reaches
the known answer, and the **soft estimate** scores it by the fraction that do (lecture 3, ≈39:00–40:34).
This trades human cost for noise: a valid but unusual path can score 0 when $N$ is small, hard
problems give almost no signal, and a wrong step can still be labelled good if continuations from it
reach the right answer (≈40:34–42:54). The resulting PRM beat one trained on PRM800K on MATH and
served as the reward for step-by-step PPO (≈44:28–46:50). Once labels no longer come from humans, a
generator trained against a PRM may learn to please it instead of reasoning — the caveat the class
raises (≈35:54–36:41).

## Ensembles of weak verifiers

No verifier is perfect. Weaver, the lecturer's group's work in lecture 3, combines many — reward models
and LLM judges — filtering out the weakest, estimating each one's accuracy with weak supervision from
very few labels, and weighting their scores accordingly (lecture 3, ≈52:18–57:02). It scales
verification by adding verifiers rather than sampling one verifier more, and the ensemble can be
distilled into a model of about 400 million parameters (≈1:04:07–1:04:53). See
[lecture 3](03-robust-verification.md) for the method and results.

## Tests inside a training loop

In [lecture 4](04-learning-from-feedback-with-tools-code.md), unit tests are both the feedback and the
reward. RLEF splits a problem's tests in two: **public tests** give execution feedback while the model
iterates, and **private tests**, hidden from it, decide the reward for its final solution (lecture 4,
≈29:36–31:55). The split stops the model from copying expected outputs it saw in feedback into later
answers, and keeps iteration cheap (Gehring et al. 2025, §2.1). The paper's own limitation is the one
every test-based verifier shares: it needs test cases, which may not exist, and it suggests pairing the
method with automatic unit-test generation (§5).

## AI feedback as a judge

Constitutional AI uses a model as the judge for harmlessness. A **feedback model** is shown two responses
and a principle as a multiple-choice question, and the probabilities it gives each option become the
labels a preference model is trained on (Bai et al. 2022, §4.1). Asked how to know such feedback is
accurate, the lecturer says to hold out a validation set and check the preference model's scores for
consistency with humans (lecture 4, ≈57:41). The paper reports the feedback model's labels as reasonably
well calibrated, and chain-of-thought reasoning as significantly improving models' judgements on 438
comparisons of helpful, honest and harmless responses (§4.3, Figure 9; §2, Figure 4). See
[the LLM training pipeline](llm-training-pipeline.md).

## LLM judges inside search and RL

[Lecture 5](05-planning-and-multi-step-reasoning.md) uses a model as a judge in two more places. **LATS**
scores each state in its search tree by prompting the model to rate how promising it is, and adds a
self-consistency score for actions that are sampled often. The paper combines the two as
$V(s) = \lambda \cdot \text{LM}(s) + (1 - \lambda) \cdot \text{SC}(s)$, computed after the
environment's feedback (lecture 5, ≈10:16–11:04; Zhou et al. 2024, §4.2). The lecture contrasts it with
Math-Shepherd's verifier-guided search: in LATS the scores come from the outcomes of actions (≈5:37). Its
HotPotQA experiments also use an oracle that tells the agent whether a submitted answer is correct
(§5.1). **SWiRL** uses judges as **process rewards**. For filtering, a model decides whether each step is
reasonable given the steps before it; during RL, a generative reward model scores each action
(≈56:25–1:02:44; Goldie et al. 2025, §2.1, §2.2). The judge scores a tool call by its query, not by the
tool's output, which it never sees during training (≈1:01:55–1:02:44). Neither judge was trained; both
were prompted (≈1:01:08; Zhou et al., §4.2).

## The known answer as a reward

[Lecture 6](06-train-time-scaling-scaling-rl.md) returns to the simplest verifier, a known final answer. **STaR** uses
it as a filter: rationales that reach the correct answer are kept for training, on the assumption that a correct answer
means sound reasoning — which lets invalid intermediate steps through (lecture 6, ≈19:29–20:16, ≈32:46–33:35).
**DAPO** uses it as the whole reward — 1 if the predicted answer is equivalent to the ground truth, −1 otherwise —
instead of a learned reward model, to avoid reward hacking, and rewrites its training problems so that every answer is
an integer such a rule can check (Yu et al. 2025, §2.4, §3.5). **DeepSeekMath**, by contrast, trains reward models, and
finds process supervision better than outcome supervision inside RL (Shao et al. 2024, §5.2.1).

The lecture frames the tradeoff for any reward used in RL: a model that is too capable hacks the reward, and a reward
with too little signal cannot drive the loop. Math and code have final answers, execution feedback and unit tests; how
many domains have signals that good, and whether an ensemble of verifiers can cover one verifier's gaps, are open
questions (≈1:04:40–1:06:11). See [reinforcement learning](reinforcement-learning.md).

## Example tests, behaviour and a learned scorer

[Lecture 7](07-self-improvement-and-deep-research-agents.md) chooses 10 submissions from up to a million
programs with three cheap checks.

- **Example tests.** The tests printed in the problem statement act as a filter that a correct program cannot
  fail. Fewer than 1% of AlphaCode's samples pass them, so filtering removes more than 99% (Li et al. 2022,
  §5.3.5, Table 9).
- **Behaviour on generated inputs.** Where no further verifier exists, a separate model writes new test
  inputs from the problem description, and programs that give identical outputs are grouped. The reasoning is
  that "there are many ways solutions can be incorrect while correct solutions tend to behave the same". The
  generated inputs need not even be valid to be useful for grouping (§4.6).
- **A learned scoring model.** AlphaCode 2 fine-tunes a Gemini Pro model to give each sample an estimated
  correctness between 0 and 1, and submits the best-scoring sample from each of the 10 largest clusters
  (AlphaCode Team 2023, *Scoring Model*). The lecture calls it a reward model — "a learned approximation of
  what should be given high score and what should be given low score" — in contrast to clustering's heuristic
  (≈25:52). It also argues that the scorer's training data should differ from the policies' to avoid
  contamination (≈35:23–36:10).

Selection still falls short. In AlphaCode, perfect selection stays well ahead of filtering plus clustering
(§5.3.5, Figure 8), and the AlphaCode 2 report says its system "relies heavily on being able to filter out
obviously bad code samples" (*Discussion and Conclusion*). See [test-time scaling](test-time-scaling.md).

## Judging open-ended work

Some outputs have no test to run. [Lecture 8](08-agentic-evaluations-and-long-horizon-tasks.md)'s benchmarks judge them
in two ways. **GDPval** uses human experts from the relevant occupation, who rank unlabeled deliverables side by side;
a comparison took over an hour on average. It also trains an experimental automated grader, which agrees with expert
graders 66% of the time, against 71% agreement between experts (Patwardhan et al. 2025, §2.5). **DeepScholar-Bench**
uses LLM judges throughout: a pairwise preference against the human-written section, evaluated twice with positions
swapped to avoid position bias; nugget extraction; graded relevance; and entailment between claims and cited sources.
It validates them against over 200 expert annotations, with 70% to 82% agreement (Patel et al. 2025, v1, §3, §5.4,
Table 4). METR avoids judging altogether by scoring every task automatically, which the paper lists among the ways its
tasks differ from real work (Kwa et al. 2025, v2, §7.2.1).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): verifiers in repeated sampling, the
  no-verifier question, verifiers vs judges in workflows, and the generator–verifier gap.
- [Lecture 2 — Test-Time Compute Scaling](02-test-time-compute-scaling.md): verifiable domains, the
  measured generation–verification gap, ORMs and PRMs, and a preview of Weaver (≈24:34–25:22).
- [Lecture 3 — Robust Verification](03-robust-verification.md): the dedicated lecture — Cobbe et al.'s
  trained verifier and GSM8K, Lightman et al.'s outcome vs process supervision and PRM800K,
  Math-Shepherd's automatic step labels and PRM-driven RL, and Weaver's weakly supervised ensembles of
  verifiers and their distillation.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md): an
  LLM judge as LATS's value function and as SWiRL's step-wise process reward.
- [Lecture 6 — Train Time Scaling/Scaling RL](06-train-time-scaling-scaling-rl.md): the final answer as STaR's
  filter and DAPO's rule-based reward, and reward hacking versus too little signal.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md):
  AlphaCode's example-test filter and clustering by behaviour on generated inputs, and AlphaCode 2's learned
  scoring model.
