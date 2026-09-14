# CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)

CS329A is a Stanford graduate seminar on AI agents that "continuously improve themselves through
interaction with themselves and the environment", taught by **Aakanksha Chowdhery** and **Azalia
Mirhoseini**. It starts from self-improvement techniques for language models — verifiers, scaling
test-time compute, search, train-time scaling with RL — moves to agents augmented with tools, code
and memory, then multi-step reasoning, planning and evaluation, with guest lectures from frontier
labs. Students read research papers for each lecture, do three homeworks, and carry out an original
research project.

> **Coverage note — partial.** This knowledge base covers **catalog lectures 1–6 of 9**
> (Course Overview; Test-Time Compute Scaling; Robust Verification; Learning from Feedback with
> Tools/Code; Planning and Multi-Step Reasoning; Train Time Scaling/Scaling RL). It says nothing about any later lecture. Do not cite it as covering the course.
>
> **No slides; papers are the course material.** The course publishes no slides on its public site
> (lecture materials go to Canvas). The course material is the **paper reading list** the site gives
> for each lecture — exactly that list. **Lecture 1 lists no readings** and is built from its
> transcript alone. **Lecture 2's four readings are transcribed in full text** in `raw/papers/`, from
> their arXiv LaTeX sources (CC BY 4.0) — Large Language Monkeys with its appendix, the other three main
> body only. **Of lecture 3's four readings only Weaver is CC BY 4.0**, and only its main body is
> transcribed; the other three (Cobbe et al. 2021, Lightman et al. 2023, Math-Shepherd) carry arXiv's
> non-exclusive licence, so they are linked, discussed and cited on the lecture page but not
> reproduced. **Lecture 4's three readings** (ReAct, RLEF, Constitutional AI) are all CC BY 4.0 and
> transcribed main body only. **Of lecture 5's five readings**, LATS and SWiRL are CC BY 4.0 and
> transcribed main body only; SPRINT (CC BY-NC-SA 4.0), ADaPT and *Wider or Deeper?* (arXiv
> non-exclusive) are linked only, and the recording discusses only LATS, SPRINT and SWiRL. **Lecture 6's three
> readings** (STaR, DeepSeekMath, DAPO) all carry arXiv's non-exclusive licence, so they are linked, discussed and
> cited on the lecture page but not reproduced, and lecture 6 has no images. Figures are the papers' own, shown with their printed captions; this KB
> writes no descriptions of charts. Papers are always cited at their **original URL**; see
> [sources.md](sources.md).
>
> **Numbering.** The Cairn catalog has nine videos, "Part 1" to "Part 9"; the site's schedule has
> twenty rows, including guest lectures and midterm presentations that are not in the playlist.
> Repo files use the **catalog position**. By title, positions 1–6 are site rows 1–6, position 8 is
> row 17 and position 9 is row 20; position 7 ("Self-Improvement and Deep Research Agents") may be
> row 7, row 8 or both. Positions 2–6 are confirmed against their transcripts; everything after
> them is tentative until built.

## Lectures

- [Lecture 1 — Course Overview](wiki/01-course-overview.md) — the course's argument in one lecture:
  scaling laws and emergent abilities, chain of thought, the pre-training → instruction tuning → RLHF
  pipeline behind ChatGPT, repeated sampling (Large Language Monkeys) and o1-style test-time scaling,
  feeding test-time outputs back into training, how reasoning models think, the move from LLMs to
  agents and agentic workflow patterns, why coding agents became reliable, the generator–verifier
  gap, applications, and course logistics. Includes the student Q&A and links to five later-lecture
  readings the lecture previews.
- [Lecture 2 — Test-Time Compute Scaling](wiki/02-test-time-compute-scaling.md) — getting more from a
  fixed model at inference: repeated sampling and coverage (Large Language Monkeys), the inference
  scaling law and why it is a power law (a long tail of hard problems), verifiable domains and the
  generation–verification gap, Snell et al.'s sequential revisions, ORM/PRM search, difficulty-based
  compute-optimal allocation and test-time compute vs a 14× larger model, and Archon's searched
  inference-time architectures. Embeds figures from all four readings where the lecture discusses them, and links their full text.
