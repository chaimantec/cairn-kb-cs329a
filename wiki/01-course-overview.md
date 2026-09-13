# Lecture 1 — Course Overview

The opening lecture makes the case for the course in one arc. Large language models got better by
getting bigger; post-training (fine-tuning on high-quality data, instruction tuning, RLHF) turned
them into useful assistants; and since roughly 2024 **inference itself** has become a second axis
of scaling — sampling many answers, thinking longer — whose outputs can be fed back into training.
That feedback loop is the *self-improvement* the course is named for. The lecture then argues that
the next step is **agents**: systems that pursue a goal over many steps, act on an environment,
take feedback, and correct themselves — and that doing this reliably needs exactly the skills the
rest of the course covers: planning, multi-step reasoning, verification and self-correction. The
last fifteen minutes are course logistics.

It is taught jointly by **Aakanksha Chowdhery** and **Azalia Mirhoseini**, who alternate
throughout; the captions do not identify every hand-over, so this page attributes a passage to one
of them only where the lecture makes it explicit.

**No slides or readings.** The course publishes no slides outside Canvas, and the course website
lists **no paper readings for this lecture** — its "Paper Readings" column is empty for the Mon
Sep 22 Course Overview row. Everything here is cited to the transcript by timestamp. Where the
lecture previews a paper that the site lists as a reading for a *later* lecture, that paper is
linked below at its original URL; this KB has not ingested it for lecture 1.

