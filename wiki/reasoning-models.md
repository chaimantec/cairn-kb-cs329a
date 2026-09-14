# Reasoning models

**Reasoning models** (also called *thinking models*) — o1, o3, DeepSeek, Gemini Thinking in the
course's examples — are language models trained to produce an extended chain of thought before
answering, and to spend more of it on harder problems. They are the point where
[test-time scaling](test-time-scaling.md) and training meet.

## What they do while thinking

For difficult problems, thinking models, like people, spend more time and consider many strategies
([lecture 1](01-course-overview.md), ≈31:14). Mirhoseini lists the behaviours involved (≈32:00):

- **Problem analysis** — understanding the problem first.
- **Task decomposition** — breaking a task into simpler, more addressable tasks.
- **Using feedback** — trying something and checking it, by running tests on code, using a
  calculator, or judging the answer, then improving. *The captions name this step "self-evolution
  strategies"; the edited transcript marks the term as unclear.*
- **Self-correction** — noticing an error mid-way and fixing it.
- **Alternative proposals** — backtracking to a different approach when something does not work.

Some of this was probably present in human-curated training data, but "a big part of it" is the
model acquiring these skills during fine-tuning and RL on synthetic data (≈32:46).

### The o1 transpose example

Asked to write a bash script that takes a matrix and outputs its transpose, o1 starts by working out
what the user is requesting and what the input and output formats are; decomposes the approach
(parse the input, build the matrix as an array of arrays); and self-corrects mid-way — the familiar
"wait, … there's something wrong" (≈32:46–34:20). The key difference from chain-of-thought prompting
is that the model "itself is producing this chain of thought" (≈33:34). See
[chain of thought](chain-of-thought.md).

## Where they help

Compared with GPT-4o, reasoning models such as o1 do better on math calculation, data analysis and
programming, but not necessarily on personal writing or editing text (≈34:20–35:06). OpenAI's o1
release showed $\text{pass@}1$ accuracy on the AIME math benchmark rising log-linearly with test-time compute
(≈30:27–31:14).

## Why they work — lecture 1's answers to students

- **Generating the reasoning, or being asked to decompose?** Training has made the model "a
  generalized thinker"; decomposition, backtracking and analysis are learned skills that generalize
  (≈35:53). Chowdhery's complementary view: a base model already produces some good reasoning chains
  among many but does not know which is correct, and train-time and test-time scaling largely teach it
  which is correct — raising $\text{pass@}1$, where repeated sampling raises $\text{pass@}k$ or coverage (≈36:41).
  Generating long thinking is expensive, but it leads to better answers (≈36:41–37:30).
- **A separate model for the reasoning?** Reasoning ability has grown with size, so you would use the
  larger model's traces, perhaps with a smaller model summarizing. And "at least currently" models
  **prefer their own traces**, even over traces from a better model (≈37:30–38:15).
- **How are they taught to reason?** No published work fully covers it; it is "a bit of both"
  templates and fine-tuning, starting from a base model that already had some thinking capability.
  Chain of thought in instruction-tuning data shows ways to think; **outcome reward models** and
  **process reward models** provide feedback (≈39:01). Models now generalize well beyond their
  training instructions (≈39:48).
- **Is reasoning emergent?** Chain of thought originally was; reasoning models are trained to reason,
  and are converging toward knowing when they need a lot of reasoning and when they do not (≈59:11).

## Reasoning is not yet agency

Reasoning models are still essentially single-turn: they answer, but do not accomplish multi-step
tasks in an environment (≈40:36). Agents need better planning, multi-step reasoning and
self-correction than reasoning alone delivered (≈47:33). See [agentic workflows](agentic-workflows.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): reasoning behaviours, the o1 example, o1 vs
  GPT-4o, and the Q&A on why reasoning models work.