- [Lecture 3 — Robust Verification](wiki/03-robust-verification.md) — how to pick the right answer
  once a model can generate it: Cobbe et al.'s trained verifier and the GSM8K benchmark (token-level
  scores, generator vs verifier size, why more samples eventually hurt), Lightman et al.'s outcome vs
  process reward models, PRM800K and active learning, Math-Shepherd's automatic step labels (hard and
  soft estimates) and PRM-driven PPO, and Weaver's weakly supervised ensemble of verifiers and its
  distillation; plus the class Q&A on reward hacking, compute allocation and reasoning models. Embeds
  Weaver's figures; the other three readings are linked only.
- [Lecture 4 — Learning from Feedback with Tools/Code](wiki/04-learning-from-feedback-with-tools-code.md)
  — models that improve from feedback, sorted by where it comes from: ReAct's interleaved reasoning and
  tool calls (the formalism, the Apple Remote example, HotpotQA/FEVER and WebShop results, failure
  modes, prompting vs fine-tuning); RLEF's execution-feedback loop for code (public vs private tests,
  the reward and hybrid token/turn value function, CodeContests results, why base models do not use
  feedback, RL vs SFT); and Constitutional AI (critique–revision fine-tuning, RL from AI feedback, the
  helpfulness–harmlessness tradeoff); plus class discussions on noisy feedback, large code bases and
  whether RL makes frameworks like ReAct obsolete. Embeds figures from all three readings and links
  their full text.
- [Lecture 5 — Planning and Multi-Step Reasoning](wiki/05-planning-and-multi-step-reasoning.md) —
  planning over many steps, three ways: LATS's Monte Carlo tree search over agent actions (the six
  operations, the LM plus self-consistency value function, UCT, HotPotQA and WebShop results, cost and
  irreversible actions); SPRINT's planner and parallel executors, trained on DeepSeek-R1 traces split
  into plans, executions and a dependency DAG (fewer sequential tokens at matched accuracy); and SWiRL's
  synthetic multi-step tool-use data and step-wise RL without live tool calls (process vs outcome
  filtering, cross-task generalisation, RL vs SFT); plus the class Q&A. Embeds LATS and SWiRL figures
  and links their full text; SPRINT and the two readings the recording skips (ADaPT, *Wider or
  Deeper?*) are linked only.
- [Lecture 6 — Train Time Scaling/Scaling RL](wiki/06-train-time-scaling-scaling-rl.md) — training a model
  on its own filtered outputs, and making RL work: why train-time scaling (small models on AIME, o1's train- and
  test-time curves, verifiability); STaR's rationale bootstrapping and rationalization (assumptions, GPT-J results on
  CommonsenseQA and GSM8K, the class discussion of what bounds it); DeepSeekMath's curated math pre-training (Common
  Crawl, code first, arXiv ineffective), GRPO's group-normalized advantage versus PPO's critic, the unified view of
  SFT/RFT/DPO/PPO/GRPO, and RL raising majority voting but not pass@K; DAPO's Clip-Higher, Dynamic Sampling,
  token-level loss and overlong reward shaping (30 → 50 on AIME 2024) and what to monitor; SFT versus RL, open
  problems and the class Q&A. All three readings are linked only (arXiv non-exclusive licence); no images.

## Topics

Cross-lecture concept pages. Each draws on the lectures built so far and will gather later lectures
as they are built.

- [Test-time scaling](wiki/test-time-scaling.md) — repeated sampling with a verifier, coverage vs
  pass@1 vs pass@k, the inference scaling law, revisions and PRM search, compute-optimal allocation by
  difficulty, inference-time architectures, o1's log-linear curve, tree search over agent actions
  (LATS), running the independent parts of one reasoning trace in parallel (SPRINT), and where test time and
  train time differ (lecture 6).
- [Verifiers](wiki/verifiers.md) — what a verifier does, unit tests and other verifiable domains,
  verifiers vs LLM judges, the generation–verification gap as measured in Large Language Monkeys,
  training a verifier, outcome vs process reward models, step labels without humans, ensembles of
  weak verifiers, public and private tests inside an RL loop, AI feedback as a judge, and LLM judges
  as a search value (LATS) and a step-wise process reward (SWiRL), and the known final answer as STaR's filter and
  DAPO's rule-based reward.
- [Chain of thought](wiki/chain-of-thought.md) — the tennis-ball prompting example, why it only works
  in large models, chain-of-thought fine-tuning, whether it was emergent or trained, and why
  ungrounded reasoning hallucinates where ReAct's tool use does not, and STaR's bootstrapping of rationales from a
  model's own correct answers.
