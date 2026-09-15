# Scaling laws

**Scaling laws** describe how a model's loss falls predictably as you invest more in it. In CS329A
they are the backdrop: the reason large language models got good, the trend that began to saturate,
and the model for the newer idea that *inference* can be scaled too
([test-time scaling](test-time-scaling.md)).

## The three axes

[Lecture 1](01-course-overview.md) describes three plots of scaling laws for the pre-trained base
model, each with test loss on the y-axis (≈2:23–3:09):

- **Compute** — the more compute put in, the lower the test loss.
- **Dataset size** — test loss falls further as the dataset grows.
- **Parameter count** — more layers or more parameters in the transformer lowers the loss.

Each lower loss "leads to a better model", and this was the foundation behind GPT-3 and everything
after it — ChatGPT, PaLM, Gemini (≈3:09). Scaling laws are **predictive**: we know how the loss will
go down as compute, data and parameters increase (≈6:15).

## Model size over time

From 2018 to 2024 model size rose almost consistently (≈3:55): BERT at 340 million parameters, GPT-2
at 1.5 billion, GPT-3 at 175 billion, PaLM at 540 billion, and GPT-4 estimated at trillions — an
exponential growth, on a chart the lecturer notes is a little outdated. The "large" in large language
models refers to this growth (≈4:42).

## What scale bought

Bigger models improved on natural-language and reasoning benchmarks, gained **few-shot learning** —
following a template from a handful of examples in the prompt, instead of needing domain fine-tuning
— and showed **emergent behaviour**: capabilities like reasoning that appear only in larger models
(≈4:42–5:29). Unlike the loss curves, emergent behaviours could not be predicted until the bigger
models existed (≈7:01). Examples in the lecture are [chain of thought](chain-of-thought.md) (≈9:20),
modular arithmetic and word unscrambling, each appearing "all of a sudden" at a certain size (≈10:06).
That is why frontier labs remain interested in pushing scale: for the steady progression and for more
emergent behaviours (≈10:06).

## Saturation, and the other axis

Scaling pre-training was "the foundation for a long time, until last year, where this was starting to
hit some kind of a saturation point" (≈3:55). Since then inference has become a frontier: OpenAI's o1
showed accuracy rising log-linearly with **test-time** compute — "previously, this has been shown for
training", now without changing parameter count (≈30:27–31:14). See
[test-time scaling](test-time-scaling.md).

## Inference scaling laws

[Lecture 2](02-test-time-compute-scaling.md) argues that inference has scaling laws too (≈4:00–7:10).
With repeated sampling, coverage $c$ grows with the number of samples $k$ as an exponentiated power law,
$c \approx \exp(a k^{b})$ (Brown et al. 2024, §3.1), fit across models from 70M to 70B parameters —
though less exactly than training laws. The power law arises because a benchmark contains a long tail
of very hard problems: Schaeffer et al. (2025, §3) prove that $-\log(\text{pass}_ {\mathcal{D}}@k)$ is a
power law in $k$ exactly when single-attempt success rates have a power-law density near zero. See
[test-time scaling](test-time-scaling.md).

[Lecture 7](07-self-improvement-and-deep-research-agents.md) shows a similar pattern surviving a submission
limit. AlphaCode's solve rate on CodeContests scales approximately log-linearly with the number of samples $k$,
both for $\text{pass@}k$ and when only 10 of the $k$ samples may be submitted, tapering off slightly in the
second case. Larger models have higher slopes, so "a better model with a higher slope can reach the same solve
rate with exponentially fewer samples" (Li et al. 2022, §5.3.1, Figure 6). The solve rate also scales
approximately log-linearly with training compute, and as sampling compute grows, so does the model size that
makes best use of it (§5.3.1, Figure 7). Asked why the relationship takes this shape, the lecturer points back
to lecture 2's readings (lecture 7, ≈21:15–22:03).

## Training compute in place of parameters

[Lecture 6](06-train-time-scaling-scaling-rl.md) adds a third kind of compute. One of its opening insights is that the
compute spent training a model on its own outputs "can substitute for the model parameters", as compute spent on more
data for smaller models already had (lecture 6, ≈3:11–3:56). Its motivating chart shows reasoning benchmarks no longer
tracking parameter count: 7B and 32B models trained this way reach high scores (≈1:39–2:25). DeepSeekMath makes a
related point about data: its 7B base model, pre-trained on curated math web data, outperforms Minerva 540B, a model 77
times larger, which the authors take as showing that "the number of parameters is not the only key factor in
mathematical reasoning capability" (Shao et al. 2024, §1.1, §2.3). See [reinforcement learning](reinforcement-learning.md).

## Trends in what agents can do

[Lecture 8](08-agentic-evaluations-and-long-horizon-tasks.md) tracks capability over time rather than loss. METR
regresses the logarithm of each frontier model's 50% time horizon on its release date, and finds the horizon "has
doubled every 212 days" since 2019, with a 95% confidence interval of 171–249 days. The 80% horizon doubles at a
similar rate from a much lower level (Kwa et al. 2025, v2, §4.2, §4.2.1). Extrapolated, the trend reaches a one-month
(167 working hours) horizon on software tasks between late 2028 and early 2031, if it continues and generalizes to real
tasks (§7.1, §8.3). GDPval's expert-judged win rate for OpenAI's frontier models has instead "increased roughly linearly
over time" (Patwardhan et al. 2025, Figure 6). The lecture uses the contrast as a caution: an exponential trend in
horizon length does not mean reliable work at those lengths (lecture 8, ≈36:05–37:40).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the three axes, model-size history, few-shot
  learning and emergence.
- [Lecture 2 — Test-Time Compute Scaling](02-test-time-compute-scaling.md): scaling laws for inference
  compute and why they take a power-law form.
- [Lecture 6 — Train Time Scaling/Scaling RL](06-train-time-scaling-scaling-rl.md): training compute on a model's own
  outputs as a substitute for parameters.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md):
  AlphaCode's log-linear solve rate in samples and compute, with steeper slopes for larger models.
