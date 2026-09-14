# Chain of thought

**Chain of thought** is the behaviour of a language model working through intermediate reasoning
steps before giving an answer. It began as a prompting technique, was recognised as a capability
that appears only in large models, and became the foundation of today's trained
[reasoning models](reasoning-models.md).

## As a prompting technique

In ordinary one-shot prompting, the model sees one solved problem and is then asked a similar one.
Chain-of-thought prompting puts the **reasoning** into the example, not just the answer
([lecture 1](01-course-overview.md), ≈7:01–7:47). Mirhoseini's example:

> *Roger has five tennis balls. He buys two more cans of tennis balls. Each can has three tennis
> balls. How many tennis balls does he have now?*

Instead of answering "11", the example says Roger started with five balls, two cans of three tennis
balls is 6, and 5 plus 6 is 11. Having seen that process, the model can apply it to new problems
(≈7:47–8:34). Any 1B-parameter model can solve that particular problem now without help, but the
chain-of-thought property "is holding to this day" and is "very, very important" for reasoning and
thinking models (≈8:34).

## As an emergent ability

Chain of thought only helps above a certain scale. On a math-dataset chart comparing LaMDA, GPT and
PaLM, the small models — around 8 billion parameters for LaMDA and around 7 billion for GPT — "can't
really benefit from chain of thought", while larger models use the reasoning in context to solve
problems better (≈9:20). *The captions garble the PaLM detail in that sentence.* The lecture groups
it with other abilities that appear suddenly at a certain size, such as modular arithmetic and word
unscrambling (≈10:06). See [scaling laws](scaling-laws.md).

Asked whether chain of thought was designed in or discovered, the answer is that it was **not baked
in by design** but discovered by giving models hard problems; the lecturer names GSM8K as the first
work that "showed signs of life", and PaLM as where it became clear this was a big deal — including
explaining jokes (≈58:25). The web-scale data models were trained on does contain methodical,
systematic writing (≈58:25).

## As training data

Chain of thought also moved into post-training. **Chain-of-thought fine-tuning** is a form of
instruction tuning whose examples walk through the process before the label (≈15:30), and chain of
thought in instruction-tuning data "shows the model some ways of how to think" (≈39:01). See
[the LLM training pipeline](llm-training-pipeline.md).

## Chain of thought vs reasoning models

Lecture 1 draws the line clearly. Chain of thought was originally emergent; reasoning models are
**trained to be thinking**, so "the entire reasoning is not an emergent behavior" (≈59:11). And where
chain-of-thought prompting supplies the reasoning in the example, a reasoning model "itself is
producing this chain of thought" (≈33:34). A student question — whether the gain comes from
generating the reasoning out loud or from being asked to decompose at all — is answered in
[reasoning models](reasoning-models.md).

## Grounding chain of thought

[Lecture 4](04-learning-from-feedback-with-tools-code.md) points out what chain of thought lacks: its
steps come from the model's internal state and get no feedback from the outside world (lecture 4,
≈3:54). ReAct interleaves the reasoning with tool calls. On HotpotQA the paper's hand analysis
attributes 56% of chain of thought's failures to hallucination and none of ReAct's; but ReAct's rigid
thought–action structure causes more reasoning errors, 47% of its failures against 16%, and chain of
thought still scores higher on HotpotQA. The best prompting methods combine the two, backing off from
one to the other (Yao et al. 2023, §3.3, Tables 1 and 2; lecture 4, ≈18:41–19:28).

Chain of thought also appears in Constitutional AI, where a feedback model reasons step by step before
judging which of two responses is more harmless. This improves its judgements, but it commits almost
fully to one answer, so its probabilities are clamped to 40–60% before use as labels (Bai et al. 2022,
§2, §4.1).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the tennis-ball example, emergence with
  scale, and the emergent-or-trained question.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md):
  ungrounded reasoning, ReAct's comparison with chain of thought, and chain of thought in a feedback
  model.
