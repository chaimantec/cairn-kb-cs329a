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

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the LLM-to-agent transition, workflow
  building blocks and patterns, coding agents and applications.
