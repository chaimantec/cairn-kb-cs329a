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

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the three axes, model-size history, few-shot
  learning and emergence.
