# CS329A — Self-Improving AI Agents (Stanford, Autumn 2025)

CS329A is a Stanford graduate seminar on AI agents that "continuously improve themselves through
interaction with themselves and the environment", taught by **Aakanksha Chowdhery** and **Azalia
Mirhoseini**. It starts from self-improvement techniques for language models — verifiers, scaling
test-time compute, search, train-time scaling with RL — moves to agents augmented with tools, code
and memory, then multi-step reasoning, planning and evaluation, with guest lectures from frontier
labs. Students read research papers for each lecture, do three homeworks, and carry out an original
research project.

> **Coverage note — complete for this recording.** This knowledge base covers **all nine catalog
> lectures** (Course Overview; Test-Time Compute Scaling; Robust Verification; Learning from Feedback
> with Tools/Code; Planning and Multi-Step Reasoning; Train Time Scaling/Scaling RL; Self-Improvement
> and Deep Research Agents; Agentic Evaluations and Long Horizon Tasks; Future Research Areas). The
> site's schedule has twenty rows, but the other eleven are guest lectures and midterm presentations
> with no video in the catalog, so this is the whole of the recorded course.
>
> **No slides; papers are the course material.** The course publishes no slides on its public site
> (lecture materials go to Canvas). The course material is the **paper reading list** the site gives
> for each lecture — exactly that list. **Lectures 1 and 9 list no readings** and are built from
> their transcripts alone; lecture 9 does present key ideas from three papers the site never lists,
> and those are described at the depth the lecture gives them, with no full text and no link.
> **Lecture 2's four readings are transcribed in full text** in `raw/papers/`, from
> their arXiv LaTeX sources (CC BY 4.0) — Large Language Monkeys with its appendix, the other three main
> body only. **Of lecture 3's four readings only Weaver is CC BY 4.0**, and only its main body is
> transcribed; the other three (Cobbe et al. 2021, Lightman et al. 2023, Math-Shepherd) carry arXiv's
> non-exclusive licence, so they are linked, discussed and cited on the lecture page but not
> reproduced. **Lecture 4's three readings** (ReAct, RLEF, Constitutional AI) are all CC BY 4.0 and
> transcribed main body only. **Of lecture 5's five readings**, LATS and SWiRL are CC BY 4.0 and
> transcribed main body only; SPRINT (CC BY-NC-SA 4.0), ADaPT and *Wider or Deeper?* (arXiv
> non-exclusive) are linked only, and the recording discusses only LATS, SPRINT and SWiRL. **Lecture 6's three
> readings** (STaR, DeepSeekMath, DAPO) all carry arXiv's non-exclusive licence, so they are linked, discussed and
> cited on the lecture page but not reproduced, and lecture 6 has no images. **Of lecture 7's three readings**,
> AlphaCode is transcribed main body only, relying on its arXiv CC BY 4.0 licence although the PDF prints an
> all-rights-reserved notice; the AlphaCode 2 Technical Report (all rights reserved) and Search-o1 (arXiv
> non-exclusive) are linked only. **Lecture 8's three readings** (METR's time-horizon paper, GDPval,
> DeepScholar-Bench) are all CC BY 4.0 and transcribed main body only, from the arXiv versions current on the
> lecture date — METR's v2 and DeepScholar-Bench's v1, both since revised. Figures are the papers' own, shown
> with their printed captions; this KB
> writes no descriptions of charts. Papers are always cited at their **original URL**; see
> [sources.md](sources.md).
>
> **Numbering.** The Cairn catalog has nine videos, "Part 1" to "Part 9"; the site's schedule has
> twenty rows, including guest lectures and midterm presentations that are not in the playlist.
> Repo files use the **catalog position**. Positions 1–6 are site rows 1–6 and position 7 ("Self-Improvement
> and Deep Research Agents") is row 8, and position 8 ("Agentic Evaluations and Long Horizon Tasks") is row 17; row 7 has no video in
> the catalog. Positions 2–8 are confirmed against their transcripts. Position 9 ("Future Research Areas") is
> row 20, confirmed by its content: it is the quarter's last lecture, it recaps the course and it announces
> its subject as "some future research areas" (≈3:57).

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
- [Lecture 7 — Self-Improvement and Deep Research Agents](wiki/07-self-improvement-and-deep-research-agents.md) —
  improving results by searching a model's outputs, for code and for knowledge. AlphaCode's pipeline (pre-training and
  GOLD fine-tuning, a million samples per problem, filtering on example tests, clustering on generated test inputs, at
  most 10 submissions), its Codeforces ranking, $10\text{@}k$ versus $\text{pass@}k$, selection as the bottleneck, and
  its limits; AlphaCode 2's fine-tuned Gemini Pro policies and scoring model, matching AlphaCode's million samples
  with about 100; the class discussion of difficulty and building reasoning in; Search-o1's agentic search inside a
  reasoning chain and Reason-in-Documents, with GPQA, human-expert and multi-hop QA results; and the closing Q&A on
  overconfidence. Embeds AlphaCode's figures and links its full text; AlphaCode 2 and Search-o1 are linked only.
- [Lecture 8 — Agentic Evaluations and Long Horizon Tasks](wiki/08-agentic-evaluations-and-long-horizon-tasks.md) —
  how to measure what agents can do, three ways. METR's time horizon: task length in skilled-human time at 50% and
  80% success, the three task suites and human baselines, the logistic fit, doubling about every seven months, how
  agents fail, the messiness, SWE-bench and internal-PR checks, and the one-month extrapolation. GDPval's expert-judged
  win rates on real professional work: 44 occupations in 9 sectors, approaching parity but improving roughly linearly,
  instruction-following failures, speed and cost with expert review, and why context matters. DeepScholar-Bench's live
  benchmark for writing related-work sections: knowledge synthesis, retrieval quality and verifiability metrics, no
  system above .19, and oracle-retrieval ablations. Plus the lecture's synthesis of the three and the closing Q&A.
  Embeds figures from all three readings and links their full text (METR v2, DeepScholar-Bench v1).
- [Lecture 9 — Future Research Areas](wiki/09-future-research-areas.md) — the closing lecture, in two
  halves. First, the quarter recapped, then three problems that bound the self-improvement loop and a
  paper proposed against each: multi-agent fine-tuning for **diverse reasoning chains** (generation
  and critic agents, debate, majority voting, why single-model self-training stops improving);
  **DeepSeekMath-V2** for verification that needs no reference solution (a verifier trained on
  human-identified proof issues and a meta-verifier that checks the verifier, for fabricated errors and
  scores that do not follow), almost 42% proof score on IMO shortlist 2024 at $\text{best-of-}32$; and
  an unnamed paper in which one model **proposes its own coding tasks** at the edge of its ability and
  solves them, with a difficulty-based reward, validation and a curriculum — the class calls it the
  Absolute Zero paper. Then efficiency for intelligence: **intelligence per watt**, the cloud compute
  demand behind it, the 77% of chatbot requests that smaller models can already answer, local
  accelerators, and the finding that local models improved 3.1x and intelligence efficiency 5.3x in two
  years. Closes with the directions the instructors name — the foundations of test-time scaling,
  continual learning, test-time-scaling infrastructure, hybrid serving and energy — and the class Q&A.

## Topics

Cross-lecture concept pages. Each draws on every lecture of the course that touches its subject.

- [Test-time scaling](wiki/test-time-scaling.md) — repeated sampling with a verifier, coverage vs
  pass@1 vs pass@k, the inference scaling law, revisions and PRM search, compute-optimal allocation by
  difficulty, inference-time architectures, o1's log-linear curve, tree search over agent actions
  (LATS), running the independent parts of one reasoning trace in parallel (SPRINT), where test time and
  train time differ (lecture 6), choosing 10 submissions from a million samples (AlphaCode, AlphaCode 2),
  reasoning effort, $\text{best-of-}N$ and retry loops on professional tasks (GDPval), and the closing
  lecture's argument that nobody has explained *why* repeated sampling surfaces correct answers.
- [Verifiers](wiki/verifiers.md) — what a verifier does, unit tests and other verifiable domains,
  verifiers vs LLM judges, the generation–verification gap as measured in Large Language Monkeys,
  training a verifier, outcome vs process reward models, step labels without humans, ensembles of
  weak verifiers, public and private tests inside an RL loop, AI feedback as a judge, and LLM judges
  as a search value (LATS) and a step-wise process reward (SWiRL), the known final answer as STaR's filter and
  DAPO's rule-based reward, and example tests, clustering by behaviour and a learned scoring model for code
  (AlphaCode, AlphaCode 2), expert pairwise grading and LLM judges validated against humans for open-ended work
  (GDPval, DeepScholar-Bench), and the verifier that checks the verifier — DeepSeekMath-V2's meta-verification
  of proof critiques, and reward models standing in where the real signal takes days.
- [Chain of thought](wiki/chain-of-thought.md) — the tennis-ball prompting example, why it only works
  in large models, chain-of-thought fine-tuning, whether it was emergent or trained, and why
  ungrounded reasoning hallucinates where ReAct's tool use does not, and STaR's bootstrapping of rationales from a
  model's own correct answers.
- [Reasoning models](wiki/reasoning-models.md) — the thinking behaviours (analysis, decomposition,
  feedback, self-correction, backtracking), the o1 bash example, where they beat GPT-4o, the Q&A
  on why they work, SPRINT's training of a reasoning model to plan and execute in parallel, and whether reflection
  emerges during RL or was already there, and the knowledge gaps that surface in long reasoning chains (Search-o1).
- [The LLM training pipeline](wiki/llm-training-pipeline.md) — pre-training, fine-tuning on
  high-quality data, instruction tuning, RLHF with reward models, RLAIF, where Constitutional AI
  replaces human harmlessness labels with AI feedback, SWiRL's step-wise RL for multi-step tasks, DeepSeekMath's math
  pre-training and instruction tuning before RL, when to choose SFT or RL, and where the prompts themselves come
  from — human experts, or a model that proposes and solves its own tasks (lecture 9) — alongside continual learning
  against the generate-then-fine-tune paradigm.
- [Reinforcement learning for LLMs](wiki/reinforcement-learning.md) — the one objective behind every RL method in the
  course, where the reward comes from (human preferences, AI feedback, tests, process reward models, judges, rules),
  PPO, GRPO and DAPO's four fixes, outcome versus process rewards, RL versus SFT, what RL does and does not
  improve, GOLD as offline RL in AlphaCode, Search-R1's RL-trained search, and rewarding a proposer by how often
  its self-generated tasks defeat the solver, so training sits at the edge of ability (lecture 9).
- [Scaling laws](wiki/scaling-laws.md) — loss vs compute, data and parameters, model sizes from BERT
  to GPT-4, few-shot learning, emergent abilities, the saturation that turned attention to
  inference, inference scaling laws for repeated sampling, and AlphaCode's log-linear solve rate in samples and
  compute, METR's exponential trend in agent time horizons against GDPval's roughly linear one, and the
  efficiency axis lecture 9 adds: intelligence per watt, and how much of two years' gain came from better
  models rather than better hardware.
- [Agents and agentic workflows](wiki/agentic-workflows.md) — what makes an agent, today's static
  workflows, building blocks and orchestration patterns (chaining, routing, parallelization,
  orchestrator, evaluator, verifier), coding agents, applications, ReAct's reason–act loop,
  coding agents trained on their own test runs, and planning over many steps (LATS's tree search,
  SPRINT's planner and parallel executors, SWiRL's multi-step tool use), search calls inside a reasoning
  chain (Search-o1), how long and how reliably agents work (METR's time horizon, failure modes, context), and
  what an environment is for — a proxy for real-world tasks, not an end in itself (lecture 9).
- [Self-improvement](wiki/self-improvement.md) — the course's central idea: test-time generation as
  training data, test generation in coding, feedback as the limit, model-labelled rewards, feedback from
  the environment, code execution and a constitution, training on self-generated multi-step
  trajectories (SWiRL), STaR's bootstrapped rationales, and what is not yet understood about RL — including
  DeepSeekMath's finding that RL made a model more consistent rather than more capable, searching a model's
  outputs and distilling the search system into a model (lecture 7), and the three limits the closing lecture
  names — diversity of reasoning chains, verification without an answer key, and the human data bottleneck.
- [Retrieval and deep research agents](wiki/retrieval-and-deep-research.md) — deep research as a workflow
  pattern, retrieval proposed for test-time answers, search as an agent's action (ReAct, SWiRL), and Search-o1's
  search inside a reasoning chain: why retrieving once is not enough, agentic RAG, Reason-in-Documents, results,
  and the class's open questions; and evaluating deep research systems with DeepScholar-Bench.
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
  Weaver for lecture 3, lecture 4's three, LATS and SWiRL for lecture 5, AlphaCode for lecture 7, and lecture 8's three — lectures 1 and 9 list no readings and add none), transcribed from arXiv LaTeX source with printed
  section, figure and table numbers. Cite a paper by section, figure or table. Large Language Monkeys
  includes its appendix; the others are main body only.
- [`raw/images/`](raw/images/) — figures cropped from those papers' PDFs with their printed captions,
  embedded in the paper files and the lecture pages. Only lectures 2–5, 7 and 8 have images; lecture 3's are
  Weaver's alone, lecture 5's are LATS's and SWiRL's, lecture 7's are AlphaCode's, and lecture 8's come from all
  three of its readings. Use an image path you have read in a file; never construct one.
- [`sources.md`](sources.md) — every paper reading on the course site, grouped by schedule row, with
  its original URL and the catalog position it belongs to (confirmed for all nine positions; rows 1
  and 20 list no readings).
- [`kb.json`](kb.json) — machine-readable coverage and provenance, including known caveats.
- [`SEE_ALSO.md`](SEE_ALSO.md) — sibling knowledge bases (CS336, CS224N) worth reading for this
  course, and what each is good for.
- [`AGENTS.md`](AGENTS.md) — how this KB is organized and the conventions for extending it.

Paper PDFs are **not** in this repo (the full text of ingested readings is, in `raw/papers/`). They were downloaded to `raw/pdfs/` on the build machine, which
is gitignored; link and cite papers at the original URLs in `sources.md`.