- [Reasoning models](wiki/reasoning-models.md) — the thinking behaviours (analysis, decomposition,
  feedback, self-correction, backtracking), the o1 bash example, where they beat GPT-4o, the Q&A
  on why they work, SPRINT's training of a reasoning model to plan and execute in parallel, and whether reflection
  emerges during RL or was already there.
- [The LLM training pipeline](wiki/llm-training-pipeline.md) — pre-training, fine-tuning on
  high-quality data, instruction tuning, RLHF with reward models, RLAIF, where Constitutional AI
  replaces human harmlessness labels with AI feedback, SWiRL's step-wise RL for multi-step tasks, DeepSeekMath's math
  pre-training and instruction tuning before RL, and when to choose SFT or RL.
- [Reinforcement learning for LLMs](wiki/reinforcement-learning.md) — the one objective behind every RL method in the
  course, where the reward comes from (human preferences, AI feedback, tests, process reward models, judges, rules),
  PPO, GRPO and DAPO's four fixes, outcome versus process rewards, RL versus SFT, and what RL does and does not
  improve.
- [Scaling laws](wiki/scaling-laws.md) — loss vs compute, data and parameters, model sizes from BERT
  to GPT-4, few-shot learning, emergent abilities, the saturation that turned attention to
  inference, and inference scaling laws for repeated sampling.
- [Agents and agentic workflows](wiki/agentic-workflows.md) — what makes an agent, today's static
  workflows, building blocks and orchestration patterns (chaining, routing, parallelization,
  orchestrator, evaluator, verifier), coding agents, applications, ReAct's reason–act loop,
  coding agents trained on their own test runs, and planning over many steps (LATS's tree search,
  SPRINT's planner and parallel executors, SWiRL's multi-step tool use).
- [Self-improvement](wiki/self-improvement.md) — the course's central idea: test-time generation as
  training data, test generation in coding, feedback as the limit, model-labelled rewards, feedback from
  the environment, code execution and a constitution, training on self-generated multi-step
  trajectories (SWiRL), STaR's bootstrapped rationales, and what is not yet understood about RL — including
  DeepSeekMath's finding that RL made a model more consistent rather than more capable.
- [Course logistics](wiki/course-logistics.md) — format, Canvas/Ed/Gradescope, the grading
  breakdown and due dates (including where the site contradicts itself), project rules, late days
  and audits.

## Raw materials

- [`raw/transcripts/`](raw/transcripts/) — **edited** lecture transcripts with `[MM:SS]` paragraph
  marks. Read these: auto-captions were copy-edited for punctuation and mis-heard terms, and
  unrecoverable passages are marked `[Ed: unclear]` rather than guessed.
- [`raw/transcripts/original/`](raw/transcripts/original/) — the verbatim auto-captions, kept as the
  reference for what was actually said.
- [`raw/papers/`](raw/papers/) — full text of the paper readings ingested so far (lecture 2's four,
  Weaver for lecture 3, lecture 4's three, and LATS and SWiRL for lecture 5), transcribed from arXiv LaTeX source with printed
  section, figure and table numbers. Cite a paper by section, figure or table. Large Language Monkeys
  includes its appendix; the others are main body only.
- [`raw/images/`](raw/images/) — figures cropped from those papers' PDFs with their printed captions,
  embedded in the paper files and the lecture pages. Only lectures 2–5 have images; lecture 3's are
  Weaver's alone, and lecture 5's are LATS's and SWiRL's. Use an image path you have read in a file; never construct one.
- [`sources.md`](sources.md) — every paper reading on the course site, grouped by schedule row, with
  its original URL and the catalog position it tentatively belongs to.
- [`kb.json`](kb.json) — machine-readable coverage and provenance, including known caveats.
- [`SEE_ALSO.md`](SEE_ALSO.md) — sibling knowledge bases (CS336, CS224N) worth reading for this
  course, and what each is good for.
- [`AGENTS.md`](AGENTS.md) — how this KB is organized and the conventions for extending it.

Paper PDFs are **not** in this repo (the full text of ingested readings is, in `raw/papers/`). They were downloaded to `raw/pdfs/` on the build machine, which
is gitignored; link and cite papers at the original URLs in `sources.md`.
