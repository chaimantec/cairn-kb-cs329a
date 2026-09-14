# Agents and agentic workflows

An **agent**, in CS329A's sense, is a system that pursues a goal over many steps: it plans, takes
actions in an environment, gets feedback, corrects itself, and decides when to stop. An **agentic
workflow** is the more common, more constrained version in practice — a graph of LLM calls, tools and
checks designed by hand. The course exists because making the first reliable requires planning,
multi-step reasoning and self-improvement that today's models only partly have.

## From chatbots to agents

LLMs used as chatbots or reasoning models are still essentially **single-turn**: fun to interact
with, but not accomplishing tasks for you ([lecture 1](01-course-overview.md), ≈40:36). In the months
before the lecture, agents like **Claude Code** and **Deep Research** began doing real workflows end
to end (≈41:22): researching where to rent a home for a year by reading many websites and summarizing
pros and cons, or — with Claude Code or OpenAI's Codex — modifying files and working out test cases
from English instructions (≈41:22–42:08).

What distinguishes an agent (≈42:08–42:57):

- It is given a **goal** and plans the steps toward it.
- It **interacts with an environment**, often through **tools** external to the model.
- It uses **feedback** to correct its steps.
- It decides **when to stop** — the goal is achieved, or it reports that it cannot be.
- It may need **memory** to keep track of the task.

## Agentic workflows today

Simpler tasks such as deep research can be done end to end, but most real systems are still **static
workflows** (≈42:57–43:43). Two cartoon examples from the lecture (≈43:43):

- A generator model produces a solution, and a second model judges it and decides whether to accept.
- An LLM is called on many inputs in parallel and the outputs are aggregated — the shape of deep
  research, whose output is a summary.

For open-ended problems it is easier to build by hand the graph of how a human would do the task, with
an **LLM evaluator** supplying feedback, than to run a fully open loop; coding and research are where
the open loop shows "signs of life" (≈44:28).

### Building blocks

LLM calls (an instruction or input in, an output out); **verifiers**; **critics or judges**,
"effectively LLM as a judge"; and **tool calls**, such as web search for deep research or a weather
lookup (≈44:28–45:14). See [verifiers](verifiers.md).

### Orchestration patterns

Lecture 1 lists these ways of wiring the blocks together (≈45:14–47:33):

- **Prompt chaining** — decompose the task into a chain of subtasks, as a reasoning model decomposes
  a problem.
- **Routing** — send complex inputs to a more complicated set of LLM calls and simpler ones to a
  simpler workflow.
- **Parallelization** — run several calls simultaneously, e.g. researching different keywords, then
  aggregate; or split into independent subtasks and combine the solutions.
- **Orchestrator** — a central "LLM manager" plans and makes the subsequent calls; Claude Code's plan
  is the example.
- **Evaluator / judge** — feedback from an LLM instead of from the real world (a user, or an actual
  unit-test run). The homeworks cover this.
- **Verifiers** — checks that genuinely verify an output, like unit tests, in verifiable domains such
  as math and code.

All of this demands that LLMs get better at **planning**, **multi-step reasoning** and
**self-improvement** — correcting their own mistakes — which reasoning models alone did not achieve,
and which the following lectures cover (≈47:33).

## Coding agents

A coding agent interacts with a computer, these days mostly through a terminal. Given an instruction
such as implementing a test, it navigates the repository, searches and views files, edits lines, runs
commands, and decides from the output what to do next (≈48:19). Even with a goal, it often has to
**clarify user intent**, because users under-specify problems, then find relevant files, act, and
verify — for example by passing tests, which it may write itself (≈52:59–53:45).

This loop "was not quite reliable last year". The architecture has not changed much; what changed is
**more powerful models and better RL** — RL with verifiable rewards and train-time scaling are working
— plus a self-improvement loop in which models generate increasingly reliable tests (≈48:19–49:05).
See [self-improvement](self-improvement.md).

## Applications named in lecture 1

- **Repetitive engineering** — code migrations, version upgrades, codebase restructuring, data
  extraction and cleanup, data-warehouse migrations, unit tests (≈53:45–54:33).
- **Customer support** — live transcription, Knowledge Assist (surfacing the relevant article),
  smart replies, call summaries; several companies target different segments, and end-to-end systems
  are starting (≈54:33–55:19).
