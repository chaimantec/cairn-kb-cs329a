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

## Step-wise RL for multi-step tasks

RLHF, RLAIF and RL from execution feedback reward a single final response.
[Lecture 5](05-planning-and-multi-step-reasoning.md) presents **SWiRL**, which rewards **each step** of a
multi-step trajectory instead. A generative reward model scores every action, whether a reasoning step
with a tool call or the final answer, given the context before it, and the policy is optimised on these
step-wise rewards using offline data (lecture 5, ≈52:24–1:04:18; Goldie et al. 2025, §1, §2.2). On the
same data, multi-step RL beat supervised fine-tuning by a good amount, and the two did best on
differently filtered data. SFT did best on trajectories with sound steps **and** correct answers; RL did
best on trajectories with sound steps, whatever the answer. The lecturer's explanation is that SFT imitates
what it is shown, while RL gives the model a new chance to act within the prior steps and rewards that
action (≈1:12:15–1:13:02; §4.2, Figure 5). SPRINT, from the same lecture, is trained with supervised
fine-tuning, and the lecturer names RL methods such as GRPO as its open direction (≈49:17).

## Preparing a model for RL, and choosing between SFT and RL

[Lecture 6](06-train-time-scaling-scaling-rl.md) shows the stages feeding each other in DeepSeekMath. Before any RL
the model is primed in its domain: continued pre-training on 120B math tokens curated from Common Crawl, starting from
a code model because code training helped math reasoning, and without leaning on arXiv papers, which the paper found
brought no notable improvement (lecture 6, ≈41:27–43:01; Shao et al. 2024, §2, §5.1). Instruction tuning on 776K math
examples follows (§3.1), and only then RL with GRPO, which raised MATH accuracy from 46.8% to 51.7% (≈46:09; Table 5).
If a model is weak in a domain, its capability there has to be built before RL can help (≈43:01).

On the choice between the last two stages, the lecturer's answer is that RL lets a model hill-climb with fewer examples
where the reward signal is strong, but takes a lot of work to get right; supervised fine-tuning on plenty of
high-quality data is often faster, but does not bring or boost reasoning in the same way (≈1:00:52–1:01:37). Asked how
much of a frontier model's training is now RL, the lecturer recalls about 1% against 99% for pre-training a year
earlier, and perhaps 5% now (≈1:08:30). The RL algorithms themselves — PPO, GRPO and DAPO — are on
[reinforcement learning](reinforcement-learning.md).

## What came next

Pre-training and fine-tuning "were the big pieces" until about a year and a half before the lecture,
when inference became a frontier too (≈19:21), and test-time generation began feeding synthetic data
back into fine-tuning (≈28:53). See [test-time scaling](test-time-scaling.md) and
[self-improvement](self-improvement.md).

## Where the training data comes from, and where it could come from instead

Every stage above is fed by data that a human chose: human-curated reasoning traces for supervised
fine-tuning, and, for reinforcement learning with verifiable rewards, question–answer pairs written by
experts ([lecture 9](09-future-research-areas.md), ≈23:25). The lecture states the cost of that
directly — a math model needs math experts, an IMO benchmark needs IMO experts, coding needs strong
software engineers — and draws the consequence: "as the models continue to surpass human
intelligence, the ability to find more and more experts and more and more such tasks starts to be
limiting" (≈24:12).

The direction it presents instead is a **model that proposes its own training tasks and then solves
them**, so that "we should not really need an external source of data" and should not need
human-generated prompts to climb on (≈24:12). The paper's setting is coding, and its tasks come in
three forms (≈24:59–26:31): **deduction**, where the model writes a program and an input and the
environment executes it to get the output; **abduction**, the same with a different emphasis; and
**induction**, where it samples an *existing* program, generates new inputs for it and a
natural-language description of what it does, and lets the environment decide whether the direction is
right. The proposer is conditioned on past examples so that the tasks stay diverse, and the triplets
(program, input, output) accumulate in a **buffer** that the proposal samples from — which the lecture
reads as "this idea of curriculum learning that is evolving over time" (≈26:31–28:52).

What makes it trainable is the reward on the proposer: a task whose solver success rate is zero earns
zero, and one with a non-zero rate earns **$1$ minus the average success rate**, so the tasks that
survive are those that are "not trivial and… not impossible" (≈27:18). Validation runs before a
proposed task enters the pipeline — program integrity, safety checks, and checking that repeated
executions give identical outputs (≈28:04) — and the results the lecture reports are state of the art
on coding benchmarks "even though they didn't have any human-curated data on the prompt side",
outperforming models trained on tens of thousands of expert examples, with complexity and diversity
both rising as training proceeds (≈29:40). The full method is on
[reinforcement learning](reinforcement-learning.md) and [self-improvement](self-improvement.md).

## Continual learning, and the alternative to fine-tuning

The lecture's largest open direction is a mismatch between how humans and models learn
([lecture 9](09-future-research-areas.md), ≈53:19). Humans improve continuously — "as we solve tasks,
as we solve problems and study and do new things, there's this continual kind of progress in how our
brain develops" — while models mostly learn in "this offline process" of generating experience and
then fine-tuning, "not something that happens on the go" (≈53:19–54:05). The lecture asks what
practices could bring a model's positive and negative experiences back into it "in a more natural way
that is different from the current asynchronous, like data generation and fine-tuning paradigm"
(≈54:05).

Two answers are discussed. One is the human analog — **long-term memory systems** — and the lecture
notes the limit of a side memory: "if I could keep a database that the LLM could learn to look at,
then I should just go update the database. But what you're really trying to teach the LLM is the
ability to reason over new domains. And oftentimes having a side memory system doesn't quite achieve
that. So that's where updating the weights does the job better" (≈1:02:48–1:03:34). The example is
from robotics: cross-embodiment generalization "doesn't happen if you don't update the weights just by
having memory systems", which makes it "more of a skills transfer problem" (≈1:03:34).

The other is to **not change the weights at all** and extend what the context can hold. Mirhoseini's
framing is an **infinite context** the model could perfectly access — "maybe that was one solution to
continual learning", because everything positive and negative could sit in the context and be reasoned
over at once — which we do not have (≈1:00:23–1:01:12). She points to **cartridges**, covered in the
course's guest lecture on memory, as a way of enabling long context and in-context learning "without
changing the weights of the model, without fine-tuning the model", by bringing the material into the
activations or the model's cache instead (≈1:01:59).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): the full pipeline, ≈11:39–19:21.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md):
  RLHF's labelling cost and Constitutional AI's RLAIF.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md):
  SWiRL's step-wise RL for multi-step tool use, and why RL and SFT want differently filtered data.
- [Lecture 6 — Train Time Scaling/Scaling RL](06-train-time-scaling-scaling-rl.md): DeepSeekMath's math
  pre-training and instruction tuning before RL, and SFT versus RL.
- [Lecture 9 — Future Research Areas](09-future-research-areas.md): the human-expert bottleneck on
  training prompts, a model that proposes and solves its own tasks, and continual learning against the
  generate-then-fine-tune paradigm.
