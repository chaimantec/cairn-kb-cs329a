# The LLM training pipeline

CS329A's picture of how a chat model is built has four stages: **pre-training**, **fine-tuning on
high-quality data**, **instruction tuning**, and **reinforcement learning from human feedback
(RLHF)**. Lecture 1 presents these as what turned GPT-3 into ChatGPT, and as the baseline the rest
of the course builds on. The stages after pre-training are collectively the *fine-tuning* step in
the lecture's wording.

## Why it mattered

ChatGPT launched in November 2022 and reached one million users in five days ([lecture 1](01-course-overview.md),
≈10:52). What made it leap past GPT-3 was not just more parameters but "instruction tuning and the
reinforcement learning from human feedback" (≈11:39). Pre-training followed by these fine-tuning
steps "were the core components that made up a model like ChatGPT" (≈18:34).

## 1. Pre-training

Train the model to predict the next token over all kinds of text and data. It is described as "the
easiest step" (≈11:39). The pre-trained model has seen the internet and books but "has no sense of
what is right and wrong", statistically knows about the world, and does not know how to follow
instructions (≈12:25). How well pre-training scales is the subject of [scaling laws](scaling-laws.md).

## Alignment: the goal of what follows

The later stages steer models toward the goals, preferences and values of humans. This is "still a
big problem" that has not been mastered (≈13:11). Charts in the lecture show a pre-trained base model
fine-tuned for qualities such as **sensibleness** and **safety** using curated, high-quality data
showing what is safe and unsafe, or sensible and not (≈13:11–13:56).

## 2. Fine-tuning on high-quality data

The same next-token objective as pre-training, applied to much higher-quality data — books, creative
essays — that companies may pay millions or hundreds of millions of dollars to acquire. The model
"becomes much better as a result" (≈13:56–14:44).

## 3. Instruction tuning

Train on **instruction and question–answer pairs**, so the model learns to follow questions and
answer them. An example item: *Please answer the following question: what is the boiling point of
nitrogen?* paired with its answer (≈15:30). A variant, **chain-of-thought fine-tuning**, includes the
worked process before the label (≈15:30); see [chain of thought](chain-of-thought.md).

The data has traditionally been a mix of human-generated data, templates and synthetic data
(≈14:44; *the captions garble part of this list*). A lot of effort goes into it, and its quality and
generality strongly shape the resulting model (≈16:16). After this stage the model behaves much more
like today's assistants: you can ask questions, go back and forth, and it walks through a process to
an answer (≈16:16).

## 4. RLHF

**Reinforcement learning from human feedback** differs from the previous steps in how the objective
and the data are made (≈17:01). Rather than supervised prompt–label pairs:

1. Companies pay humans — sometimes experts — to rate model-generated answers, e.g. which is correct
   and which is not (≈17:01).
2. Those preferences train a **reward model** that stands in for the human (≈17:47).
3. The reward model then guides the LLM's parameters toward generations it judges good (≈17:47).

Rewards can target different qualities — **correctness, helpfulness, specificity, harmlessness** —
and are weighted according to what the model's builders care about most (≈17:47–18:34). The same
idea of a learned judge reappears in [verifiers](verifiers.md) and agentic
[evaluators](agentic-workflows.md).

## RLAIF: AI feedback in place of human labels

[Lecture 4](04-learning-from-feedback-with-tools-code.md) returns to RLHF to show its limit: humans rank
pairs of outputs, a reward model is built from their preferences, and the model hill-climbs on it — but
collecting tens of thousands of human labels is extremely time-consuming (lecture 4, ≈46:50–47:36).
**Constitutional AI** replaces the human harmlessness labels with AI feedback guided by a short list of
human-written principles. A supervised stage fine-tunes on the model's own critiqued and revised
responses; an RL stage trains a preference model on AI judgements of which response is less harmful,
and trains the policy against it (≈48:22–49:09; Bai et al. 2022, §1.2). The paper calls this RL from AI
feedback (**RLAIF**); its preference model still uses human labels for helpfulness (§1.2). The lecturer
also notes that post-training takes a much smaller share of compute than pre-training — maybe 5% — and
is repeated fairly often (≈52:18).

## What came next

Pre-training and fine-tuning "were the big pieces" until about a year and a half before the lecture,
when inference became a frontier too (≈19:21), and test-time generation began feeding synthetic data
back into fine-tuning (≈28:53). See [test-time scaling](test-time-scaling.md) and
[self-improvement](self-improvement.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the full pipeline, ≈11:39–19:21.