- **Research reports** — identify references for a topic (the example is the 2022 Winter Olympics
  opening ceremony), outline, summarize each reference, and combine into a full-length article; one
  homework has students try it (≈55:19–56:53).
- **AI scientists** — idea generation, experiment iteration and paper write-up; useful for
  brainstorming beyond a researcher's usual range despite hallucinations (≈56:53–57:39).

## Reasoning and acting: ReAct

[Lecture 4](04-learning-from-feedback-with-tools-code.md) goes back to one of the first designs for a
language model that uses tools. **ReAct** prompts a model to interleave verbal reasoning with actions —
tool calls such as searching Wikipedia or navigating a shopping site — so that each observation feeds
the next thought (lecture 4, ≈4:40–7:47). Reasoning alone is ungrounded and hallucinates; acting alone
cannot reason about what it has seen. Interleaving the two gave more grounded and more interpretable
trajectories (≈19:28; Yao et al. 2023, §3.3). Formally, ReAct adds the space of language to the agent's
action space: a thought is an action that changes no part of the environment, only the context the
next step sees (Yao et al., §2).

Its costs are the ones any agent loop pays: tasks with large action spaces need more demonstrations than
fit in context, and every reasoning step adds inference cost (≈21:47). The lecturer notes that the
thinking modes of today's open-source models do this out of the box, having been distilled on such
traces (≈11:39).

The class discussion of ReAct lists what an agent loop may need beyond reasoning and acting: reflection
on noisy environment feedback, backtracking out of repetitive loops, confidence from repeated attempts,
task decomposition, parallel approaches, memory, and routing subtasks to the models best at them —
"almost like building a compound system" (≈22:34–27:18). Asked whether RL post-training will make
handcrafted frameworks like ReAct obsolete, the lecturer answers yes where the space to explore can be
defined, and no where the workflow is domain-specific, as for a finance or legal agent
(≈1:06:12–1:08:34).

## Coding agents that learn from their test runs

Lecture 4's second paper, **RLEF**, trains a code model with reinforcement learning on the loop a coding
agent runs: write code, run it against public tests, read the feedback, try again, and be rewarded by
hidden private tests (≈29:36; Gehring et al. 2025, §2.1). Before that training, iterating on feedback
did not beat sampling independently at the same budget (≈35:49; Gehring et al., §3.3). For code bases
too large for the context window, the class's ideas — searching for what is relevant, summaries, a graph
representation — are the kind of thing Claude Code does, and what SWE-bench targets (≈43:41–46:02). See
[self-improvement](self-improvement.md).

## Planning over many steps

[Lecture 5](05-planning-and-multi-step-reasoning.md) is about agents that plan over several steps, and
shows three designs. **LATS** turns ReAct's single trajectory into a tree search. At each state it samples
several actions and executes them in the environment. It scores the resulting states with an LLM judge
plus a self-consistency heuristic, chooses what to expand with UCT, and reflects on failed trajectories
(lecture 5, ≈7:57–16:27; Zhou et al. 2024, §4.2). It relies on being able to return to an earlier state,
which an action such as paying for a service does not allow (≈19:35; Zhou et al., §6). **SPRINT** trains a
reasoning model to act as a **planner** that writes independent subtasks, and as a pool of **executors**
that carry them out in parallel, round after round (≈25:52–26:38; Biju et al. 2025, §3.1). **SWiRL**
trains a model for multi-step tool use: when to call a tool, what query to write, and when to stop and
answer. It learns from its own offline trajectories, without calling tools during training
(≈53:10–1:01:08; Goldie et al. 2025, §2). The site's reading list for the lecture also includes ADaPT,
which decomposes a sub-task only when the executor cannot carry it out (Prasad et al. 2024, abstract);
the recording does not discuss it.

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the LLM-to-agent transition, workflow
  building blocks and patterns, coding agents and applications.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md):
  ReAct's reason–act loop, RLEF's execution-feedback loop for coding agents, and the class discussion
  of what else an agent loop needs.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md):
  LATS's tree search over actions, SPRINT's planner and parallel executors, and SWiRL's step-wise RL
  for multi-step tool use.
