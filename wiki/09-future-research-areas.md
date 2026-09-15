# Lecture 9 — Future Research Areas

The closing lecture does two things. It recaps the quarter — the self-improvement loop of verifiers,
feedback and reinforcement learning, open-ended evolution, agentic workflows with tools, retrieval
and memory, planning, and evaluation — and then argues about what comes next, in two halves. The
first half presents three ideas for pushing the self-improvement loop further: **diverse reasoning
chains**, so that a model training on its own outputs does not collapse into one way of solving
problems; **verification that does not need a reference solution**, so that the loop does not stop
where the final answer cannot be checked; and **breaking the data bottleneck**, so that the tasks
the model trains on do not have to be curated by human experts. The second half turns to
**efficiency for intelligence** — the lecture calls its metric *intelligence per watt* — and argues
that as inference demand explodes, the interesting question is not only how capable a model is but
how much capability you get per unit of energy, and how much of the traffic could be served by
smaller models on local hardware rather than in the cloud.

The lecture then works through the directions the two instructors consider open — the foundations of
test-time scaling, continual learning, and the systems infrastructure for high-throughput, low-latency
test-time scaling — and takes questions from the class. It ends with the quarter's closing remarks.

The captions do not mark every hand-over, so this page names a lecturer only where the lecture makes
it explicit: **Aakanksha Chowdhery** presents the three papers in the self-improvement half (she says
so at ≈3:57), and **Azalia Mirhoseini** presents the efficiency half (Chowdhery hands it to her by
name at ≈39:49). Elsewhere the page says *one of the instructors*.

