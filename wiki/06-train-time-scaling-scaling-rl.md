# Lecture 6 — Train Time Scaling/Scaling RL

This lecture closes the loop the course has been building since [lecture 2](02-test-time-compute-scaling.md):
take what a model produces, keep what a verifier or a known answer says is good, and train the model on it
(≈0:05, ≈5:29). That is **train-time scaling**, and its central tool is reinforcement learning. Three
readings show how the recipe evolved. *STaR* (2022) bootstraps reasoning with a bare-bones loop: a model
writes rationales, keeps those that reach the correct answer, and is fine-tuned on them; for problems it
fails, it is shown the answer and asked to explain it. *DeepSeekMath* (2024) first builds a strong math
base model from curated web data, then introduces **GRPO**, a variant of PPO that drops the critic and
scores each sampled answer against the rest of its group. *DAPO* (2025) makes GRPO work for long chains of
thought at scale, with four fixes to the RL algorithm that take a 32B model from 30 to 50 points on AIME
2024.

The lecture's takeaways are that a model can learn from its own filtered outputs, that the compute spent
doing so can substitute for model parameters, and that in RL small implementation details decide whether
training works at all (≈3:11–3:56). It ends on what RL does *not* yet do: in DeepSeekMath's analysis it
makes a model more consistent, not fundamentally more capable (≈52:19, ≈1:03:08).

The captions do not name the lecturer, so this page does not either.

