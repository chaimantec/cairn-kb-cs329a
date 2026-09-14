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

### Coverage, $\text{pass@}1$ and $\text{pass@}k$

Two measurements recur, and it matters which one a result reports.

- **Coverage** is the fraction of problems for which **at least one** of the samples is correct. It
  is what a system with a perfect verifier could achieve. Lecture 1 treats $\text{pass@}k$ as the same
  kind of number (≈36:41).
- $\text{pass@}1$ is the accuracy of a single answer.

In Large Language Monkeys, raising the samples per problem from 1 to 10,000 on math and coding
benchmarks let smaller open models, worse than GPT-4o with one sample, overtake it on coverage in
every case shown (≈22:34); for some problems only three or four of the 10,000 samples were right
(≈24:09). The lecture's reading is that models "already know a whole lot more than what you get out
of them when you just ask them once" (≈23:21). Chowdhery's framing connects the two metrics: a base
model already produces some correct reasoning chains among many but cannot tell which, so much of
train-time and test-time scaling "comes down to it learns which is correct" — raising $\text{pass@}1$ toward
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

A second form scales how long a single model thinks. OpenAI's o1 release showed $\text{pass@}1$ accuracy
on the AIME math benchmark rising log-linearly with test-time compute — the kind of curve previously
shown only for training compute (≈30:27–31:14). See [reasoning models](reasoning-models.md).

## Feeding it back into training

Test-time scaling is also a data engine. Sampling many solutions to math problems with known
answers, or many solutions to coding problems, yields high-quality synthetic data to fine-tune on —
the combination the lecture credits for DeepSeek and the o1-series and Gemini Thinking models, and
"the self-improving piece" (≈28:53–29:40). See [self-improvement](self-improvement.md).

## An inference scaling law

[Lecture 2](02-test-time-compute-scaling.md) makes the coverage curve quantitative. In *Large Language
Monkeys*, coverage $c$ as a function of the number of samples $k$ is fit by an **exponentiated power
law**, $\log(c) \approx a k^{b}$ (lecture 2, ≈4:49–7:10; Brown et al. 2024, §3.1), across Llama 3,
Gemma and Pythia models from 70M to 70B parameters. A follow-up reading explains the shape: each
problem's success rate $\text{pass}_ i@k = 1 - (1 - \text{pass}_ i@1)^k$ improves exponentially in $k$,
and the aggregate follows a power law exactly when the distribution of single-attempt success rates
has a power-law tail of very hard problems (lecture 2, ≈7:58–11:08; Schaeffer et al. 2025, §3). Such a
law lets you predict how many samples a target coverage needs (lecture 2, ≈6:23).

## Parallel samples, sequential revisions and search

Repeated sampling is **parallel**. Snell et al. (2024) add two other ways to spend test-time compute
(lecture 2, ≈26:55–34:00): **sequential revisions**, where the model keeps revising its own attempt, and
**search against a process reward model**, such as beam search that keeps the highest-scoring partial
solutions at each step. Parallel sampling behaves like a global search over approaches, revisions like
local refinement (Snell et al., §6.2). See [verifiers](verifiers.md) for outcome and process reward
models.

## Selection sets the ceiling

$\text{Best-of-}N$ is only as good as whatever picks the answer. In [lecture 3](03-robust-verification.md),
Cobbe et al.'s trained verifier keeps improving accuracy up to about 400 samples per problem and then
declines, because with more candidates it is more often fooled by a wrong solution that looks right
(≈14:05–17:58; Cobbe et al. 2021, §5.1) — against lecture 2's majority voting, which stopped tracking
the correct answer well before that (lecture 3, ≈14:52–15:38). Process reward models push the limit
further (Lightman et al. 2023, §3). Weaver scales the selection side itself: instead of sampling one
verifier more, it adds verifiers to an ensemble, and the lecture lists more generations, larger
generator and verifier models, and more verifiers as separate axes along which inference compute can
be spent (lecture 3, ≈1:00:07).

## Allocating compute by difficulty

Which strategy is best depends on the question. Binning questions by the model's $\text{pass@}1$ and choosing
the best strategy per bin — the **compute-optimal** strategy — beats $\text{best-of-}N$ with up to $4\times$ less
test-time compute; easy questions do best with fully sequential revisions, harder ones with a balance
(lecture 2, ≈34:45–37:57; Snell et al., §3, §6.2). In a FLOPs-matched comparison, test-time compute on a
small model can beat a $\sim 14\times$ larger pretrained model on easy and medium questions or at low inference
load, but pretraining wins on the hardest questions (lecture 2, ≈37:57–41:53; Snell et al., §7).

## Inference-time architectures

*Archon* composes techniques — generators, fusers, critics, rankers, verifiers, unit-test generators and
evaluators — into layered architectures, and searches for one with Bayesian optimization under an
inference call budget (lecture 2, ≈45:03–1:01:29; Saad-Falcon et al. 2024, §3). Fusing several responses
into one was "surprisingly a very effective method", and adding layers helps, much as in deep networks
(lecture 2, ≈48:11, ≈58:24).

## Tree search over actions

[Lecture 5](05-planning-and-multi-step-reasoning.md) spends test-time compute on **search with feedback
from the environment**. LATS runs Monte Carlo tree search over an agent's actions, with UCT balancing
exploration and exploitation. The lecturer's point is that it turns more test-time compute into better
solutions on multi-step tasks (lecture 5, ≈17:14–18:01). On HotPotQA its exact match rises from 0.44 to
0.52 to 0.61 as the sampled trajectories go from 10 to 30 to 50 (Zhou et al. 2024, §5.4, Table 10). The
price is cost, since every expansion and backpropagation adds inference, and the lecture names it as the
method's main downside (≈19:35). The site's reading list for lecture 5 also includes *Wider or Deeper?*,
whose AB-MCTS decides at each node whether to sample new candidate responses or revisit existing ones
using feedback (Inoue et al. 2025, abstract); the recording does not discuss it.

The same lecture puts parallelism to a different use. **SPRINT** does not sample more answers: it trains a
reasoning model to run the independent parts of one reasoning trace in parallel. That cuts the number
of sequential tokens, by 39% on problems where the baseline needs more than 8,000 tokens, at matched or
better accuracy (≈25:04–26:38, ≈44:41; Biju et al. 2025, §4.2). See
[reasoning models](reasoning-models.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): repeated sampling, coverage, o1's
  log-linear $\text{pass@}1$ curve, and the questions about latency, temperature and verifiers.
- [Lecture 2 — Test-Time Compute Scaling](02-test-time-compute-scaling.md): the dedicated lecture —
  Large Language Monkeys and its inference scaling law, why the law is a power law, the
  generation–verification gap, Snell et al.'s revisions, PRM search and compute-optimal allocation, and
  Archon's inference-time architectures. Its four readings are transcribed in `raw/papers/`.
- [Lecture 3 — Robust Verification](03-robust-verification.md): $\text{best-of-}N$ with a trained verifier and
  where it stops helping, process reward models, and Weaver's scaling of verification by ensembling.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md): LATS's tree
  search with environment feedback and its cost, and SPRINT's parallel execution within one reasoning
  trace.
