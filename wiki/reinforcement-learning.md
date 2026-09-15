# Reinforcement learning for LLMs

In this course, **reinforcement learning (RL)** means updating a language model — the *policy* — so that the
outputs it samples earn more reward, where the reward comes from outside the model's training text: human
preferences, AI feedback, tests, a known answer or a judge. [Lecture 6](06-train-time-scaling-scaling-rl.md) is
devoted to it, and it runs through lectures 1, 3, 4 and 5 as the way feedback becomes training. This page collects
the algorithms and the design choices in one place; the lecture pages have the details and results.

## One objective behind all of them

Every method in the course raises the log-probability of sampled outputs in proportion to some signal. STaR's paper
makes the simplest case explicit: with a reward of 1 when the answer is correct and 0 otherwise, the policy gradient
weights $\nabla \log p_M(\hat{y}, \hat{r} \mid x)$ — the log-probability of a sampled rationale $\hat{r}$ and answer
$\hat{y}$ for problem $x$ — by that reward, so only correct outputs contribute. That is exactly fine-tuning on
filtered correct samples (lecture 6, ≈19:29; Zelikman et al. 2022, §3.1).

DeepSeekMath generalizes this into one formula. The gradient of a training method is an expectation, over data from
a **data source**, of $\nabla_\theta \log \pi_\theta(o_t \mid q, o_{\lt t})$ for each token $o_t$ of an output $o$ to a
question $q$, multiplied by a **gradient coefficient** that the algorithm computes from a **reward function** (Shao et
al. 2024, §5.2.1, Table 10). Supervised fine-tuning has coefficient 1 on human-selected data. Rejection-sampling
fine-tuning samples from the SFT model and keeps correct answers; its online version samples from the current policy.
PPO and GRPO sample from the current policy and weight each output by a learned reward. In the paper's experiments
online sampling beat offline, and rewarding outputs by different amounts beat reinforcing every correct one equally
(§5.2.1, Figure 5).

## Where the reward comes from

- **Human preferences, through a reward model** — RLHF ([lecture 1](01-course-overview.md); see
  [the LLM training pipeline](llm-training-pipeline.md)).
- **AI feedback against written principles** — Constitutional AI's RLAIF, which replaces human harmlessness labels
  with a model's judgements ([lecture 4](04-learning-from-feedback-with-tools-code.md)).
- **Code execution** — RLEF rewards a final solution by whether it passes private tests (lecture 4).
- **A process reward model** — Math-Shepherd trains one on automatically labelled steps and uses it as the reward for
  step-by-step PPO ([lecture 3](03-robust-verification.md)).
- **A prompted LLM judge** — SWiRL scores every step of a multi-step tool-use trajectory with Gemini 1.5 Pro
  ([lecture 5](05-planning-and-multi-step-reasoning.md)).
- **A rule** — the known final answer. STaR keeps or discards; DAPO rewards 1 for an answer equivalent to the ground
  truth and −1 otherwise, choosing a rule over a reward model to avoid reward hacking (lecture 6; Yu et al. 2025,
  §2.4).

The choice sets the loop's limits. If the model is too capable, it hacks a learned reward; if the reward carries too
little signal, the loop cannot hill-climb (lecture 6, ≈1:04:40–1:05:25). Math and code are where RL works best
because their rewards are verifiable (≈7:05). See [verifiers](verifiers.md).

## PPO

**Proximal Policy Optimization** is the standard RL algorithm for RLHF (lecture 6, ≈43:48). It maximizes a clipped
objective: the ratio between the new and old policy's probability of each token, multiplied by that token's
**advantage** — how much better the outcome was than expected — with the ratio clipped to $[1-\epsilon, 1+\epsilon]$
so that one update cannot move the policy too far. The advantage comes from Generalized Advantage Estimation over a
**learned value function**, the critic, and a KL penalty toward a reference model is added to the per-token reward
(Shao et al. 2024, §4.1.1). The cost is memory: the lecture counts four models to hold — old policy, new policy,
critic and reward model (≈43:48–44:35).

In the course, PPO is Math-Shepherd's step-by-step RL against a process reward model (lecture 3), and RLEF's training
of a code model, where the policy acts per token but the value function works per turn (lecture 4).

## GRPO

**Group Relative Policy Optimization**, from DeepSeekMath, drops the critic. For each question it samples a group of
$G$ outputs, scores them, and gives every token of output $i$ that output's reward normalized within the group as its
advantage:

$$\hat{A}_ {i,t} = \frac{r_i - \operatorname{mean}(\mathbf{r})}{\operatorname{std}(\mathbf{r})}$$

where $\mathbf{r} = \lbrace r_1, \ldots, r_G\rbrace$ are the group's rewards. The KL penalty moves out of the reward
and into the loss (Shao et al. 2024, §4.1). Normalizing within a group suits reward models, which are trained on
comparisons anyway, and dropping the critic saves the memory that lets RL scale (lecture 6, ≈44:35–45:22). It raised
DeepSeekMath-Instruct 7B from 46.8% to 51.7% on MATH (≈46:09; Table 5). [Lecture 5](05-planning-and-multi-step-reasoning.md)
names RL methods such as GRPO as the open direction for SPRINT, which is trained with supervised fine-tuning (≈49:17).

GRPO's weakness is the group itself. If every sample in a group is right, or every one wrong, the normalized rewards
are zero and there is "nothing for the model to learn" — so problems that are all too easy or all too hard give no
signal (lecture 6, ≈48:25–49:15).