[Edited transcript](../raw/transcripts/06-train-time-scaling-scaling-rl.md) ·
[verbatim captions](../raw/transcripts/original/06-train-time-scaling-scaling-rl.md) ·
[video](https://www.youtube.com/watch?v=yVnmHSAy3ck) ·
[course website](https://cs329a.stanford.edu/) · [sources](../sources.md)

> **Numbering.** This is catalog position 6 ("Part 6 | Train Time Scaling/Scaling RL") and site schedule
> row 6, "Train Time Scaling/Scaling RL" (Fri Oct 10). The mapping is confirmed by the transcript, which
> discusses all three readings the site lists for row 6, in order: STaR (≈15:37–40:41), DeepSeekMath
> (≈40:41–53:06) and DAPO (≈53:06–1:03:08).

## Readings

The course publishes no slides. Its course material is the three papers the course site lists for this
lecture. Licences were checked on each arXiv abstract page: **all three carry arXiv's non-exclusive
licence**, which does not permit republishing their text or figures. They are linked, discussed and cited
here by section, figure, table and equation; none is transcribed, and this page has no images.

| Reading | In this KB | Where the lecture covers it |
|---|---|---|
| Zelikman et al. (2022), [STaR: Bootstrapping Reasoning With Reasoning](https://arxiv.org/pdf/2203.14465) | linked only (arXiv non-exclusive licence) | ≈15:37–40:41 |
| Shao et al. (2024), [DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models](https://arxiv.org/abs/2402.03300) | linked only (arXiv non-exclusive licence) | ≈40:41–53:06 |
| Yu et al. (2025), [DAPO: An Open-Source LLM Reinforcement Learning System at Scale](https://arxiv.org/abs/2503.14476) | linked only (arXiv non-exclusive licence) | ≈53:06–1:03:08 |

Section, figure and table numbers below are those of the arXiv versions consulted. Numbers on this page
come from the papers' text and tables, never from reading a chart.

## Why train-time scaling

### Small models, high scores

The lecture motivates the day with AIME, a benchmark of competition math problems harder than the MATH
benchmark the course used earlier, which is "already saturated and contaminated" for most models because
they have been trained on it. Homework 1 uses AIME 2024 and 2025 (≈0:52–1:39). The lecturer's chart puts
GPT-3.5, believed to have 175 billion parameters, at almost 5%; DeepSeekMath, "using train time scaling on
7B model", at 51.7%, and at 60% with additional tricks; and DAPO on a Qwen 32B model at 50% (≈1:39–2:25).
The point is that reasoning benchmarks no longer track parameter count: smaller models reach high accuracy,
and the lecture sets out to explain how (≈2:25).

The readings place two of those numbers on a different benchmark. DeepSeekMath's 51.7% and its 60.9% with
self-consistency over 64 samples are on **MATH**, not AIME, and the paper reports no AIME score (Shao et
al., abstract, Table 5). In the same table GPT-3.5 scores 34.1% on MATH. DAPO's 50 points are on AIME 2024,
as the lecture says (Yu et al., abstract).

### Three insights and one loop

Reinforcement learning on reasoning "will not work well" if done naively, usually because the
implementation details are hard to get right (≈3:11). The lecture's three insights (≈3:11–3:56):

1. A model trained on the internet can improve further by **learning from its own outputs, filtered
   cleverly** — which is why the class is called self-improving AI agents.
2. The compute spent training a model on its own outputs **can substitute for model parameters**, as
   compute spent on more data for smaller models already has. See [scaling laws](scaling-laws.md).
3. In RL, unlike supervised learning, **small fixes become extremely important** as the algorithms scale.

It sets these against the paradigms from [lecture 1](01-course-overview.md): pre-training on the internet;
fine-tuning, including RLHF or RLAIF on preference data, which gives chatbots; and test-time scaling, such
as the majority voting students evaluate in homework 1 (≈3:56–4:42). Train-time scaling joins the last two:
take the model's outputs after test-time scaling has filtered them, and fine-tune on them. "If you take
nothing away from this entire lecture, but you just remember this loop, then you have learned the basics
of what train time scaling does" (≈4:42–5:29). A student asks how to balance compute between train time and
test time; the lecturer holds the question for the third paper (≈5:29).

### o1's two curves, and why math

OpenAI's o1 chart shows $\text{pass@}1$ accuracy on AIME rising log-linearly both with **train-time compute**
(left) and with **test-time compute** (right) (≈6:15–7:05; the same release appears in
[lecture 1](01-course-overview.md)). Together they allow a circular loop: generate outputs and feed them back
to improve the model. It works in math because math is **verifiable** — you can tell which outputs are
correct and choose those for training — and it works better wherever verification exists (≈7:05). See
[verifiers](verifiers.md).

A student notes that on the chart test-time compute seems to buy more than train-time compute. The lecturer
says there is no intuition that one should be better than the other, that this is one instance they wanted
to show, and — "I copied the plot" — that published graphs are not always right (≈10:57–12:29).

### What reasoning looks like

Reasoning models spend tokens spelling out a step-by-step process before answering (≈7:52; the captions
garble the model names in this sentence). The patterns in those tokens are **problem analysis**, **task
decomposition**, **self-evaluation**, **backtracking** to an earlier step, and trying **different
approaches**, "a somewhat parallel search" (≈7:52–8:37). The examples come from an early o1-series model: a
script request to print the transpose of a matrix given as a string, where the model first works out the
input and output formats and then decomposes the job into parsing, building the matrix, transposing and
printing; and a chemistry question about pH, where it stops mid-calculation — "wait, the correct formula is
different" — and corrects itself (≈8:37–10:10). Lecture 1 walks through the same transpose example; see
[reasoning models](reasoning-models.md).

Where verification is available — computer programming, data analysis, mathematical calculation — people
preferred the thinking model's output over GPT-4o's more than half the time; personal writing and text
editing gained much less (≈10:10–10:57).

### Test time versus train time

Test-time compute is cheap to add once a model is trained: with a good verifier you can sample and search
almost without limit, and repeated sampling tells us a correct solution exists (≈12:29–13:16). The lecturer
previews papers for "next Friday" that make search effectively infinite, naming AlphaCode (≈12:29; the
captions mark the name as uncertain). The site lists *Competition-Level Code Generation with AlphaCode* under schedule
row 8, Self improvement with Search & Deep Research Agents, on Fri Oct 17 — the Friday after this lecture — and
[lecture 7](07-self-improvement-and-deep-research-agents.md) covers it. Train-time scaling is harder in two ways: it has to be scaled
correctly, and the closed feedback loop needs **enough successes** to learn from (≈13:16).

The two serve different goals. A pre-trained model has some level of capability in the domains it has seen.
Test-time scaling reasons over many traces and picks one, which works when the verification is robust —
"like saying, I'm going to throw spaghetti at the wall, and I know where it should land." Train-time scaling
teaches the model, raising $\text{pass@}1$ so a correct output is more likely, and it still needs
verification in the loop (≈14:02–14:49).

Asked whether training on hard problems makes a model worse at easy ones, the lecturer says typically not,
unless the reasoning chains break — for example the repetitive chains called **overthinking** (≈14:49–15:37).
The question comes back during DeepSeekMath.

## STaR: Bootstrapping Reasoning With Reasoning (Zelikman et al., 2022)

Paper: [arXiv 2203.14465](https://arxiv.org/pdf/2203.14465) (linked only). The lecturer notes it was done by
an author at Stanford (≈0:05).

### Where do rationales come from?

Chain of thought makes a model interpretable and improves its reasoning (≈16:24; see
[chain of thought](chain-of-thought.md)). The goal is a model with strong step-by-step reasoning on any
problem, and the existing routes to one all fall short (≈16:24–17:09):

- Internet-scale data rarely contains reasoning steps.
- Annotating steps by hand is very expensive.
- Generating them from known solution patterns works only in very specific domains.
- Few-shot prompting with a handful of worked rationales still underperforms a model fine-tuned on a
  larger dataset *without* rationales.

The paper makes the same case: rationale datasets built by hand are expensive, template-based ones only work
where a general solution is already known, and few-shot rationale prompting "substantially underperform[s]"
models fine-tuned to predict answers directly (Zelikman et al., §1).

### The loop

STaR's key insight "is very simple" (≈17:09). Start with a small set of examples that have rationales, and a
large dataset of problems that have only answers. Few-shot prompt the model to write a rationale and an
answer for each problem, **keep only the rationales that reached the correct answer**, fine-tune on them, and
repeat (≈17:54, ≈25:45–26:33). What is new compared with test-time scaling is that only correct answers are
kept (≈17:54).

In the paper's notation, $M$ is a pretrained language model, $\mathcal{D}$ a dataset of problems $x_i$ with
answers $y_i$, and $\mathcal{P}$ a small prompt set of problems with rationales, with $P \ll D$ (for example
$P = 10$). The model produces a rationale $\hat{r}_ i$ and an answer $\hat{y}_ i$ for each problem, and only
those with $\hat{y}_ i = y_i$ are kept (§3.1, Algorithm 1). Two details the lecture does not mention: each
iteration fine-tunes the **original** pretrained model on the newly collected data rather than continuing to
train one model, to avoid overfitting, and the loop repeats until performance plateaus (§3.1).

### Rationalization

Fine-tuning only on correct examples stalls: the problems the model cannot solve give it no signal, so it
cannot learn to solve new ones (≈17:54–18:42). STaR's fix is **rationalization**: give the model the correct
answer as a hint and ask it to explain its way there — "the answer is 42" — then fine-tune on the problem,
the rationale and the answer **without the hint**, as if the model had solved it directly. This widens the
training set to harder problems, so reasoning can bootstrap iteratively instead of staying limited to what
the model could already solve (≈18:42–19:29, ≈26:33).

The lecture's prompt example is the paper's Figure 2: *Where do you put your grapes just before checking
out?*, with the hint that (b) grocery cart is correct (≈24:57; §3.2). The paper describes the model as
reasoning backward from the answer, and notes a second benefit, a larger dataset (§3.2).

### An RL algorithm, bare-bones

The lecturer calls STaR "very bare bones" and says "you can almost call it an off-policy reinforcement
learning technique" (≈19:29). The paper frames the loop as an approximation to a policy-gradient objective.
With the model viewed as first sampling a latent rationale $r$ and then an answer, and a reward that is 1 when
the answer is correct and 0 otherwise, the expected reward over the dataset is

$$J(M, X, Y) = \sum_i \mathbb{E}_ {\hat{r}_ i, \hat{y}_ i \sim p_M(\cdot \mid x_i)} \mathbf{1}(\hat{y}_ i = y_i)$$

and its gradient weights $\nabla \log p_M(\hat{y}_ i, \hat{r}_ i \mid x_i)$ by that same indicator, so every
rationale that misses the answer contributes nothing — which is exactly STaR's filter (§3.1, Equations 1–2).
STaR approximates this by greedy decoding and by taking several gradient steps on each batch. The paper
reserves the off-policy description for **rationalization**, which samples from the hint-augmented model as a
proposal distribution (§5).

### The assumptions

The lecture names three (≈19:29–21:02):

- **Correctness of the final answer is a proxy for the quality of the reasoning.** They found this generally
  true in math. Filtering out incorrect answers removes lower-quality chains and gives higher-quality data,
  but it also means never learning from incorrect chains.
- **Given the answer as a hint, the model can produce a valid reasoning path.**
- **The initial model is strong enough to bootstrap from few-shot examples.** If the problems are far beyond
  it, STaR makes little progress, which is where iterating matters.

The paper's own limitations match the third: few-shot performance must be above chance for the first
iteration to succeed, and GPT-2 could not bootstrap even on arithmetic. It adds that tasks with high chance
performance, such as binary decisions, yield many poor rationales (§6).

### Questions on the method

- *Does STaR start from a dataset with correct answers?* Yes — everyone starts from a benchmark (≈21:02).
- *Is anything filtered in the rationalization step, in case the reasoning to a given answer is wrong?* Not
  in STaR. Follow-on papers do, for example with a process reward model over the steps; evaluating reasoning
  quality without relying on the final outcome is offered as a project idea. The course tries "to show you
  papers as they evolved" (≈21:02–21:48, ≈24:10–24:57).
- *What if a problem is too hard even with the answer?* That is the assumption that some subset of problems is
  within the model's range; iterating may bring the rest within reach later (≈22:33).
- *Can the failed attempts be used?* "Learning from negative examples has not been nailed. Learning from
  positive examples has been." Any non-zero reward lets you close the RL loop; some papers learn from negative
  examples, but the lecture does not cover them (≈23:24–24:10).

### Setup

The experiments use **GPT-J**, a 6-billion-parameter open-source model (≈26:33; §4.1). Training has a small
warm-up and then a constant learning rate, and the number of inner-loop training steps grows with each
outer-loop iteration, because a slower start helps (≈27:20). The paper's values: a 100-step warm-up, 40
training steps in the first outer loop, and 20% more with each loop (§4.1). The three task families are
multi-digit **arithmetic** (a synthetic dataset), **CommonsenseQA**, multiple-choice questions about everyday
situations, and **GSM8K**, grade-school math word problems, "about 9k samples" (≈27:20–28:09). In the paper,
GSM8K has 7,473 training and 1,319 test problems; CommonsenseQA has 9,741 training, 1,221 development and 1,285
test questions; and each arithmetic iteration samples 10,000 problems from 50,000 generated (§4.1–4.2).

### Results

**CommonsenseQA.** Rationalization lets STaR reach **72.5%** while using only about 86% of the training
data, where fine-tuning directly needs much more data for its accuracy (≈29:43–30:28). The paper's Table 1
gives the comparison on the development set: STaR with rationalization 72.5% on 86.7% of the training data
(78.2% from rationale generation plus 8.5% from rationalization); STaR without rationalization 68.8% on 69.7%;
GPT-J fine-tuned to answer directly 60.0%; few-shot chain-of-thought GPT-J 36.6%; few-shot chain-of-thought
LaMDA 137B 55.6%; and GPT-3 fine-tuned directly, 30 times larger, 73.0% (§4.4, Table 1).

At ≈28:09 the lecture quotes STaR at "51.7%" while using "70% to 87% of the data". The data range matches
Table 1, but 51.7% is not a STaR result; it is the figure the lecture gives for DeepSeekMath at ≈1:39.

**Human evaluation.** Human raters compared STaR's rationales on CommonsenseQA with the few-shot ones, and the
qualitative analysis found them reasonable — expected, since CommonsenseQA is everyday natural language
(≈28:56–29:43). In the paper, crowdworkers were 30% more likely to rank STaR's rationales above few-shot ones
($p=.039$), and 74% more likely to prefer them over human-written rationales ($p \lt .001$), which the authors
say reflects how hard it is to elicit good rationales rather than human-level performance (§4.4).

**GSM8K.** Here rationalization did not improve performance much, and fine-tuning directly on good data
improves the model similarly (≈30:28–31:14). One reason given is the number of calculation steps: the model's
own reasoning used about as many steps as the provided ones, and when a problem is simple, making the model
reason does not help much (≈31:14). The paper reports 10.7% with rationalization and 10.1% without, against
5.8% for direct fine-tuning and 3.1% for few-shot chain of thought, on 28.7% and 25.0% of the training data
(§4.5, Table 2). The model's number of calculation steps matched the human solution's 53–57% of the time, and
when they differed the model usually used fewer (§4.5, Figure 6).

**Plateaus.** Because STaR is "not really true RL", multiple iterations eventually plateau, and how many to run
takes tuning (≈28:09–28:56). This was "the first signs of life that was shown for reasoning" on a small model
(≈30:28).

The paper's arithmetic results, not discussed in the lecture: 89.5% after 16 iterations against 76.3% for a
baseline trained on 10,000 examples without rationales, and with rationalization 2-digit addition rose from
under 1% to 32% after one iteration (§4.3, Figure 4). Adding 9- and 10-digit problems late in training, the
model solved many such out-of-distribution problems, though training became less stable (§4.3, Figure 5).

### Takeaways and limits

Rationalization conditions on the answer, and with the answer in view the model may produce a better
rationale; outputs kept without rationalization are the examples the model was already confident about
(≈32:01). The paper puts the same point formally — rationale generation samples from $p(r \mid x)$, while
rationalization samples from $p(r \mid x, y)$, which may be a better search space — and notes that at low
sampling temperature the confident examples give a weaker gradient signal (§5). Few-shot prompts help the
model learn how to write rationales but can bias their style toward the prompts' formatting, which then shapes
what the model trains on (≈32:01–32:46; §5, "Few-shot Prompting").

Overall, bootstrapping rationales gives high sample efficiency without humans writing reasoning chains, and
applies to symbolic, natural-language and mathematical problems (≈32:46). Its weak point is evaluation: most
tasks have no good way to check a rationale without a human or a process reward model, and filtering only on
the final answer lets invalid intermediate steps through (≈32:46–33:35). The paper also tried higher sampling
temperature as an alternative to rationalization and found it counterproductive: more correct answers reached
through incorrect reasoning (§5, "Temperature").

### Why not distil from a bigger model?

A student argues that to train a small model you would simply distil from a frontier model. The lecturer
agrees that distillation is the practical route and is not covered in the course, and says the point here is
different: to take a model's own outputs and improve that model, to understand what building it from scratch
takes, and to improve capabilities the model does not have (≈33:35–35:55).

### V-STaR and Quiet-STaR

Two follow-ups are mentioned but are not readings. STaR uses the known answer as a stand-in for a verifier;
adding a trained verifier to the loop, with generator and verifier trained together, is **V-STaR**.
**Quiet-STaR**, as the lecture describes it, moves the reasoning steps out of language into the model's latent
space (≈36:43–37:29).

### Discussion: what bounds STaR?

The class discusses what limits STaR's performance (≈37:29). Students suggest that STaR cannot make logical
leaps beyond the kind of reasoning in its training data (≈38:19); that it is bounded by the model's reasoning
ability, since a model that cannot reason even with hints produces no rationales to learn from (≈39:06); and
that some answers are easier to rationalize than others — an answer of 225 suggests multiplying 15 by 15 — so
problems the model must rationalize may need more data (≈39:54). The lecturer calls these valid and asks the
class to keep the underlying question in mind for RL: "a lot of the magic in this particular domain is like,
what can the base model do as we're building on top of that?" (≈39:54–40:41).

## DeepSeekMath (Shao et al., 2024)

Paper: [arXiv 2402.03300](https://arxiv.org/abs/2402.03300) (linked only). The lecture covers two of its
contributions — the data behind the base model, and the GRPO algorithm — and its analysis of what RL improves.

### A 7B model at the top of the MATH chart

On the MATH benchmark, top-1 accuracy had risen over time with model size, and then a 7B model, DeepSeekMath,
did very well. The main leap, the lecture says, was getting the reinforcement-learning part of the train-time
loop right (≈40:41–41:27). The chart is the paper's Figure 1, top-1 accuracy of open-source models on MATH
without tools or voting.

### Data first

Before RL comes priming the model in the domain, which answers a point from the STaR discussion: if a model is
weak in a domain, its capability there has to be improved first (≈43:01). Earlier work improved PaLM on STEM by
training on a lot of science and math data, typically arXiv papers (≈41:27; the captions do not name that
paper). DeepSeek's findings, as the lecture presents them (≈42:13–43:01):

- Training on arXiv papers "is not the trick".
- Start from **DeepSeek-Coder**, a model already improved on code: code-to-math training helped significantly,
  because it helped the model reason and use tools better. The lecture adds that this was the first time it had
  been shown.
- Curate math content from **Common Crawl** web pages: mining it with OpenWebMath gave far better coverage across
  math domains and a much larger yield of tokens.

The paper's numbers. The **DeepSeekMath Corpus** is 120B math tokens from 35.5M web pages, collected in four
iterations by a fastText classifier first trained with OpenWebMath as positive examples; it is almost 7 times the
size of Minerva's math web pages and 9 times OpenWebMath, and pages matching benchmark questions were filtered
out (Shao et al., §1.1, §2.1). **DeepSeekMath-Base 7B** is initialized from DeepSeek-Coder-Base-v1.5 7B and
trained for 500B tokens — 56% from the corpus, 4% AlgebraicStack, 10% arXiv, 20% GitHub code and 10% natural
language (§2.3). It scores 36.2% on MATH, above Minerva 540B's 33.6%; Minerva builds on PaLM and is 77 times
larger (§2.3, Table 2). Instruction tuning on 776K examples with chain-of-thought, program-of-thought and
tool-integrated solutions gives **DeepSeekMath-Instruct 7B**, at 46.8% on MATH (§3, Table 5).

On code, the paper's 1.3B experiments find that code training before math training improves math reasoning
both with and without tools, "at least for mathematical reasoning" (§5.1.1, Table 6). On arXiv, papers "seem
ineffective in improving mathematical reasoning": training on arXiv-only corpora brought no notable improvement,
or even deterioration, at 1.3B and 7B (§5.1.2, Tables 8–9). The paper offers no explanation and says the result
should be "taken with a grain of salt" — it has not studied other tasks, arXiv mixed with other data, or larger
models. The lecture's reason, that arXiv lacks adequate coverage of math content (≈42:13), is its own gloss.

### From PPO to GRPO

After supervised fine-tuning, DeepSeekMath scales up RL (≈43:48). The standard RL algorithm for RLHF, even with
verifiers, is **PPO**. Scaled to larger models it becomes a memory problem: the lecture counts four models to
keep — an old policy, the new policy being learned, a **critic** giving feedback, and a **reward model**
assigning rewards (≈43:48–44:35). **GRPO**, Group Relative Policy Optimization, removes the critic and keeps
three (≈44:35). For each question it samples many answers, scores each with the reward model, and normalizes:

$$\hat{A}_ {i,t} = \widetilde{r}_ i = \frac{r_i - \operatorname{mean}(\mathbf{r})}{\operatorname{std}(\mathbf{r})}$$

Here $\mathbf{r} = \lbrace r_1, \ldots, r_G\rbrace$ are the rewards of the $G$ outputs sampled for one question,
and every token $t$ of output $i$ gets that output's normalized reward as its advantage (Shao et al., §4.1.2).
"You could just take the reward, subtract the average reward, and then divide that by the spread of rewards"
(≈45:22). The group makes the rewards comparative, which suits reward models because they are trained on
comparisons anyway, and dropping the critic saves the memory that lets RL scale (≈45:22).

The paper's account (§4.1.1, Figure 4). PPO maximizes a clipped surrogate objective in which the advantage comes
from **Generalized Advantage Estimation** over a learned value function, a model typically as large as the
policy; and because a reward model usually scores only the last token, that value function is hard to train
accurately at every token. GRPO uses the average reward of the group as the baseline instead. Its objective, for
a question $q$ with outputs $o_1, \ldots, o_G$ sampled from the old policy $\pi_{\theta_{\text{old}}}$, is

$$\mathcal{J}_ {\text{GRPO}}(\theta) = \mathbb{E}\left[\frac{1}{G}\sum_{i=1}^{G}\frac{1}{\vert o_i\vert}\sum_{t=1}^{\vert o_i\vert}\left(\min\left[\rho_{i,t}\hat{A}_ {i,t}, \thinspace \operatorname{clip}\left(\rho_{i,t}, 1-\epsilon, 1+\epsilon\right)\hat{A}_ {i,t}\right] - \beta \thinspace \mathbb{D}_ {\text{KL}}\left[\pi_\theta \Vert \pi_{\text{ref}}\right]\right)\right]$$

where $\rho_{i,t} = \pi_\theta(o_{i,t} \mid q, o_{i,\lt t}) / \pi_{\theta_{\text{old}}}(o_{i,t} \mid q, o_{i,\lt t})$
is the probability ratio between the current and old policies, $\epsilon$ is the clipping range, and $\beta$
weights a KL-divergence penalty toward a reference model $\pi_{\text{ref}}$, usually the SFT model. PPO puts its
KL penalty inside the per-token reward; GRPO adds it to the loss directly (§4.1.1). The lecture's one mention of
generalized advantage estimation (≈44:35) attaches it to GRPO; in the paper it is PPO's method, which GRPO's
group baseline replaces.

The paper also applies GRPO with **process supervision**, where a process reward model scores each step and a
token's advantage is the sum of the normalized rewards of the steps after it, and **iteratively**, retraining
the reward model on the policy's new samples (§4.1.3–4.1.4, Algorithm 1).

### Results

On MATH, RL took the model from **46.8% to 51.7%**, and the lecture calls it the first open-source model at the
7B scale to cross 50% without a critic (≈46:09). In the paper, RL used about 144K chain-of-thought questions
related to GSM8K and MATH, 64 sampled outputs per question and a KL coefficient of 0.04 (§4.2). It raised
GSM8K from 82.9% to 88.2% and MATH from 46.8% to 51.7%, and also improved out-of-domain benchmarks it was not
trained on, such as CMATH from 84.6% to 88.8% (§1, Table 5). The paper describes the model as the first in the
open-source community over 50% on MATH (§1.2).

### One formula for SFT, RFT, DPO, PPO and GRPO

The lecture presents the paper's unified view: RL variants differ in the **data source** and in how they compute
the gradient, which depends on a **gradient coefficient** and on the probability of the output given the
question (≈46:09). In the paper,

$$\nabla_\theta \mathcal{J}_ {\mathcal{A}}(\theta) = \mathbb{E}_ {(q,o) \sim \mathcal{D}}\left[\frac{1}{\vert o\vert}\sum_{t=1}^{\vert o\vert} GC_{\mathcal{A}}(q, o, t, \pi_{rf}) \thinspace \nabla_\theta \log \pi_\theta(o_t \mid q, o_{\lt t})\right]$$

with three components: the data source $\mathcal{D}$, the reward function $\pi_{rf}$, and the algorithm
$\mathcal{A}$, which turns data and reward into the gradient coefficient $GC$ (§5.2.1). Its Table 10 places
supervised fine-tuning, rejection-sampling fine-tuning (RFT), DPO, online RFT, PPO and GRPO in that frame.

The lecture compares STaR, which is not in the paper's table, with online rejection-sampling fine-tuning and
GRPO (≈46:55–47:40). STaR generates once and gets a reward of 1 if correct and 0 if wrong. Online RFT generates
online but likewise rewards only correct answers and rejects the rest. GRPO generates online, scores the samples
with a reward model, and takes the advantage from the group baseline. The paper's experiments find that online
RFT beats offline RFT, especially later in training; that GRPO beats online RFT, because it can reinforce and
penalize responses by different amounts where online RFT reinforces every correct response equally and never
penalizes; that process supervision beats outcome supervision; and that iterative RL helps, most in its first
iteration (§5.2.1, Figures 5–6).

### Questions: regression, rewards and the KL term

The student who asked about regression on easy problems asks again: if fine-tuning on hard problems updates all
the weights, won't easy problems suffer (≈47:40)? The lecturer answers through the reward distribution. What the
model needs is a spread of rewards across the problems it is shown. On simple problems every sample gets 1, and
on problems that are all very hard every sample gets 0; either way the normalization has nothing to work with and
there is "nothing for the model to learn" — a shortcoming the next paper fixes (≈48:25–49:15). Another student
points to the KL-divergence term, which keeps the policy from drifting from what it could already solve; the
lecturer accepts that interpretation (≈49:15–50:02). Asked whether all the weights must be updated, the lecturer
says no: recent work, including blog posts, shows fewer-weight updates can work (≈50:47; the captions leave the
method's name unclear). Asked where a graded score comes from, rather than 0 or 1: from the trained reward model
(≈51:32).

### Consistency, not capability

The online RL loop, sampling from the current model, beats the earlier STaR-style approach. But with 32 samples,
RL improved majority voting over $K$ samples, $\text{Maj@}K$, and not $\text{Pass@}K$: the majority of solutions became correct, while the
chance that at least one was correct did not rise. "The model actually became more consistent, not fundamentally
smarter" (≈52:19).

This is the paper's Figure 7, $\text{Maj@}K$ and $\text{Pass@}K$ of the instruction-tuned and RL models on GSM8K
and MATH. The paper concludes that RL makes the output distribution more robust: "it seems that the improvement is
attributed to boosting the correct response from TopK rather than the enhancement of fundamental capabilities"
(§5.2.2). It suggests one reason may be that its RL used only the instruction-tuning questions and naive nucleus
sampling, and lists future directions for each component — out-of-distribution prompts and tree-search sampling
for the data; algorithms robust to noisy rewards, noting that even PRM800K has about 20% incorrect annotations;
and reward models that generalize, express uncertainty, and supervise the process (§5.2.3).

## DAPO: An Open-Source LLM Reinforcement Learning System at Scale (Yu et al., 2025)

Paper: [arXiv 2503.14476](https://arxiv.org/abs/2503.14476) (linked only). DAPO stands for **Decoupled Clip and
Dynamic sAmpling Policy Optimization** (Yu et al., abstract).

### Why naive GRPO falls short

Scaling GRPO naively to Qwen 32B, "which is very easily accessible", gives 30% on AIME. The lecture lists what goes
wrong: the entropy of the model **collapses** and it becomes too confident; training becomes **unstable**; and
response length becomes uncontrollable. DeepSeek's own RL reached 47% (≈53:06; the captions garble the name of
DeepSeek's model). DAPO makes explicit the RL techniques that GRPO's paper did not cover (≈53:54).

The paper gives the same numbers: 30 points for its initial GRPO run on Qwen2.5-32B against 47 for
**DeepSeek-R1-Zero-Qwen-32B**, with entropy collapse, reward noise and training instability as the causes; it
suggests critical training details were omitted from DeepSeek's R1 report (§1). DAPO reaches 50 points on AIME
2024 with 50% of DeepSeek-R1-Zero-Qwen-32B's training steps (§1, Figure 1).

Two further choices in the paper, not discussed in the lecture, differ from DeepSeekMath. DAPO **removes the KL
penalty**, because a long-chain-of-thought model is meant to diverge significantly from its initial model (§2.3).
And it uses no learned reward model: the reward is **rule-based**, 1 if the predicted answer is equivalent to the
ground truth and −1 otherwise, to avoid reward hacking (§2.4). Its training set, DAPO-Math-17K, has math problems
rewritten so that every answer is an integer, which keeps that rule reliable (§3.5).

The full objective samples a group of $G$ outputs $o_i$ for each question $q$, with probability ratio
$r_{i,t}(\theta)$ between the current and old policies and GRPO's group-normalized advantage $\hat{A}_ {i,t}$:

$$\mathcal{J}_ {\text{DAPO}}(\theta) = \mathbb{E}\left[\frac{1}{\sum_{i=1}^{G}\vert o_i\vert}\sum_{i=1}^{G}\sum_{t=1}^{\vert o_i\vert}\min\left(r_{i,t}(\theta)\hat{A}_ {i,t}, \thinspace \operatorname{clip}\left(r_{i,t}(\theta), 1-\varepsilon_{\text{low}}, 1+\varepsilon_{\text{high}}\right)\hat{A}_ {i,t}\right)\right]$$

subject to at least one, and not all, of the $G$ outputs being correct (§3, Equation 8). Each of the four
techniques below is one piece of that formula or of its reward.

### 1. Clip-Higher

PPO clips the probability ratio **symmetrically**, treating increases and decreases the same way. A low-probability
token can then only climb so far, while a high-probability one is barely constrained, so exploration collapses.
Making the clip **asymmetric** allows bigger increases (≈53:54–54:41). With it, accuracy is higher and entropy
stays stable instead of collapsing; entropy is a proxy for how much exploration is possible (≈54:41).

The paper's example: with $\varepsilon = 0.2$, a token whose old probability is 0.01 can rise at most to 0.012,
while one at 0.9 is allowed up to 1.08, so "exploitation" tokens are effectively unbounded and "exploration" tokens
are held down. It observed that the tokens hitting the upper clip had low probability, below 0.2 on average
(§3.1, Figure 3). DAPO **decouples** the lower and upper clipping ranges, raising only
$\varepsilon_{\text{high}}$ — raising $\varepsilon_{\text{low}}$ would push unlikely tokens toward probability 0 —
and trains with $\varepsilon_{\text{low}} = 0.2$ and $\varepsilon_{\text{high}} = 0.28$ (§3.1, §4.1). Figure 2
shows AIME accuracy and actor entropy before and after the change.

### 2. Dynamic Sampling

This fixes the problem from the DeepSeekMath Q&A. With GRPO, a group whose samples are all correct, or all wrong,
has zero advantage and contributes no gradient. DAPO **oversamples** and **filters out** groups with rewards all 0
or all 1, keeping only questions with some signal, so that no gradient is wasted and the effective batch size stays
constant (≈55:27–56:14). The lecture's "64 solutions" is an exemplar: the number of samples is a choice, specific
to the benchmark's difficulty and the base model's capability (≈56:14–57:01).

In the paper, the share of prompts with accuracy 1 keeps rising during training, shrinking the effective batch and
making the gradient noisier (§3.2, Figure 3). DAPO keeps sampling until the batch is filled with prompts whose
accuracy is neither 0 nor 1 — the constraint in the objective above. This does not necessarily slow training,
because generation time is dominated by long-tail samples, and the same performance is reached faster (§3.2,
Figure 6). The settings are 512 prompts per batch and **16 responses per prompt** (§4.1); 64 per question was
DeepSeekMath's GRPO setting (Shao et al., §4.2).

### 3. Token-Level Policy Gradient Loss

GRPO computes its loss **per sample**: each answer counts once, so a very long garbage answer can weigh the same as a
short good one. DAPO instead computes the loss **per token**, which the lecture presents as a way of shaping by
length (≈57:01–58:32). The lecture ties the change to controlling entropy and mean response length (≈57:47; as
captioned, the sentence on how response length moved is hard to follow).

The paper spells out the consequence. With every sample weighted equally, the tokens of a long response
each contribute *less*, which has two effects: the model learns less from the reasoning in high-quality long samples,
and it fails to penalize the gibberish and repeated words that excessively long samples tend to contain. Without the
change, entropy and response length both grew unhealthily (§3.3, Figure 4). In the normalization, GRPO averages
$\frac{1}{\vert o_i\vert}\sum_t$ within each sample and then $\frac{1}{G}\sum_i$ across samples; DAPO divides the sum
over all tokens by $\sum_i \vert o_i\vert$, so longer responses have more influence and a pattern is rewarded or
suppressed equally wherever it appears (§3.3). The paper found it added less accuracy than the other techniques but
made training more stable and length grow more healthily (§4.2). The explicit length penalty is the next technique.

### 4. Overlong Reward Shaping

On a hard problem the model may still be reasoning when it hits the generation limit and gets truncated, and these
truncated chains add a lot of noise. DAPO applies a **gradual penalty** to deal with them, which stabilized training
(≈58:32–59:17). Other papers handle long chains by increasing the context length over the course of RL (≈59:17).

In the paper, the default of giving truncated samples a punitive reward penalizes sound reasoning for being long.
**Overlong Filtering** first masks the loss of truncated samples, which already stabilizes training (§3.4, Figure 5).
**Soft Overlong Punishment** then adds a length-dependent penalty to the correctness reward:

$$R_{\text{length}}(y) = \begin{cases} 0, & \vert y\vert \leq L_{\max} - L_{\text{cache}} \cr \dfrac{(L_{\max} - L_{\text{cache}}) - \vert y\vert}{L_{\text{cache}}}, & L_{\max} - L_{\text{cache}} \lt \vert y\vert \leq L_{\max} \cr -1, & L_{\max} \lt \vert y\vert \end{cases}$$

where $\vert y\vert$ is the response length, $L_{\max}$ the maximum length and $L_{\text{cache}}$ the width of the
punishment interval (§3.4, Equation 13). Training used an expected maximum of 16,384 tokens plus a 4,096-token
cache, so generation stops at 20,480 (§4.1).

### Adding the pieces up

Starting from GRPO on AIME, the lecture walks through the gains (≈59:17–1:00:03), which are the paper's Table 1,
AIME 2024 accuracy averaged over 32 runs:

| Setting | AIME 2024 |
|---|---|
| Naive GRPO | 30 |
| + Overlong Filtering | 36 |
| + Clip-Higher | 38 |
| + Soft Overlong Punishment | 41 |
| + Token-level Loss | 42 |
| + Dynamic Sampling (DAPO) | 50 |
| DeepSeek-R1-Zero-Qwen-32B, for comparison | 47 |

The lecture says 50 beats what DeepSeek-R1 distilled into Qwen 32B had achieved (≈1:00:03). The paper's comparison
is DeepSeek-R1-Zero-Qwen-32B, DeepSeek's RL training of the Qwen2.5-32B base model, not a distilled model (§1,
Table 1).

### What to watch during RL

The loss by itself is not a good enough proxy for how RL training is going. Watch the **response length**, the
**entropy** — not too low and not too high — and the **share of samples with accuracy 1**, which tells you how much
you need to sample (≈1:00:03). If response length explodes, control the loss; if the entropy misbehaves, control it;
and if nothing improves after a number of steps, the reward may be saturated. The optimization in the RL loop is
simply harder (≈1:00:52).

The paper's section on training dynamics monitors response length, reward, generation entropy and mean probability
(§4.3, Figure 7). It warns that length can stagnate or fall for long periods, so it is read together with validation
accuracy; that the training reward rises steadily but correlates little with validation accuracy, a sign of
overfitting; and that entropy should stay in a range, with a slow upward trend helping performance. It also reports
that reflection and backtracking, "virtually" absent early in training, appeared as training progressed (§4.4,
Table 2).

## SFT or RL, and which of the three

The lecturer returns to the question held since STaR (≈1:00:52–1:01:37). In domains with a strong reward signal,
**RL** lets you hill-climb with fewer examples, but it takes a lot of work to get right. **Supervised fine-tuning**
on plenty of high-quality data is often the faster way to improve performance, but it does not bring or boost
reasoning in the same way.

And when to reach for each paper's approach (≈1:01:37–1:03:08):

- **STaR** when you have only a few examples with reasoning — say 100 — and no RL infrastructure. Simple reasoning
  tasks such as GSM8K improve reasonably, "maybe not phenomenal", within what the model could already do.
- **DeepSeekMath's GRPO** when you have a good enough base model primed with good instruction data. It is a
  reasonably strong algorithm that works with limited memory and GPUs, and is well proven on standard math reasoning.
- **Techniques like DAPO's** when reasoning chains get long, which usually means harder problems and a need for
  state-of-the-art performance: then every variable in the RL algorithm and infrastructure has to be controlled. This
  matters for competition-level problems such as AIME and IMO. (The captions garble the name in this sentence; the
  passage goes through the three papers in order.)

## What RL does and does not improve

All three techniques improve majority-vote performance, $\text{Maj@}K$, as compute grows, improve answer formatting, and make the
model more coherent over multiple steps. None of them yet improves **fundamental capability** — teaching the model to
solve new problems or to generalize far out of domain — and what it would take is an open question (≈1:03:08).
Improving $\text{pass@}K$ means exactly that kind of capability, and step jumps in capability have typically come from
a breakthrough or from scaling in some dimension (≈1:03:53–1:04:40).

Everything in the loop, from STaR's rationale quality to RL, depends on the model's ability to reason and on a
verifier or reward model (≈1:04:40–1:05:25). If the model is too capable, it hacks the reward; if the reward has too
little signal, the loop cannot hill-climb. Math and code have final answers, execution feedback — the RL paper from
the previous Friday, [RLEF](04-learning-from-feedback-with-tools-code.md) — and unit tests, so they are easy to
hill-climb; how many domains have such signals, which signals are good enough, and whether an **ensemble of
verifiers** can cover a single verifier's gaps are open questions (≈1:05:25–1:06:11; see
[lecture 3](03-robust-verification.md) on Weaver).

## Open problems

Train-time scaling is an active research area (≈1:06:11–1:07:43):

- **Why does only majority voting improve**, $\text{Maj@}K$ and not $\text{Pass@}K$?
- **Are reasoning behaviours real?** Backtracking, self-evaluation and self-correction show up in reasoning chains, but
  are they emerging, or were they already present and becoming statistically more prevalent? (DAPO's paper reports
  reflection appearing during training; see above.)
- **Learning from failures.** A few papers do it usefully, but most techniques just filter failures out.

Promising directions are better **data** generation; **algorithms** robust to noise in the reward models; the choice
of **rewards**; and combining STaR's rationalization with techniques like DAPO's — all suggested as project directions
(≈1:06:57–1:07:43).

## Closing questions

- **How much of a frontier model is still pre-training?** Not published for Anthropic. The lecturer recalls RL being
  about 1% against 99% for pre-training a year earlier and perhaps 5% now, and says Grok 4 claimed 50% RL without
  improving much, because the bottlenecks from this lecture — weak or noisy rewards — still apply (≈1:07:43–1:09:16).
  These figures are the lecturer's recollection, not from a reading.
- **Can RL teach knowledge the model has never seen**, such as calculus? The student's point is accepted: RL gets
  better at what the model can already do. "It's basically able to explore better in the design space of what it
  knows" and reach solutions by exploration and search (≈1:10:06–1:10:52).
- **What about benchmarks with very little data, like AIME, and the risk of leakage?** RL is more data-efficient, so
  it does not need much data; the papers show a certain number of examples is enough to hill-climb. Verification
  signals depend on having good verifiers, possibly several. "I don't think there are not enough data is the right
  abstraction. I think it's do you have enough data to hill climb on" — which is also what dynamic sampling filters for
  (≈1:10:52–1:12:25).

## Related pages

- [Reinforcement learning for LLMs](reinforcement-learning.md) — PPO, GRPO and DAPO side by side, and every RL method
  the course has covered.
- [Self-improvement](self-improvement.md) — training on the model's own filtered outputs: STaR's rationales and
  rationalization, and what RL does and does not improve.
- [The LLM training pipeline](llm-training-pipeline.md) — math pre-training on curated web data before RL, and SFT
  versus RL.
- [Reasoning models](reasoning-models.md) — the thinking patterns, and whether reflection emerges during RL.
- [Test-time scaling](test-time-scaling.md) — o1's train-time and test-time curves, and the loop from test-time
  outputs back into training.
- [Verifiers](verifiers.md) — the final answer as STaR's verifier, rule-based versus learned rewards.
- [Chain of thought](chain-of-thought.md) — bootstrapping rationales instead of prompting for them.
- [Lecture 3 — Robust Verification](03-robust-verification.md) — Math-Shepherd's PPO with a process reward, and
  Weaver's ensemble of verifiers.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md) — RLEF's RL from
  execution feedback, and RLHF and RLAIF.
