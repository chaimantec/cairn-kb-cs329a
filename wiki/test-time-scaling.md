# Test-time scaling

**Test-time scaling** (the course also says *inference scaling* and *test-time compute scaling*)
means getting more capability out of a model by spending more computation **when it answers**,
rather than by training it further. The model's parameters are fixed; what changes is how you
generate from it and how you choose among what it generates. CS329A treats this as the second axis
of scaling alongside model size, and as the raw material for self-improvement.

## Repeated sampling

The simplest form, introduced in [lecture 1](01-course-overview.md) through Mirhoseini's lab's
*Large Language Monkeys* work, is **repeated sampling**: ask the model the same problem many
times, independently and in parallel, and use a **verifier** or selection mechanism to output one
correct response (≈20:57). For code the verifier can be a set of unit tests (≈21:44). It works
because generation is stochastic, and the **temperature** setting controls how varied the samples
are (≈21:44).

The name comes from the infinite monkey theorem — a monkey typing forever would eventually produce
Shakespeare — with the LLM as the monkey (≈20:08).

### Coverage, pass@1 and pass@k

Two measurements recur, and it matters which one a result reports.

- **Coverage** is the fraction of problems for which **at least one** of the samples is correct. It
  is what a system with a perfect verifier could achieve. Lecture 1 treats **pass@k** as the same
  kind of number (≈36:41).
- **pass@1** is the accuracy of a single answer.

In Large Language Monkeys, raising the samples per problem from 1 to 10,000 on math and coding
benchmarks let smaller open models, worse than GPT-4o with one sample, overtake it on coverage in
every case shown (≈22:34); for some problems only three or four of the 10,000 samples were right
(≈24:09). The lecture's reading is that models "already know a whole lot more than what you get out
of them when you just ask them once" (≈23:21). Chowdhery's framing connects the two metrics: a base
model already produces some correct reasoning chains among many but cannot tell which, so much of
train-time and test-time scaling "comes down to it learns which is correct" — raising pass@1 toward
what coverage showed was reachable (≈36:41).

### Practical limits raised in lecture 1

- **Latency vs cost.** Parallel samples run in parallel, so latency is less of a concern than the
  compute cost, and the trade-off depends on the problem (≈26:29).
- **Temperature.** Too high produces gibberish; "usually if you go beyond 1.2 or so, it's not
  great", though other techniques can increase diversity (≈27:14–28:06).
- **No verifier.** Without one, selection is much harder; the options are LLM judges, LLM reward
  functions and LLMs with tools (≈26:29). See [verifiers](verifiers.md).
- **Adaptive budgets.** Whether the number of samples can depend on difficulty is flagged as an
  interesting direction; follow-up work uses a reward model to steer more sampling to unsolved
  problems (≈39:48–40:36).

## Scaling the length of thought

A second form scales how long a single model thinks. OpenAI's o1 release showed **pass@1** accuracy
on the AIME math benchmark rising log-linearly with test-time compute — the kind of curve previously
shown only for training compute (≈30:27–31:14). See [reasoning models](reasoning-models.md).

## Feeding it back into training

Test-time scaling is also a data engine. Sampling many solutions to math problems with known
answers, or many solutions to coding problems, yields high-quality synthetic data to fine-tune on —
the combination the lecture credits for DeepSeek and the o1-series and Gemini Thinking models, and
"the self-improving piece" (≈28:53–29:40). See [self-improvement](self-improvement.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): repeated sampling, coverage, o1's
  log-linear pass@1 curve, and the questions about latency, temperature and verifiers.
- Catalog lecture 2, *Test-Time Compute Scaling*, is the dedicated lecture; it is not yet in this
  KB. The course site lists four readings for it, starting with
  [Large Language Monkeys (Brown et al. 2024)](https://arxiv.org/abs/2407.21787) — see
  [sources](../sources.md).