[Edited transcript](../raw/transcripts/09-future-research-areas.md) ·
[verbatim captions](../raw/transcripts/original/09-future-research-areas.md) ·
[video](https://www.youtube.com/watch?v=AyO6wyu4DEg) ·
[course website](https://cs329a.stanford.edu/) · [sources](../sources.md)

> **Numbering.** This is catalog position 9 ("Part 9 | Future Research Areas"), and it is site
> schedule row 20, "Future Research Areas" (Fri Dec 5). The lecture is the last of the quarter and its
> content confirms the mapping: it opens by recapping "what we have covered in this quarter"
> (≈0:05) and says explicitly, "we wanted to cover some future research areas" (≈3:57). Rows 9–19
> (guest lectures, midterm presentations, and the agentic software engineering and memory lectures)
> have no video in the catalog.

> **No readings, and no slides.** The course publishes no slides publicly, and the site's schedule row
> for this lecture lists **no paper readings** — so this page is built from the edited transcript
> alone, cited by `[MM:SS]` timestamp, exactly like [lecture 1](01-course-overview.md). The lecture
> *does* present key ideas from three papers; the site does not list them as readings for this row or
> any other, so this KB describes them only at the depth the lecture does, transcribes none of them,
> and does not link them. Only one of the three is named in the lecture at all.

## The quarter in review

The lecture opens with a recap in one paragraph (≈0:05–1:39). The class began with **test-time
scaling** and **train-time scaling** using self-improvement techniques — "the fundamental loop is
driven by verifiers, and feedback from running those verifiers and getting rewards" (≈0:05) — with
reinforcement learning and search algorithms helping models climb on math and coding. From there it
moved to **evolutionary strategies**, a whole class on open-endedness in which the model is allowed
to explore rather than only to chase reward, and where AlphaEvolve and similar techniques were
discussed. Then came **end-to-end agentic workflows** built on tool use: as models interact with
environments and tools, workflows can be driven end to end, and tasks that span many steps need
knowledge bases, and therefore retrieval and memory. That led to **planning and multi-step
reasoning**, and to thinking about **evaluations** for the next generation of models and which
capabilities would drive them (≈1:39).

Mirhoseini is asked whether she would add anything, and adds the **symbolic techniques** one of the
guest speakers covered, under the umbrella of tool use, and how they can add to **synthetic data**,
"which is another trend we are seeing" (≈1:39). See
[test-time scaling](test-time-scaling.md), [verifiers](verifiers.md),
[agentic workflows](agentic-workflows.md) and
[retrieval and deep research](retrieval-and-deep-research.md).

## What the course means by an agent

Before the future directions, the lecture restates the course's central object (≈2:25–3:57). The
class is named for self-improving *agents*, so "it's not just focusing on LLMs. It's focusing on the
agent aspect of it": an agent is a **generalization of an LLM** that has a goal, interacts with an
environment, collects feedback, and uses that feedback to correct its steps — "systems that can
direct their own processes, use tools, and then accomplish a goal" (≈2:25).

In today's paradigm, the LLMs by themselves are often not powerful enough to drive towards a full
goal, so these systems are frequently **hand-written workflows** orchestrating LLMs and tools —
though in some scenarios, such as coding agents, the workflows are increasingly driven by the models
themselves (≈3:11). Constructing such a workflow means orchestrating LLMs, **verifiers** (because you
need some form of reward), possibly LLM-as-a-judge verifiers, **tool calls**, and **search
algorithms** with parallel LLM calls exploring (≈3:11). And because an agentic workflow is driving
towards a goal, it "needs the capability to plan and to reason over multiple steps, correct itself if
it were going in the wrong direction, and overall keep improving in its capabilities, which is where
the self-improvement aspect comes in" (≈3:57). See
[agentic workflows](agentic-workflows.md).

## Three problems that bound the self-improvement loop

The lecture organizes the self-improvement half around three questions the course's techniques have
not answered (≈3:57–6:17). First, the loop is "still limited to narrow domains like math and coding":
how do you **generalize** across domains, and how do you keep the reasoning chains **diverse** enough
that the loop keeps going? Second, building these loops "continues to be" about **verification** —
robust verification, or meta-verification, where what went into the reasoning chain is itself
verified. Third, even in train-time scaling, "the prompts that go into training these models gets
selected very statically and require humans to select them": how do you **break through the data
barriers** so that the self-improvement loop can pick the right data to drive itself?

Each of the three papers that follows is presented as a direction on one of those problems. See
[self-improvement](self-improvement.md).

## Diversity: multi-agent fine-tuning

The first paper "is coming from multi-agent finetuning, which focuses on self-improvement with
diverse reasoning chains" (≈6:17). This is the source the lecture gives for it; it is not on the
course's reading list and the lecture gives no URL.

### Why self-training stops improving

The argument starts from what is bottlenecked (≈6:17–7:51). Pre-training compute is "a compression of
internet scale data", and instruction finetuning "often requires real human data, where you need
human preferences to decide what is good model response and bad model response" (≈7:05). The
alternative is **synthetic data** generated by LLMs, used in **iterative fine-tuning**: generate
possible solutions, filter out the incorrect ones — this is **rejection sampling** — and fine-tune on
the good ones only (≈7:05). There are many variants, STaR among them, and reasoning chains can be
added to the process (≈7:05). See [self-improvement](self-improvement.md) for STaR and
[reinforcement learning](reinforcement-learning.md).

The problem is **diversity**. If a single large language model generates the data, it "will generate
solutions that will be very similar", and performance stops increasing "after a few iterations or
after tens of iterations" (≈7:51). The lecture's explanation is a comparison with pre-training: the
pre-training corpus is so diverse because it was generated over a long time by humans, and that
diversity is part of why it helps. But when a single model generates outputs for a set of prompts,
"it will not have very diverse responses even at high temperatures" (≈7:51). This is the same
saturation the course saw earlier — RL that raises majority voting without raising
$\text{pass@}k$, and the question of whether a model is learning or only becoming more consistent
(lecture 6). See [reinforcement learning](reinforcement-learning.md).

### The method: generation agents, critic agents, debate

The intuitive answer is **multiple agents specialized in some way**, used to improve diversity
(≈8:38). The paper proposes "multiple specialized agents for generation that will then produce
diverse initial solutions", and trains both **generation agents** and **critic agents** (≈8:38).

The step-by-step process (≈8:38–11:47):

1. Each **generation agent** produces an initial answer to the question.
2. A **summarization** step runs across the generation agents' answers — either summarized by a
   model or simply concatenated (≈11:47).
3. The **critic agent** critiques that updated set of answers, and the critique is added to the input
   (≈9:23).
4. All the generation agents produce **updated answers**, using the summary of the other agents'
   responses as part of their input.
5. **Majority voting** runs on top of the updated answers, and the responses are summarized again.
6. The process can continue as a **debate** over multiple rounds.

The lecture stresses what is different from the familiar critique loop: "instead of getting the
critique over a single agent's response, you're actually using multiple agents' responses and then
specializing and summarizing across them" (≈10:13). Diversity therefore exists **before** the critique
stage: you "basically get majority voting for free just by having multiple agents and assuming that
these are trained slightly differently" (≈10:13).

The training details the lecture gives (≈10:13–11:47):

- The **generation models** are fine-tuned from the same base model, with the goal of producing good
  answers given a question. Across iterations they take outputs, **filter for agreement with the
  majority vote**, and run SFT on those prompt–response pairs. The lecture notes the naive version
  people use in practice: just use *different models* with different prompts, "the poor man's version
  of doing multiple generations" (≈11:00).
- The **critic models** take the updated answer and select the best one. They are fine-tuned on a mix
  of trajectories where the answer is correct at the start and is corrected over the course of a
  debate — so "the critic model is learning how to contrast the correct and the incorrect answer"
  (≈11:00).

### What it buys

The evidence is presented through two axes (≈12:35–13:22). On the **x-axis** is the number of
fine-tuning iterations. On the **y-axis** is either **negative log likelihood**, "just a proxy for
performance", or **embedding dissimilarity**, where a higher value means more diverse. The lecture
reads two things off the results: on two open-source models, multi-agent fine-tuning **continues to
increase performance** across iterations, and "Llama seems more responsive to this technique"
(≈12:35); and on the right-hand plot, the responses "continue to stay quite diverse" as the process
goes on, over math as a dataset (≈13:22).

The experiments are on **math**, across **three open-source models** — fine-tuning meant they had to
stay with open-source models (≈13:22). Against **single-agent fine-tuning**, where "the accuracy
collapses or doesn't continue to improve", the multi-agent technique kept improving over multiple
steps of fine-tuning. The paper also shows an **adjacent domain**, GSM8K, where the fine-tuned agents
show higher performance: the lecture notes this "is a slightly older piece of work, so the numbers
are not that high", but reads the result as evidence that the technique "generalizes beyond just in
domain data sets that it's trained on" (≈14:08).

The takeaway the lecture draws: "if you want self-improvement, the reasoning chains that are provided
to the model to drive those need to be diverse in some way", and one technique for generating them is
to have multiple models or agents generate the chains (≈14:08). See
[self-improvement](self-improvement.md).

## Verification: DeepSeekMath-V2 and the meta-verifier

The second problem is **verification**, which the course covered in its first few lectures — "it's
quite hard to verify model outputs", and typically "you would just look at the outcome" (≈14:53).
Here the lecture names its paper: **DeepSeekMath-V2**, "a recent paper that came out which tries to
show that, at least in the theorem proving domain, it is possible to build self-verification loops,
which are automated" (≈14:53). The course's row-6 reading list has a *DeepSeekMath* paper
([sources](../sources.md)); the lecture does not relate the two, and this page does not either.

### Why outcome rewards are not enough

The current reinforcement learning approach "assumes that the final answer will match the ground truth,
and that's how you're basically creating the rewards", and this has "enabled saturation of multiple
benchmarks" in math (≈15:40). But "oftentimes, even when you have the correct answer, you might not
have the correct reasoning" — and getting models to do better at theorem proving takes "rigorous
step-by-step derivation, which the final output doesn't quite give you" (≈15:40). See
[verifiers](verifiers.md) and [reinforcement learning](reinforcement-learning.md).

Why the obvious fix does not work (≈16:26): large language models "are often trained on quantitative
reasoning, so the proofs that they might generate are mathematically invalid. And if you ask them to
verify, they will claim that the incorrect proofs are valid." So **LLM-as-a-judge does not work here**
(≈16:26). What a human expert does instead is read the proof and say it "has issues … this next step
is not following from the last step or there's reasoning gaps in the proof solution" (≈16:26).

### The architecture: verifier and meta-verifier

The paper's move is to train **verifiers, or meta-verifiers**, rather than only reward models
(≈17:15). Humans identify issues in proofs **without any reference solutions**, and LLMs are trained
on that to identify issues and improve on them — producing "a model which can now critique the proofs
themselves" (≈17:15).

The final architecture (≈18:01–18:48) has the familiar **generator** producing proofs and a
**verifier** verifying them, and adds a **meta-verifier** that "will review the verifier's analysis
for whether it makes sense or whether there are issues with the proofs". The verifier is trained to
take the issues and **score the proofs on a scale of $0.5$ and $1$**. The verifier then improves the
generator, the generator "will produce harder proofs, which will then go improve the verifier", and
"this ends up being a loop in some way" (≈18:01).

The interesting consequence is **automation of the labelling**: once you have data on finding
incorrect proofs, that kind of labelling can be automated, so you do not have to rely on humans
identifying issues forever once the meta-verifier starts to learn this (≈18:48).

### What meta-verification actually checks

The lecture is careful about why this matters (≈18:48–19:34). Verifiers "can get correct score when
the reasoning chains are incorrect" — for example, they "might come up with fabricated errors". So
meta-verification is **evaluation of the evaluation**: does the verifier-as-judge's identified issue
actually exist, and does the score follow from the issue? Experts annotate the quality of that
evaluation, and "just by adding this additional block, they are able to improve the quality"
(≈19:34). The lecture frames it as one more layer: there are reasoning chains, then a verifier over
those chains, and now a judgment of "is this evaluation correct or not" (≈19:34).

### Results, and how to generalise

The results are benchmarked on **IMO** problems and one other competition set, *which the captions
give as "CNML" and the edited transcript marks `[Ed: unclear]` — no benchmark of that name appears
anywhere in this course's materials*. The lecture also mentions that "the Gemini version of that was
presented by one of our guest lecturers" (≈20:23) — a guest lecture that is not in the catalog.
Building on DeepSeek-V3-based models, "in eight iterations just $\text{pass@}1$, the proof score
continues to climb just with that iterative loop"; and with
**$\text{best-of-}32$** — picking the best of 32 generated proofs — "you are almost getting to 42% in
proof score for IMO shortlist of 2024" (≈20:23). The lecture calls this "quite promising as a
hill-climbing technique" (≈20:23).

The insights the lecture states (≈21:09–21:54): the best proofs "will achieve higher verification
scores", the generator "can learn to differentiate higher quality proofs from flawed proofs", and
self-verification therefore gives a better improvement loop. Verification "can be a bottleneck and
this is one way to break that bottleneck" (≈21:09).

To generalize the technique to other areas, the lecture lists three ingredients (≈21:54): LLM-based
verifiers that can **identify issues without reference solutions**; an **additional meta-verification
block** to reduce the chance of **hallucinated issues**; and **an additional incentive for the
generator to maximize quality through deliberate reasoning**. The limitation is stated plainly: "this
is still limited by domains where verification is easier rather than more difficult" (≈22:39).

## Data: a model that proposes its own tasks

The third problem is which data or prompts to train on (≈22:39). The lecture calls the paper it
presents "very interesting", noting that it "has not seen much use yet, but is quite promising"
(≈23:25). As with the first paper, the lecture does not name it or link it. Later in the quarter's
Q&A a student refers to it as "the Absolute Zero paper" (≈1:03:34); that is the class's name for it,
not one the lecturer uses.

### Why human-curated prompts are a bottleneck

In the current training stack you either use **human-curated reasoning traces** for supervised
learning, or, for reinforcement learning with verifiable rewards, you expect **experts to curate the
question–answer pairs** (≈23:25). That means building a math model needs math experts; IMO problems
need IMO experts; coding needs strong software engineers (≈24:12). "As the models continue to surpass
human intelligence, the ability to find more and more experts and more and more such tasks starts to
be limiting" (≈24:12). See [llm-training-pipeline](llm-training-pipeline.md).

The paper's proposal "goes to the other extreme": **a single model can both propose tasks and then
solve them**, so that "we should not really need an external source of data" and should not need
human-generated prompts to climb on (≈24:12). The lecture notes this "is feasible in certain
domains" and that the paper focuses on **coding** (≈24:59).

### Three task types

The proposer's task constructions come from coding paradigms (≈24:59–26:31), and the lecture
describes three:

- **Deduction** — generate a program and input; the environment executes it to get the output. "Your
  usual come up with a program and an input pair", where the environment computes the outputs
  (≈25:45).
- **Abduction** — similar: generate the program and the inputs, and the environment again computes
  the outputs (≈26:31).
- **Induction** — different: **sample an existing program**, generate **new inputs** for it plus a
  natural-language description of what it does, and let the environment execute to decide whether
  this is going in the right direction (≈26:31).

The proposer is **conditioned on past examples**, explicitly prompted, so that they are added to
promote diversity (≈26:31) — the lecture returns to this as the **buffer**.

### Choosing tasks at the edge of ability

How tasks are selected (≈27:18): each generated task is **passed to the solver**, and the proposer's
reward depends on the solver's **success rate** on it. If the success rate is zero, the reward is
zero; if it is non-zero, the reward is **$1$ minus the average success rate**. So the tasks that get
selected are "the ones that are not trivial and the ones that are not impossible" — those of
**moderate difficulty**, where the solver sometimes succeeds and sometimes fails, which "will
generally help with getting the model to learn" (≈27:18). The proposer's job is therefore "to
generate tasks for optimal task difficulty at a current set of model weights", and as the model
becomes more capable "the proposer should learn to propose harder problems" (≈28:04). See
[reinforcement learning](reinforcement-learning.md).

### Validation, the buffer, and the curriculum

Generated tasks are **validated before they enter the training pipeline** (≈28:04–28:52). In the
coding abstraction the paper can run **program integrity** checks, seeing whether executing the task
produces errors; run **safety checks**; and check that running the inputs **multiple times gives
exactly the same outputs**, which is possible in code. This is what stops the proposal stage from
"just hack and come up with garbage tasks" (≈28:04).

A **buffer** is kept: for every seed triplet — input, output and program — the triplet is added to a
task buffer, the proposal can sample references from it, and it records how many times the model is
succeeding or failing on each task (≈28:52). The lecture reads this as "this idea of **curriculum
learning** that is evolving over time" (≈28:52).

### Results, and the class's connection to SWiRL

What the paper shows (≈29:40–31:12): it reaches **state of the art on coding benchmarks even though
there was no human-curated data on the prompt side**, and it "outperform[s] models trained on tens of
thousands of expert examples" (≈29:40). The emergent behaviours the lecture lists are that
**complexity metrics increase over time** (expected, if the proposer is raising difficulty),
**diversity of programs and answers improves** "when the loop is set up correctly", and the proposer
"is actually increasingly generating more and more difficult tasks as the training is progressing"
(≈29:40). The lecture describes the relationship as "almost like game theory where the proposer and
solver are slightly adversarial, but overall, they are helping each other improve" (≈30:26).

Mirhoseini connects this to a lecture the course had already had: SWiRL's synthetic multi-step tool-use
data showed the same thing — "synthetic data like model generating its own training data can improve
the model not only on the task that the generation is happening on but also in transferability",
with a model trained on multi-hop retrieval question answering getting better at calling Python and
solving math problems, and vice versa (≈31:12). She adds that **larger models** seem better at
absorbing "this kind of data flywheel" and at generalizing through it, which was also the SWiRL
observation on the RL optimization side (≈32:44). See [self-improvement](self-improvement.md) and
[reinforcement learning](reinforcement-learning.md).

The lecture's own summary of the third idea (≈30:26–31:12): the task-selection problem should not
"get bottlenecked on human data" as long as some form of verification exists, and it needs "some form
of ability to do curriculum learning". It also flags a surprise: "even though they only hill-climb on
self-proposed code tasks, they actually see strong performance on both coding and math benchmarks",
and "larger models get bigger gains relative to smaller models" (≈31:12).

## What is still open, in the lecturers' words

The lecture then states the three takeaways directly (≈32:44–33:32). To achieve "true generalization
and reasoning" we need **diversity in the reasoning chain**, and how to achieve it "continues to be
an open problem". We need **better verification**, and "the more we can break that verify loop in a
way where we are not bottlenecked by humans or tasks that are verified only through human experts,
the better off we are" — building reward models is challenging. And we need to solve **data
selection**: "there's only so much data that can be curated by humans for what prompts go in", and
breaking that barrier is important for the next generation of self-improving models.

### Verifiable domains, and everything else

The open question the instructors put first is how far self-improvement can be pushed **within
verifiable domains**, and how much that **generalizes** to domains where verification is not
automated — "can we generally make the model smarter and smarter in areas that we can verify and
minimize or remove the need for labeled data in non-verifiable domains based on that?" The lecture
calls that "a very compute-heavy research experiment" (≈34:18).

Examples of domains where verification is hard, and why (≈35:04): AI applications that do not fall
into verifiable problems include ones where **verification is slow** — scientific discovery, a chip
design simulation that takes days to run to collect one reward signal, or a chemical experiment that
requires going to a lab to see the quality or reward. The constraint is latency: in RL fine-tuning or
test-time scaling "we need these verifiers to be almost instant, or we can wait a little bit. Maybe
we can wait minutes. Maybe we can allow one hour." The reason is what RL training needs: "hundreds
or thousands of steps of iteration", against which "we can't wait like days or someone human in the
loop to collect the reward" (≈35:53). Truly non-verifiable also includes **creativity and subjective metrics** —
creative writing, where "designing an exact reward function for that is hard" — and where a modelled
reward lets "the RL or the agent can do reward hacking if the model is slightly off base" (≈36:41).

### A student's question: how do people attack the hard-to-verify domains?

A student asks how people are attempting to handle the harder areas, "like chip design or" — and
*the two words that follow are marked `[Ed: unclear]` in the edited transcript, which keeps the
captions' reading of them rather than guessing* (≈37:28). The answer the lecture gives is to
**train a separate reward model that predicts the outcome of a simulation**: collect a lot of data on
the expensive simulations or experiments offline, then train a reward model that, for a given input,
predicts the quality of the output, and use that reward model as the verification object in the
optimization loop (≈37:28). The caveat is stated immediately: "the generality of the reward model is
a function of how much data you have. And it can be inaccurate and that can cause problems"
(≈38:16).

Chowdhery adds a concrete instance from the class's own neighbourhood (≈38:16–39:49): before chip
design, there is **kernel** work, where **compiler execution** gives you an observable — did the
generated code produce the right result, and with what metrics? — but a class project is trying to
**read performance profiles** as the code becomes more complex, "and that's a harder problem". Even
when design-optimized kernels can be produced for simple enough things, concatenating them and
profiling to find and fix the parts that are not optimized is harder. What people do about it, in
her description, is **break the problem into subparts**, have the model look at each part, and
**give it reference solutions in a knowledge base** — which is, she says, literally describing a class
project: "because of this being a hard-enough problem, and the models not being at that capability,
often it requires breaking down the problem in interesting ways to what the models are able to do
now" (≈39:03). See [agentic workflows](agentic-workflows.md).

## Efficiency: intelligence per watt

The lecture then turns to its second half: "we can do the self-improvement. But you're now doing a
lot more inference. And there's a cost to this intelligence" (≈39:49). Mirhoseini presents work done
in collaboration with two colleagues the lecture names — "Professor Ray" and "Professor John Hennessy"
(*the captions' spelling of the first is kept; nothing in the course's own materials gives another*) —
and "a large team of great collaborators" (≈40:37). The work looks at trends on **both** the model side
and the hardware-accelerator side, to think about what AI inference workloads, model sizes and
accelerators will look like in the future.

### The demand side

The starting observation is that we are "in the mainframe era": the large models being used — via
ChatGPT, Gemini and similar — "all of them are being run on cloud", because they are large,
sometimes proprietary, and even the open ones are still served from cloud hardware (≈41:26). At the
same time, "the demand for compute as a result is exploding" (≈41:26):

- **Google Cloud grew "in something in the order of 1200x in the last 20 months"** in compute
  serving (≈41:26).
- **NVIDIA had "a 10x year-over-year kind of growth"** — "and this is crazy. These numbers are
  explicitly driven by AI" (≈42:13).
- Serving that demand "right now" needs "something in the order of 250 gigawatt of data centers",
  which means more and more **energy** supply, and the lecture says this is exploding too (≈42:13).
- On the number of tokens processed, the lecture gives a February figure of **160 trillion** and an
  October figure the captions render as **1.3 billion** (≈43:00) — *lower* than the February number
  rather than higher. The edited transcript keeps the caption as spoken and marks it `[Ed: unclear]`;
  no source this KB is allowed to draw on settles what the figure should be, and none is supplied.

The lecture's summary: "this is one of the fastest growing compute demands that we are seeing in
history" (≈43:00).

### What the queries look like

The study also looked at **what** is being asked (≈43:00–44:37). From "this very large-scale data set
of ChatGPT users", it turns out that "something in the order of **77% of requests** are for task[s]
like practical guidance, information… or writing" (≈43:47). For a lot of these tasks "we don't need
the very best frontier models to answer them correctly, rather smaller and local models can be used"
(≈43:47). The chart the lecture refers to shows the **categories of user queries over time**, and the
reading is that although users ask more complex questions as chatbots get better, "the vast majority
of the type of queries that are being asked are on the side of, again, the simpler side of
complexity that can be addressed by smaller models rather than large proprietary models"
(≈44:37).

### Why local hardware is now interesting

The third trend is **improvements in local inference accelerators** (≈44:37–45:26). Since 2012 the
lecture reports "something like **126x improvement** in the GPU memory of the local accelerators",
and notes that a laptop — "our MacBooks could have something in the order of **100 gigabyte of
memory**" — can now fit a very large model, especially if it is served in **quantized** versions. The
conclusion drawn is that some of the largest models out there can be served locally, which pairs with
the finding that a lot of user queries are addressable by smaller models (≈45:26).

### The metric: intelligence per watt

The question the work asks is what role **local inference** can play in **redistributing** inference
demand, since today "pretty much all of it is going to cloud" and to accelerators such as H100s, TPUs
and the newer device generations (≈45:26–46:13). To look at this systematically the work defines a
new metric that measures capability *and* efficiency together (≈46:13):

$$\text{intelligence per watt} = \frac{\text{average task accuracy}}{\text{average power draw to solve the task}}$$

On the **capability** side it looks at "the percentage of the queries that are addressable by the
model for the single turn and reasoning type of queries"; "local models" in this study means models
with **20 billion active parameters or less**; and on the **efficiency** side it looks at "how much
useful compute we can get from these per watt from running these models on our local hardware"
(≈46:59).

The study's scope (≈47:46): **more than 20 local models** including Qwen, GPT-OSS and Gemma3; both
**enterprise and local accelerators**; and workloads of **1 million queries** sourced from ChatGPT
plus reasoning benchmarks. The metrics collected include accuracy, energy, latency and compute
(≈48:32).

### The findings

Three findings, as the lecture states them (≈48:32–50:55):

1. **Local models are already good, and improving fast.** Since 2023 there was a **3.1x improvement**
   in accuracy, or in the portion of chat queries they could solve — "this is very, very fast. And
   this happened in only two years". The models could address **88.7%** of the queries the lecture
   described (≈49:19).
2. **Local accelerators lag enterprise chips on efficiency.** An **Apple M4 Max delivers 1.5x lower
   intelligence per watt than the B200** — because the B200 is extremely optimized for language-model
   workloads, whereas an Apple chip is optimized for other workloads too, and because "the
   understanding wasn't that the LLMs are going to be running locally" when those chips were designed
   (≈49:19–50:06).
3. **Intelligence efficiency improved 5.3x over two years**, and the lecture decomposes it: **3.1x
   from better models** and **1.7x from better hardware** (≈50:06–50:55).

The lecture's reading of the three together: local models are becoming better at an accelerated rate
while both model and hardware efficiency improve, which "suggests that we are heading towards this
future, that more and more of this traffic can be addressed, or can be solved by models that we can
run on our edge device, for example, on our laptop or on our phone device in the future" (≈50:55).

## The directions the lecturers name

The lecture closes with a list of what remains open, split between the self-improvement side and the
efficiency side.

**Foundations of test-time scaling** (≈51:42–52:31). The way the field approaches test-time scaling
now is "through RL, through collecting this data and then fine-tuning the models", but that it "kind of like
suggests that there is more here". The lecture's questions: why do we see this property at all —
"what does it suggest from the model that as we ask the model a question over and over again through
test time scaling, what is happening that these correct answers are coming out?" — and **what are the
best practices to distill these successful trajectories back to the model** (≈52:31). See
[test-time scaling](test-time-scaling.md).

**Continual learning** (≈53:19–54:51). The lecture draws the contrast with humans: "as we solve
tasks, as we solve problems and study and do new things, there's this continual kind of progress in
how our brain develops and how we become more and more skillful", whereas for models, "it seems like mostly there's this offline
process" of agents generating experiences, and then, "maybe after some time, there's
this fine-tuning process of the model. It is not something that happens on the go" (≈53:19). That
mismatch is what a concept like continual learning could address: "what are the new practices that we
can bring in model development that we can bring these positive experiences and learning from
negative experiences and problem solving that the model do[es] back into the model in a more natural
way that is different from the current asynchronous, like data generation and fine-tuning paradigm?"
(≈54:05).

**Infrastructure for high-throughput, low-latency test-time scaling** (≈54:51–55:39). Test-time
scaling as the course describes it — repeated sampling, back-and-forth updates to previous
generations, tool calling — "is very different from the current mainstream chatbot usage, which is
just mostly single turn back and forth with the model". That leaves "a lot of opportunities for doing
systems and inference optimization work for these type[s] of test scaling", work her lab has done,
and these methods are "becoming more and more mainstream", so the optimization will matter more
(≈54:51).

The lecture's summary of the whole picture (≈55:39–56:25): a lot of interesting work happens in
pre-training, which the course "touched less on", because it mostly focused on post-training and
test-time scaling methods — and there is now "this new kind of unleashed era of like synthetic data,
flywheel, and continual learning" at the junction of fine-tuning, online learning and test-time
scaling.

**Serving, energy and kernels** (≈57:14–58:01). The intelligence-per-watt observation suggests a
shift "from everything on cloud to a lot more locally", and the directions that follow: **hybrid
inference serving engines** that "smoothly route traffic between our local and cloud kind of models
and accelerators, depending on the need and the complexity of the resources"; **new model
architectures and kernels** for energy-efficient inference, especially on local accelerators, which the
lecture calls a direction "much less kind of focused on compared to the cloud accelerators"; and **energy itself**,
which "is going to be the most kind of valuable resource that we have going forward", making metrics
such as intelligence per watt, and better ways of measuring and optimizing power usage, "very, very
important" — an area "less worked on right now in terms of the target metric of optimization"
(≈57:14, ≈58:01).

### The class's questions

**Is continual learning a memory-systems problem or a model problem?** A student asks whether the
continual-learning idea from success and failure will show up in **long-term memory systems and
architectures** around the LLM, or **in the models themselves** (≈58:52–59:38). The answer starts
by granting the analogy — continual learning "might pertain to long term-memory systems, that would
be the human analog of it" — but pushes the question earlier: before that, even in multi-step
reasoning, learning from success or failure and "getting the model to keep that skill set and update
things in the weights, or in [a] subset of weights in a useful way" matters. "Learn how to learn is
an important capability that we basically don't have right now" — watching videos of a task and
learning to do it, "as opposed to being given a lot of task demonstrations" (≈1:00:23).

Mirhoseini offers **in-context learning** as another route (≈1:00:23–1:01:59): if a model had an
**infinite context** it could perfectly access, "maybe that was one solution to continual learning",
because everything positive and negative could sit in the context and the model could reason over all
of it at once — but we do not have that, and even at a million tokens the ability to reason over
in-context data diminishes (*the captions are unclear on the exact term here*). She points to
**cartridges**, covered earlier in the course, as another way of enabling long context and in-context
learning **without changing the weights** — bringing knowledge into the activations, or into the
model's cache, rather than fine-tuning. So there are ways to think about continual learning beyond
fine-tuning, one of them being "just increasing the effective context length massively" (≈1:01:59).

Asked which is easier in practice — **updating the weights or updating a memory store** — the answer
is that it depends on the application (≈1:02:48–1:03:34). "If I could keep a database that the LLM
could learn to look at, then I should just go update the database"; but the goal is teaching the LLM "the ability to reason over new domains. And oftentimes having a
side memory system doesn't quite achieve that. So that's where updating the weights does the job better". The example
given is from robotics: "no matter how much memory systems you add", cross-embodiment generalization
"doesn't happen if you don't update the weights" — it is "more of a skills transfer problem"
(≈1:03:34).

**Is there a paper on agents creating their own environments?** A student asks, referring to the
third paper, whether the environment — described there as the source of post-training data — can be
**self-created by agents** (≈1:03:34–1:05:07). The answer is that the question is really about the
**task distribution**: in the era of narrow intelligence you could create simulations for games and
use them as environments for toy tasks, and "the reason environments matter in the current generation
are that they are proxies for real-world tasks". So it is not "so much that there's a paper for
agents to self-create environments"; it is "what is the set of tasks that you're trying to represent",
and if there is an easy way to simulate them, "then whether you use agents to create that or software
to create that, that's fairly straightforward". What matters is whether the environment is a
"reasonable proxy of how the model will interact with the real world to get feedback" — and for gaming, "it's kind of a finite
space to explore", so simulation is easier (≈1:05:07). See
[agentic workflows](agentic-workflows.md).

## Closing remarks

The lecture ends with both instructors thanking the class (≈1:05:53–1:07:28). Chowdhery calls it "a
real pleasure teaching you all and creating the content for this class", and makes the point that the
field is moving so fast that "anything that we teach now starts to be history by the next time the
class rolls around. And yet the basic techniques that you're learning are extremely valuable over
time, because a lot of the new stuff still builds on top of the basic techniques" (≈1:05:53). She
points forward to the students' projects and posters. Mirhoseini adds that the instructors were
themselves learning while preparing the material, "because a lot of these topics are just like so
fresh and so new", and closes with the hope that "this is just the beginning" for the students'
research and work (≈1:06:40).

## The lecture's three papers

The lecture presents key ideas from three papers as directions for future work. **None of them is a
reading the course site lists** — row 20's "Paper Readings" column is empty, and none of the three
appears under any other row either — so this KB does not transcribe them, does not commit any figure
of theirs, and does not link them, exactly as it does not ingest papers a lecture merely cites.

| Where the lecture introduces it | How the lecture identifies it | Topic |
|---|---|---|
| ≈6:17 | "coming from multi-agent finetuning, which focuses on self-improvement with diverse reasoning chains" — no title, no URL | Diversity of reasoning chains; generation and critic agents trained by debate and majority voting |
| ≈14:53 | **DeepSeekMath-V2** — named as "a recent paper that came out"; no URL | Automated self-verification for theorem proving: a verifier trained to find issues without reference solutions, plus a meta-verifier that checks the verifier |
| ≈22:39 | Not named. "There's this very interesting paper that has not seen much use yet, but is quite promising" | A model that proposes its own coding tasks at the edge of its ability and solves them, with no human-curated prompts |

The class refers to the third as "the Absolute Zero paper" in a question at ≈1:03:34. A guest lecture
not in the catalog is said to have presented a Gemini-side counterpart of the second paper's problem
(≈20:23).

## Where this lecture's material comes from

Everything on this page is cited to the edited transcript. The lecture's own numbers, and the places
where the captions are ambiguous, are collected in the transcript's `[Ed: …]` notes and in
[`kb.json`](../kb.json)'s caveats; where the lecture reads a chart, this page reports what the
lecturer said about it rather than restating values from the chart.

## Related pages

- [Self-improvement](self-improvement.md) — the loop this lecture's first half pushes on: where the
  feedback comes from, and what breaks it.
- [Verifiers](verifiers.md) — outcome and process reward models, judges, and the meta-verifier this
  lecture adds on top.
- [Reinforcement learning for LLMs](reinforcement-learning.md) — where the reward comes from, and
  what RL does and does not improve.
- [The LLM training pipeline](llm-training-pipeline.md) — human-curated data, synthetic data and the
  data bottleneck.
- [Test-time scaling](test-time-scaling.md) — the repeated sampling this lecture says is still not
  understood.
- [Agentic workflows](agentic-workflows.md) — what an agent is, and the environments it acts in.
- [Scaling laws](scaling-laws.md) — loss versus compute, and the efficiency axis this lecture adds.
- [Course logistics](course-logistics.md) — the course's structure and the project the closing
  remarks point to.