## DAPO: what it takes at scale

Naive GRPO on Qwen2.5-32B reached 30 points on AIME 2024, against 47 for DeepSeek's own RL run on the same base model,
with entropy collapse, reward noise and unstable training (lecture 6, ≈53:06; Yu et al. 2025, §1). DAPO's four changes
brought it to 50 (Table 1):

1. **Clip-Higher** — separate lower and upper clipping ranges, with a higher upper one, so low-probability
   "exploration" tokens can grow and entropy does not collapse (§3.1).
2. **Dynamic Sampling** — oversample, and drop groups whose samples are all correct or all wrong, so every question in
   the batch carries gradient (§3.2). This is the fix for GRPO's weakness above.
3. **Token-Level Policy Gradient Loss** — average the loss over all tokens in the batch rather than within each sample
   first, so long responses carry their full weight and their gibberish is penalized (§3.3).
4. **Overlong Reward Shaping** — stop penalizing sound reasoning merely for being truncated, and add a gradual length
   penalty near the limit (§3.4).

DAPO also removes the KL penalty, because a long-chain-of-thought model is meant to move far from its starting point,
and uses the rule-based reward above (§2.3–2.4). The lecture's lesson is the third of its opening insights: in RL,
"the small fixes can be extremely important" at scale (≈3:56). It adds what to monitor instead of the loss — response
length, entropy, and the share of samples already at accuracy 1 (≈1:00:03).

## Outcome versus process rewards

An **outcome** reward scores only the final answer; a **process** reward scores each step
([lecture 3](03-robust-verification.md) introduces the two as outcome and process reward models). The course has both
inside RL. RLEF uses a binary outcome, and whether harder problems need feedback at every step is left open
(lecture 4). Math-Shepherd's step-by-step PPO beat PPO with an outcome reward model (lecture 3). DeepSeekMath found
GRPO with process supervision better than with outcome supervision (Shao et al. 2024, §5.2.1, Figure 5). SWiRL rewards
every step of a multi-step trajectory (lecture 5).

## RL versus supervised fine-tuning

The lectures agree on the direction and differ on the reason:

- **RLEF** ([lecture 4](04-learning-from-feedback-with-tools-code.md)): RL beats supervised fine-tuning on filtered
  rollouts.
- **SWiRL** ([lecture 5](05-planning-and-multi-step-reasoning.md)): multi-step RL beats SFT on the same data, and the two
  want differently filtered data. SFT imitates what it is shown, so wrong answers hurt it; RL learns best from
  trajectories with sound steps, whatever the answer.
- **Lecture 6**: RL hill-climbs with fewer examples where the reward is strong but takes a lot of work to get right;
  SFT on plenty of good data is often faster but does not bring or boost reasoning in the same way (≈1:00:52–1:01:37).
  Distilling from a stronger model is the practical alternative, which the course does not cover (≈33:35–35:55).

## What RL improves

DeepSeekMath's analysis found that RL raised majority-vote accuracy, $\text{Maj@}K$, but not $\text{Pass@}K$: correct
answers became more likely among the samples, while problems the model could not solve at all stayed unsolved (Shao
et al. 2024, §5.2.2, Figure 7; lecture 6, ≈52:19). The lecturer's summary is that RL gets better at what a model can
already do, exploring "the design space of what it knows" (≈1:10:06–1:10:52), and that step jumps in capability have
come from breakthroughs or scaling (≈1:04:40). Lecture 1 left the same question open: why RL gives such a jump if the
ability is already in the pre-trained model is "not completely well understood" ([lecture 1](01-course-overview.md),
≈52:12; see [self-improvement](self-improvement.md)). Learning from failures, and algorithms robust to noisy rewards,
are among lecture 6's open problems (≈1:06:57).

## Around search

[Lecture 7](07-self-improvement-and-deep-research-agents.md) touches RL at three points. AlphaCode fine-tunes
with **GOLD**, "an offline RL algorithm which allows the model to both learn from tokens it already assigns high
likelihood to, and to ignore tokens that are not in its distribution". It was adopted because a problem has
many correct solutions and only one needs to be found (Li et al. 2022, §4.3); AlphaCode 2 fine-tunes Gemini Pro
with the same objective (AlphaCode Team 2023, *Policy and Fine-Tuning*). The lecturer suggests RL as a way to
cut test-time sampling, since a model that gets better at solving problems in an RL loop needs fewer samples
(≈33:04). And the lecture contrasts Search-o1's prompting-based search with **Search-R1**, which teaches a model
to search with RL; that paper is not on the reading list (≈1:08:19). The closing Q&A also mentions efforts to
train calibration with RL or RLHF (≈1:10:41).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): RLHF, and the open question of why RL works.
- [Lecture 3 — Robust Verification](03-robust-verification.md): Math-Shepherd's step-by-step PPO with a process reward
  model.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md): RLEF's PPO on
  execution feedback, and Constitutional AI's RL from AI feedback.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md): SWiRL's step-wise RL with a
  judge, and RL versus SFT on multi-step data.
- [Lecture 6 — Train Time Scaling/Scaling RL](06-train-time-scaling-scaling-rl.md): STaR as bare-bones RL,
  DeepSeekMath's GRPO and unified view, and DAPO's fixes for RL at scale.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md): GOLD
  as offline RL in AlphaCode's fine-tuning, RL to reduce test-time sampling, and Search-R1 versus Search-o1.