[Edited transcript](../raw/transcripts/01-course-overview.md) ·
[verbatim captions](../raw/transcripts/original/01-course-overview.md) ·
[video](https://www.youtube.com/watch?v=6YnLB0XbTnI) ·
[course website](https://cs329a.stanford.edu/) · [sources](../sources.md)

> **Numbering.** This is catalog position 1 ("Part 1 | Course Overview") and site schedule row 1.
> From position 2 onward the catalog's nine videos and the site's twenty schedule rows diverge;
> see [INDEX](../INDEX.md).

## The instructors

Chowdhery introduces herself as having worked on large language models "for a while", an adjunct
professor at Stanford, and a researcher at the startup Reflection AI (≈0:05). Mirhoseini is an
assistant professor in the CS department; the two met at Google Brain, and she has also worked on
Claude at Anthropic and on Gemini at Google DeepMind (≈0:52). This is the second time they have
taught the course, updated in both lectures and format (≈0:52).

## Scaling: why bigger models got better

The lecture's starting point is the scaling behaviour that became obvious after GPT-3: as you add
parameters, models improve — BERT and T5 "were good", and larger models were much better (≈2:23).
Chowdhery describes three plots of **scaling laws** for pre-trained base models. Test loss falls as
you increase **compute**, as you increase **dataset size**, and as you increase **parameter count**
(more layers, more parameters) (≈2:23–3:09). This was "the foundation for a long time", until
roughly the year before the lecture, when it began to hit "some kind of a saturation point"
(≈3:55).

She illustrates the growth in model size from 2018 to 2024 — BERT at 340 million parameters,
GPT-2 at 1.5 billion, GPT-3 at 175 billion, PaLM at 540 billion, and GPT-4 "estimated" at
trillions — calling it exponential growth, and noting that the chart is a little out of date
(≈3:55). See [scaling laws](scaling-laws.md).

### What scale buys

Bigger models brought three things (≈4:42–5:29):

1. **Better benchmark performance**, across natural-language and reasoning benchmarks.
2. **Few-shot learning.** Where earlier models had to be fine-tuned for each domain, a large model
   can follow a template from a few examples in its prompt, which "makes it extremely easy to
   prototype things". The worked example is a prompt that says *translate English to French* and
   then asks for *cheese*: answering from the task description alone is **zero-shot**; adding a
   few example translations first is **few-shot** (≈5:29–6:15).
3. **Emergent behaviour** — capabilities such as reasoning that "only emerge in larger models",
   and that could not be predicted before those models existed (≈5:29, ≈7:01).

## Chain of thought

Mirhoseini takes over for the most important of those emergent behaviours, **chain of thought**
(≈7:01). In ordinary one-shot prompting the model sees one solved example and is asked a similar
problem. With chain of thought, the example also shows the *reasoning*. Her example: *Roger has
five tennis balls. He buys two more cans of tennis balls. Each can has three tennis balls. How many
tennis balls does he have now?* — and instead of just "11", the example walks through it: Roger
started with five balls, two cans of three is 6, and 5 plus 6 is 11 (≈7:47). Any 1B-parameter
model can do that problem today without help, she notes, but the chain-of-thought property "is
holding to this day" and underpins reasoning and thinking models (≈8:34).

The property appears only with scale. On a results chart for three model families — LaMDA, GPT and
PaLM — on a math dataset, the small versions (around 8 billion parameters for LaMDA, around 7
billion for GPT) get nothing from chain of thought, while larger ones can use the reasoning in
context to solve problems better (≈9:20). *The captions garble the PaLM part of that sentence, so
the exact PaLM size quoted is not recoverable.* Other abilities show the same sudden appearance at
a certain size, such as modular arithmetic and word unscrambling (≈10:06). This is why frontier
labs keep pushing scale: not only for the smooth improvement, but for more emergent behaviours
(≈10:06). See [chain of thought](chain-of-thought.md).

## From GPT-3 to ChatGPT: the post-training pipeline

ChatGPT launched in November 2022 and reached one million users in five days, far faster than the
other well-known services on the comparison chart (≈10:52). What made it leap past GPT-3 was not
scale alone but two further pieces: **instruction tuning** and **reinforcement learning from human
feedback** (≈11:39). Mirhoseini walks through the whole pipeline (≈11:39–19:21); see
[the LLM training pipeline](llm-training-pipeline.md).

- **Pre-training** — "the easiest step": train the model to predict the next token over all kinds
  of text (≈11:39). The result has seen the internet and books but "has no sense" of right and
  wrong, and does not know how to follow instructions (≈12:25).
- **Alignment with human preferences** is the goal of what follows: steering models toward human
  goals, preferences and values — "still a big problem" that has not been mastered (≈13:11). Charts
  show a pre-trained model fine-tuned for qualities such as *sensibleness* and *safety* on curated
  data (≈13:11–13:56).
- **Fine-tuning on high-quality data**, with the same next-token objective as pre-training but on
  much better data — books, creative essays — which companies may pay millions or hundreds of
  millions of dollars for (≈13:56–14:44).
- **Instruction tuning** — training on instruction and question–answer pairs so the model learns to
  follow questions and answer them, e.g. *Please answer the following question: what is the
  boiling point of nitrogen?* with its answer. A variant, **chain-of-thought fine-tuning**, shows
  the model a worked process before the label (≈15:30). This data is described as a mix of
  human-generated data, templates and synthetic data (≈14:44; *the captions garble the middle item*),
  and its quality and generality strongly shape the resulting model (≈16:16).
- **RLHF**. Instead of supervised prompt–label pairs, companies pay humans — sometimes experts —
  to rate model answers, and fit a **reward model** to those preferences. The reward model then
  stands in for the human, guiding the LLM's parameters toward generations it scores well
  (≈17:01–17:47). Rewards can target different qualities — correctness, helpfulness, specificity,
  harmlessness — weighted according to what matters most for the model being built (≈17:47–18:34).

## Inference as a new frontier: repeated sampling

"Since a year and a half ago", Mirhoseini says, it has turned out that **inference** is also a
frontier for capability (≈19:21). Her lab's example is **Large Language Monkeys**, named after the
infinite monkey theorem — the unproven claim that a monkey typing forever would eventually produce
Shakespeare (≈19:21–20:08). Let the LLM be the monkey: ask it the same problem many times
(**parallel sampling**), and use a **verifier** or selection mechanism to pick a correct response as
the system's output (≈20:57). For code, the verifier can be unit tests run against each generation
(≈20:57–21:44). This works because generation is not deterministic, and **temperature** controls how
varied the responses are (≈21:44).

The results: on math and coding benchmarks, raising the number of samples per problem from 1 to
10,000 and plotting **coverage** — the fraction of problems solved by at least one sample — shows
smaller open models that are worse than GPT-4o (the red dashed line) with one sample beating it once
they are sampled enough, "in all of these cases" (≈22:34). *The captions garble the names of the
smaller models, so this page does not name them.* The conclusion drawn is that models "already know
a whole lot more than what you get out of them when you just ask them once" (≈23:21). It is called
**inference scaling** because the model's parameters are untouched (≈23:21). For some problems only
three or four of the 10,000 samples were correct, which shows how much scaling inference matters
(≈24:09). See [test-time scaling](test-time-scaling.md).

Student questions on this result drew out four points:

- **Why not just search the answer space?** The space of possible answers is vastly bigger than
  what a model's samples cover; 10,000 samples is "highly sample efficient", and some of the
  problems were hard IMO-level problems a small model solved this way (≈24:56–25:42).
- **Latency.** Parallel samples can run in parallel, so latency is less of an issue than cost; the
  compute–cost trade-off depends on the type and complexity of problem, and is covered later
  (≈25:42–26:29).
- **Domains without a verifier.** These are harder; there is a body of research on training LLMs as
  judges, LLM reward functions, and LLMs with tools, and a whole lecture is dedicated to verifiers
  (≈26:29–27:14). See [verifiers](verifiers.md).
- **Temperature.** Higher is not always better — too high produces gibberish; "usually if you go
  beyond 1.2 or so, it's not great", though other tricks can raise diversity (≈27:14–28:06).

The paper is listed on the course site as a reading for the Test-time Compute Scaling lecture:
[Large Language Monkeys: Scaling Inference Compute with Repeated Sampling (Brown et al. 2024)](https://arxiv.org/abs/2407.21787).

## Bringing test-time scaling back into training

The next development combines the two axes. With DeepSeek (released, the lecturers confirm, in
December 2024) and the o1 series and Gemini Thinking, test-time scaling became an engine for
generating **synthetic training data**: for math problems with known answers, sample many solutions
that reach the golden answer; for coding, generate large amounts of good solutions — and fine-tune
on them (≈28:06–28:53). Bringing test-time scaling "back to the process of training the model or
fine tuning the model to become better" is "the self-improving piece that we are very excited
about", and it is open-ended (≈29:40). See [self-improvement](self-improvement.md).

Where Large Language Monkeys showed log-linear scaling of *coverage* with the number of samples
(given a verifier), OpenAI's o1 release showed a log-linear relationship for **pass@1** accuracy on
the hard AIME math benchmark as test-time compute grows — the kind of scaling previously shown for
training, now at test time without changing parameter count (≈29:40–31:14).

## How reasoning models think

For hard problems, thinking models, like humans, spend more time and consider more strategies
(≈31:14). Mirhoseini lists the steps such a model goes through (≈32:00): **problem analysis**;
**task decomposition** into simpler, more addressable tasks; trying something and using feedback —
running tests on code, using a calculator, or judging the answer — to improve (*the captions call
this "self-evolution strategies"; the edited transcript marks the term as unclear*); **self-
correction**; and **alternative proposals**, backtracking to try a different approach when
something fails. Some of this was probably in human-curated training data, but much is acquired
during fine-tuning and RL on synthetic data (≈32:46).

The worked example is o1 asked to write a bash script that outputs the transpose of a matrix. It
begins by working out what the user is asking and what the input and output formats are, decomposes
the approach (parse the input, build the matrix as an array of arrays), and self-corrects mid-way —
the familiar "wait, … there's something wrong" (≈32:46–34:20). The difference from chain-of-thought
prompting is that **the model produces the chain of thought itself** (≈33:34). Compared with GPT-4o,
reasoning models such as o1 do better on reasoning-heavy work — math calculation, data analysis,
programming — but not necessarily on personal writing or text editing (≈34:20–35:06). See
[reasoning models](reasoning-models.md).

Questions from the floor on this section:

- **Is the gain from generating the reasoning, or from being asked to decompose?** Mirhoseini's
  answer is that the training data has made the model "a generalized thinker": decomposition,
  backtracking and analysis are learned skills that generalize (≈35:53). Chowdhery adds a framing
  that recurs through the course: a base model can already produce some good reasoning chains among
  many, but does not know which is correct. Much of train-time and test-time scaling "really comes
  down to it learns which is correct" — which raises **pass@1**, as in the reasoning-model results,
  as opposed to **pass@k** or coverage, as in the repeated-sampling results (≈36:41).
- **Could a different, smaller model produce the final answer?** Reasoning ability has grown with
  model size, so if anything you would use the larger model's traces and perhaps have a smaller
  one summarize. And models, "at least currently", prefer their own traces, even over traces from a
  better model (≈37:30–38:15). The course's multi-step reasoning lecture discusses SWiRL, which
  brings in a different model to evaluate and give feedback rather than to generate traces
  (≈38:15).
- **How are reasoning models taught to reason?** There is no published work that fully covers it;
  it is "a bit of both" hard-coded templates and fine-tuning, starting from a base model that
  already had some thinking capability. Chain of thought in instruction-tuning data shows the model
  ways to think, and the course will cover **outcome reward models** and **process reward models**
  as sources of feedback (≈39:01). Models now generalize well beyond their training instructions
  (≈39:48).
- **Can the number of samples depend on problem difficulty?** Follow-up work uses a reward model
  that, if it captures some notion of complexity, can guide more sampling on unsolved problems — an
  interesting direction to explore (≈39:48–40:36).

## From LLMs to agents

Chowdhery turns to why the course exists (≈40:36). LLMs, as chatbots or reasoning models, are still
essentially **single-turn**: fun to interact with, but not accomplishing tasks for you. In the months
before the lecture, agents such as **Claude Code** and **Deep Research** began doing real
workflows end to end (≈41:22) — researching where to rent a home for a year to take a class at
Stanford by reading many websites and summarizing pros and cons, or, with Claude Code or OpenAI's
Codex, modifying files and working out test cases from English instructions (≈41:22–42:08).

What makes something an **agent** rather than a chatbot (≈42:08–42:57):

- it is given a **goal**, and plans steps toward it;
- it **takes actions** in an environment, possibly through **tools** external to the model;
- it gets **feedback** and corrects its steps;
- it decides **when to stop** — goal achieved, or report that it cannot be;
- and it may need **memory** to stay on track.

In practice, simpler tasks such as deep research can already be done end to end, but most deployed
systems are **agentic workflows**: fairly static graphs built by hand (≈42:57–44:28). Two cartoon
examples: one model proposes a solution and another judges whether to accept it; or, as in deep
research, an LLM is called on many inputs and the results are aggregated into a summary (≈43:43).
For open-ended problems it is easier to hand-build the graph of how a human would do it, with an
LLM evaluator for feedback, than to run a fully open loop — though coding and research show "signs
of life" for the open loop (≈44:28).

The building blocks are LLM calls, **verifiers**, critics or judges ("effectively LLM as a judge"),
and **tool calls** such as web search or a weather lookup (≈44:28–45:14). They are orchestrated in
patterns (≈45:14–47:33):

- **Prompt chaining** — a task decomposed into a chain of subtasks.
- **Routing** — send complex inputs to a more elaborate set of calls and simple ones to a simpler
  workflow.
- **Parallelization** — several calls at once, as when deep research investigates different
  keywords, then aggregates; or independent subtasks whose solutions are combined.
- **Orchestrator** — a central "LLM manager" does the planning and makes subsequent calls, as with
  the plan Claude Code produces.
- **Evaluator or judge** — feedback from an LLM rather than from the real world; the homeworks
  cover this.
- **Verifiers** — checks that can actually verify an output, such as unit tests, in verifiable,
  rule-based domains like math and code.

All of these require LLMs to get better at **planning**, **multi-step reasoning** and
**self-improvement** — correcting their own mistakes — which reasoning alone did not deliver, and
which are the topics of the lectures that follow (≈47:33). See
[agentic workflows](agentic-workflows.md).

## Coding agents, and why they became reliable

A coding agent is an LLM interacting with a computer — these days mostly a terminal — given an
instruction such as implementing a test. It navigates the repository, searches and views files,
edits lines, runs commands, and decides from the output what to look at or edit next (≈47:33–48:19).
That loop "was not quite reliable last year" and is only now becoming reliable (≈48:19).

Asked why, given the architecture has not changed much, Chowdhery says the paradigm is the same;
the difference is **more powerful models and better RL** — RL with verifiable rewards is working, and
train-time scaling is working (≈49:05). Once models improve, a **self-improvement loop** kicks in:
they can generate tests, and those tests become more reliable (≈49:05). CodeMonkeys, covered the
previous year, generates unit tests for model-written code and checks whether the tests make things
better (≈49:51). CodeMonkeys is listed on the site for the Agentic Frameworks for Software
Engineering row: [CodeMonkeys: Scaling Test-Time Compute for Software Engineering](https://arxiv.org/abs/2501.14723).

The underlying issue is the **generator–verifier gap** (≈50:38). Models can easily generate a lot of
content, sensible or not, but deciding what is useful needs a feedback loop. In creative writing
there is little feedback to be had, so human feedback becomes the bottleneck; in domains with good
feedback, models can keep improving. Robust verification is hard, and "verification continues to be
one of the bottlenecks in this space" (≈50:38). Mirhoseini will cover a paper from her lab on how to
combine verifiers. The lecture does not name it, but the site's Robust Verification row lists
[Shrinking the Generation-Verification Gap with Weak Verifiers](https://arxiv.org/abs/2506.18203),
whose author list includes Mirhoseini and whose abstract is about combining weak verifiers. See [verifiers](verifiers.md).

A student asks why RL produces such a large jump if the knowledge should already be in pre-training
(≈51:24). Chowdhery's answer is that it is still an open research question: opinions differ on
whether RL or pre-training's diverse data is doing the work, and both help. Restating the question —
if repeated sampling of a pre-trained model already finds one correct answer, feedback should raise
pass@1 but not really improve the model — she says "that whole loop is not completely well
understood", though there are signs that continued RL keeps improving models (≈51:24–52:59).

## Where agents are used

Even with a clear goal, an agent often has to **clarify user intent**, because users under-specify
problems; then find relevant files, act, and verify — for code, by passing tests it may have written
itself (≈52:59–53:45). Models like o3 have seen many such traces end to end (≈53:45). The lecture's
examples of deployment:

- **Repetitive software work** — code migrations, version upgrades, restructuring a codebase, data
  engineering (extracting and cleaning data), data-warehouse migrations, and writing unit tests
  (≈53:45–54:33).
- **Customer support** — live transcription, "Knowledge Assist" (surfacing the relevant article from
  a knowledge base, better than plain search), smart replies in chat, and call summaries; several
  companies are pursuing different segments, and end-to-end systems are starting (≈54:33–55:19).
- **Research reports** — instead of doing a literature review, summarizing each paper and
  synthesizing by hand, an LLM system identifies references for a topic (the example is the 2022
  Winter Olympics opening ceremony), builds an outline, summarizes each reference, and combines them
  into a full-length article. One homework has students try this (≈55:19–56:53).
- **AI scientists** — helping with idea generation, experiment iteration and paper write-up, from
  "the AI scientist paper". Despite hallucination, the breadth of ideas can push brainstorming
  outside what a researcher would have thought of (≈56:53–57:39). The site lists
  [The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery (Lu et al. 2024)](https://arxiv.org/abs/2408.06292)
  for the Open-Ended Evolution of Self-Improving Agents lecture.

## Is reasoning emergent?

A closing question asks whether chain of thought was designed into training or discovered by
prompting, and whether other emergent behaviours might appear (≈57:39). The answer is that chain of
thought was **not baked in by design**: it was discovered by giving models hard problems and seeing
that reasoning chains helped. The lecturer names GSM8K as the first work that "showed signs of life"
of this, and says that with a larger model like PaLM it became clear this was a big deal — one
example being that it could explain jokes (≈58:25). The model had also read the whole web, so it had
seen methodical, systematic text (≈58:25).

But today's reasoning models are **trained** to reason; "the entire reasoning is not an emergent
behavior", and models are converging toward knowing when they need a lot of reasoning and when not
(≈59:11). Chain of thought *originally* was emergent. As for new emergent behaviours, the lecturer
would not frame it that way: for agentic workflows we go looking for planning, multi-step reasoning
and self-correction, there are papers on self-correction and backtracking, and such behaviours can
be seen as reinforced by fine-tuning as much as emergent (≈59:11–59:58).

## Course logistics

Covered from ≈1:00:45 to the end; the course website states the same details with dates, and
[course logistics](course-logistics.md) collects both.

- The topic list keeps last year's overall theme, with guest lectures from frontier AI labs on
  subjects from how post-training has evolved to multimodal agents in robotics, alongside project
  presentations (≈1:00:45).
- Check Canvas for the latest updates; lecture materials are uploaded before each class, and all
  due dates are already posted (≈1:01:32).
- **Three homeworks** this quarter, one more than last time (≈1:01:32).
- **A research project**, with API credits provided, in teams of two to four; working alone is
  allowed but teaming is encouraged, not least for pooled credits (≈1:02:19–1:03:06). Acceptable
  projects include a new evaluation dataset or benchmark, a study of an existing agentic system's
  reliability, hill-climbing an existing benchmark, or improving or questioning a decision in one of
  the course's papers. A survey paper or "just an app" is not acceptable — it must be research, with
  a hypothesis or question, "something more than vibe coding" (≈1:03:52–1:05:23). Students are
  expected to read the papers as lectures go (≈1:04:38).
- **Milestones**: a project proposal in early October, a midterm presentation about two weeks later
  that must show experimental progress, a heavily weighted final report, and a poster session at the
  end of the quarter (≈1:05:23–1:06:08). Past students have published papers from their projects
  (≈1:06:54). The poster session is **December 12, 4:00–6:00 PM**, with industry guests (≈1:06:54–1:07:40).
- **Grading**: the three homeworks are 50% and the project 50% (≈1:07:40). Office hours are posted on
  Canvas; questions go on Ed, publicly where possible; submissions go through Gradescope
  (≈1:07:40–1:08:28). The late policy admits no exceptions because the class is large (≈1:08:28).
  Audits are not allowed; videos will eventually be on YouTube (≈1:09:15).

## Readings this lecture points to

None of these is a reading *for this lecture*. Each is named or described here and listed by the
course site under a later schedule row.

| Paper (original URL) | Where lecture 1 mentions it | Site schedule row |
|---|---|---|
| [Large Language Monkeys (Brown et al. 2024)](https://arxiv.org/abs/2407.21787) | Repeated sampling results, ≈19:21–28:06 | 2 · Test-time Compute Scaling |
| [Shrinking the Generation-Verification Gap with Weak Verifiers](https://arxiv.org/abs/2506.18203) | "a paper that she did in her lab about how to combine verifiers", ≈50:38 | 3 · Robust Verification |
| [SWiRL](https://arxiv.org/abs/2504.04736) | Using a different model for feedback, ≈38:15 | 5 · Multi-step Reasoning/Planning |
| [The AI Scientist (Lu et al. 2024)](https://arxiv.org/abs/2408.06292) | AI-scientist workflow, ≈56:53 | 7 · Open-Ended Evolution of Self-Improving Agents |
| [CodeMonkeys](https://arxiv.org/abs/2501.14723) | Generating unit tests as verifiers, ≈49:51 | 13 · Agentic Frameworks for Software Engineering |
