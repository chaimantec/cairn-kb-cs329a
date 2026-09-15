# Lecture 8 — Agentic Evaluations and Long Horizon Tasks

This lecture asks how to measure what AI agents can actually do, now that chatbot and single-shot
question-answering benchmarks saturate quickly (≈2:25–3:10). It looks at three recent answers, each
measuring a different axis. METR's *Measuring AI Ability to Complete Long Tasks* measures **duration**: the
length of task, in the time it takes a skilled human, that a model completes with 50% or 80% success, and
how that length has grown. Its answer is a doubling about every seven months since 2019. OpenAI's
*GDPval* measures **economic value**: whether a model's deliverable on real professional work is as good as
an industry expert's, judged head to head by other experts. There the best model is approaching parity, but
the trend is roughly linear. *DeepScholar-Bench* measures **research synthesis**: writing the related-work
section of a recent paper by retrieving, synthesizing and citing prior work. No system it tests scores
above 19% across all metrics.

The lecture's closing argument is that none of these is enough on its own. Long horizons do not mean
reliable or high-quality outputs, real work is heavy with context that benchmarks write into the prompt,
and agents still struggle to find and verify the right sources. So evaluation needs duration-based,
value-based and synthesis-quality metrics, each validated against humans who can do the tasks
(≈1:06:34–1:08:07).

The captions do not name the lecturer, so this page does not either.

[Edited transcript](../raw/transcripts/08-agentic-evaluations-and-long-horizon-tasks.md) ·
[verbatim captions](../raw/transcripts/original/08-agentic-evaluations-and-long-horizon-tasks.md) ·
[video](https://www.youtube.com/watch?v=8JAqLnTaZu4) ·
[course website](https://cs329a.stanford.edu/) · [sources](../sources.md)

> **Numbering.** This is catalog position 8 ("Part 8 | Agentic Evaluations and Long Horizon Tasks"), and it
> is site schedule row 17, "Agentic Evaluations & Long-Horizon Tasks" (Mon Nov 17). The transcript confirms
> the mapping: it names row 17's three readings at the start (≈1:38–2:25) and discusses them in that order —
> the METR time-horizon paper (≈3:56–27:25), GDPval (≈27:25–51:40) and DeepScholar-Bench (≈51:40–1:01:03).
> Rows 9–16 (guest lectures, midterm presentations, and the agentic software engineering and memory
> lectures) have no video in the catalog. The lecture refers back to a lecture on memory (≈18:56) and to an
> AI scientist paper covered earlier (≈5:32), neither of which is in the catalog.

## Readings

The course publishes no slides. Its course material is the three papers the course site lists for this
lecture. All three are licensed CC BY 4.0 (checked on their arXiv abstract pages, 2026-09-15), so their main
bodies are transcribed in this KB. Their appendices are not.

| Reading | In this KB | Where the lecture covers it |
|---|---|---|
| Kwa et al. (2025), [Measuring AI Ability to Complete Long Tasks](https://arxiv.org/abs/2503.14499) — **v2** | [main body](../raw/papers/08-metr-long-tasks.md) | ≈3:56–27:25 |
| Patwardhan et al. (2025), [GDPval: Evaluating AI Model Performance on Real-World Economically Valuable Tasks](https://arxiv.org/abs/2510.04374) — v1 | [main body](../raw/papers/08-gdpval.md) | ≈27:25–51:40 |
| Patel et al. (2025), [DeepScholar-Bench: A Live Benchmark and Automated Evaluation for Generative Research Synthesis](https://arxiv.org/abs/2508.20033) — **v1** | [main body](../raw/papers/08-deepscholar-bench.md) | ≈51:40–1:01:03 |

**Which version.** Two of the readings were revised after the lecture was given, and the revisions change
the numbers and names the lecture quotes. METR's paper is now at v4 (July 2026). It is retitled *Measuring
AI Ability to Complete Long Software Tasks*, and its headline model and doubling time differ. DeepScholar-Bench
is now at v2 (February 2026), which renames DeepScholar-base and reports new results. This KB transcribes the
versions that were current on the lecture date and that the lecture quotes: METR's
[v2](https://arxiv.org/abs/2503.14499v2) (30 March 2025), whose title is also the one the course site lists,
and DeepScholar-Bench's [v1](https://arxiv.org/abs/2508.20033v1) (27 August 2025). GDPval has only v1. The
abstract links above serve the latest version; cite the versioned links when quoting numbers from this page.

**About the figures on this page.** The images are the papers' own figures, cropped from the published PDFs
with their printed captions. This KB does not read values off charts: what a figure shows is what its caption
and the paper's text say. Where the lecturer reads a value off a chart, this page says so. Numbers on this page
come from the papers' text and tables.

## Why agentic evaluation is hard

The lecture opens by asking the class what evaluations they have run for their projects. Students mention a
multi-hop question answering task and a private company wiki fed to a coding agent (≈0:05–1:38). The question
the lecture then poses is what makes *agentic* evaluation harder than what came before (≈0:51).

Four or five years ago, a model producing any explanation at all was a big deal. Now progress is measured by
whether we can forecast AI's effect on the economy or on safety (≈2:25). Traditional benchmarks — chatbots,
question answering, single-shot answers to something already in the context — saturate quickly. So the lecture
measures two things (≈3:10–3:56): **capability**, as how long or complex a task a model can accomplish, and
**economic impact**, as the value of the tasks it can do and whether it can do real-world work humans do now.
METR calibrates its time-horizon metric by human professionals, and GDPval measures a model's win rate against
experts. "Both of them are equally valid metrics in certain ways. And the insights are very similar. But the
trends that they measure in terms of how models perform are quite different" (≈3:56).

METR's paper makes a similar case for a new metric. Existing benchmarks often consist of artificial rather than
economically valuable tasks, are often adversarially selected against current models, and saturate increasingly
quickly, with no intuitive way to compare across them (Kwa et al., §1).

## Measuring AI Ability to Complete Long Tasks (Kwa et al., 2025)

### Duration and reliability

A chatbot of three to five years ago lost track of a multi-turn conversation after a couple of rounds; today's
models keep context over long conversations. That leads to the question of **what duration of task a model can
complete**, and then **how reliably** (≈3:56–4:46). METR's metric ties both together, with the time a
professional human takes as the "universal anchor" (≈4:46–5:32).

The paper defines the **task completion time horizon** as "the duration of tasks that models can complete at a
certain success probability", and operationalizes it as the **X%-(task completion) time horizon**, "the length of
tasks that models can complete approximately X% of the time" (§1). Its headline is the 50% time horizon.

![METR, Figure 2](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-2.jpg)

*Kwa et al. (2025), Figure 2: the methodology. Create a diverse suite of 170 tasks; have humans and AI agents (a model plus a scaffold) attempt them, recording successful humans' times and agents' success rates; fit a logistic model to find the time horizon at which each agent has a 50% chance of success, and plot it against the model's release date.*

### Three task suites

The benchmark combines three suites, 170 tasks in all (≈5:32–6:19; §3.1):

- **HCAST** — 97 diverse software tasks from 46 task families, ranging from 1 minute to around 30 hours, in
  cybersecurity, machine learning, software engineering and general reasoning (§3.1.1).
- **RE-Bench** — 7 difficult ML research engineering tasks, each intended to take a human expert about 8 hours
  (§3.1.2). The lecture compares them to the AI scientist paper covered earlier in the class (≈5:32).
- **Software atomic actions (SWAA)** — 66 new single-step tasks of 1 to 30 seconds, "atomic actions commonly
  performed in software engineering work", built so the suite can measure pre-2023 models (§3.1.3).

All tasks are scored automatically. The lecture's examples are the paper's Table 1 (≈11:03–11:48):

| Family | Length | Description |
|---|---|---|
| find_shell_script | 3 seconds | Multiple choice: "Which file is a shell script?" Choices: "run.sh", "run.txt", "run.py", "run.md" |
| wikipedia_research | 1 minute | Research simple factual information from Wikipedia and provide accurate answers to straightforward questions. |
| oxdna_simple | 9 minutes | Detect and fix a bug in the input files for a molecular dynamics simulation using the oxDNA package. |
| munge_data | 56 minutes | Write a Python script to transform JSON data from one format to another by inferring the conversion rules from provided example files. |
| cuda_backtesting | 8 hours | Speed up a Python backtesting tool for trade executions by implementing custom CUDA kernels while preserving all functionality, aiming for a 30x performance improvement. |

*Kwa et al. (2025), Table 1. The lecture gives the oxDNA task as about 10 minutes.*

The paper's text frames the lengths: under a minute, tasks measure knowledge needed for software engineering but
do not require agency; around ten minutes is about the easiest meaningful step of a real project; the shortest
standalone economically valuable projects take about an hour; and by eight hours tasks are "meaningfully valuable
software projects" (§3.1.4).

**Tasks from the class** (≈7:54–9:29). Asked which tasks they would delegate, students name writing a paper's
literature review (several hours — HCAST or RE-Bench territory), getting every BibTeX entry right by querying the
right websites (more HCAST-like), and refactoring a code base, which could take a few hours or a couple of days
and still needs someone to check correctness (closer to RE-Bench). The literature review comes back as the
third reading's task.

### Human baselines

A student asks how the human times are set when some people are faster than others: one expert, or a
distribution of many (≈9:29–10:18)? The lecturer's answer is that getting many experts is expensive, so
typically a few experts solve a task and you look at inter-rater agreement, collecting more data points when
the times are far apart (≈10:18).

In the paper, baseliners are "skilled professionals in software engineering, machine learning, and
cybersecurity", with an average of about 5 years of relevant experience (§3.2). Over 800 baselines total 2,529
hours: 558 (286 successful) from HCAST and RE-Bench, and 249 (236 successful) from SWAA. HCAST baseliners work in
the same environment as the agents, recorded to prevent cheating, with bonuses for success and speed, and a task's
duration is the **geometric mean** of its successful baselines (§3.2.1). 148 of the 169 tasks in Table 2 have
human baselines; 21 HCAST tasks rely on researcher estimates (Table 2). RE-Bench tasks are all rated 8 hours, and
SWAA was baselined by METR staff with a timing web app (§3.2.2–3.2.3).

The lecture adds a caveat of its own: experienced people "are conditioned to what it means to be successful", so
they may underestimate a task's difficulty in a way that does not match how hard it is for a model (≈11:48). The
paper discusses skill in both directions: its baseliners have much less context than employees, which may
*increase* measured task length, but are likely much more skilled than the average software engineer, which may
*decrease* it (§8.1).

### From success rates to a time horizon

Agents were run about 8 times on each task (§3.3.2). Success falls as tasks get longer: across all models, success
rate is negatively correlated with human completion time, well fit by an exponential model
($R^2 \approx 0.83$ against the logarithm of human time) (§3.3.3, Figure 4). The lecturer describes such a plot —
human time on the x-axis, success rate from 0 to 1 on the y-axis — as showing SWAA's short tasks mostly succeeding,
HCAST's anywhere from about 0.8 down to 0, and RE-Bench's having some success but on the low side (≈12:37–13:24).
That is the lecturer's reading of the chart. "the longer the task, the harder it is for models to stay on track and
complete" (≈13:24).

To turn that into one number per model, each run is scored as success or failure — continuously scored tasks
are thresholded at human performance — and a logistic curve is fitted per model (§4.1):

$$p_{\text{success}}(\text{model}, \text{task}) = \sigma\left((\log h_{\text{model}} - \log t_{\text{task}}) \cdot \beta_{\text{model}}\right)$$

Here $t_{\text{task}}$ is the geometric mean time of the task's successful human baselines, $\sigma$ is the
logistic function, and $h_{\text{model}}$ and $\beta_{\text{model}}$ are learned per model. $h_{\text{model}}$
is the **50% time horizon**: the human task length at which the fitted curve predicts a 50% chance of success.
The approach is inspired by item response theory, except that task difficulty comes from human time rather than
being learned from agent performance (§4.1).

![METR, Figure 5](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-5.jpg)

*Kwa et al. (2025), Figure 5: success rates of all models on the test suite, showing the computation of time horizon as predicted 50% success rate time. The logistic fit is fairly good, though there is a jump in success rate between tasks under 1 minute and tasks over 1 minute, the boundary between SWAA and HCAST tasks.*

### Doubling every seven months

Plotting each model's horizon against its release date gives the paper's headline (≈13:24–15:01). Regressing
the logarithm of time horizon on release date, the horizon "has doubled every 212 days", with a 95% bootstrapped
confidence interval of 171–249 days (§4.2) — about seven months. The lecture walks the trend: GPT-2 in 2019 at
about two seconds, GPT-4 in 2023 at minutes, and Claude 3.7 Sonnet in 2025 at 59 minutes (≈13:24–15:01). The
lecturer reads GPT-4's horizon off the chart as about eight minutes (≈14:14); the paper's text puts chat models
like GPT-4 and Claude 3 Opus "in the 5–30 minute range" (§4.2). The paper's abstract rounds current frontier models
such as Claude 3.7 Sonnet to "around 50 minutes", and §4.2.1 gives its 50% horizon as 59 minutes.

![METR, Figure 1](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-1.jpg)

*Kwa et al. (2025), Figure 1: the length of tasks, measured by how long they take human professionals, that generalist autonomous frontier model agents can complete with 50% reliability has been doubling approximately every 7 months for the last 6 years. The shaded region is a 95% CI by hierarchical bootstrap over task families, tasks and task attempts.*

The lecturer stresses what 50% means: "that's like saying that you gave your task to an intern, but it only
completes it 50% of the time" (≈14:14). The paper notes that the trend in 2024 and early 2025 may be faster, with o1
and Claude 3.7 Sonnet above the long-run trend, though the gap is hard to distinguish from noise (§4.2).

### What drives the improvement

The lecture lists the capabilities behind the trend (≈15:01–16:34): better logical reasoning, better code
generation and better tool use (it points back to ReAct, [lecture 4](04-learning-from-feedback-with-tools-code.md)),
more reliability — not repeating the same behaviour over and over — and recovering from errors, which on an
eight-hour task needs "some notion of state, or memory, of what the final goal is" and how far along you are.
The paper's qualitative analysis reads transcripts of the tasks where newer models beat older ones. It finds models
"have improved greatly in terms of tool use capabilities, demonstrate a markedly greater ability to adapt to
mistakes (as opposed to repeating unsuccessful actions), and perform much better at parts of tasks requiring
logical reasoning or code generation" (§5).

**Why does Claude Code do better than a year ago?** The class suggests several reasons (≈16:34–19:43):
*context engineering* — structuring the context better across time steps, including compaction when the context
fills up; learning from user feedback in logs, as Cursor may do for tab completions; explicit planning for complex
problems, with **re-planning** after executing some steps; users pasting back broken code as feedback; and memory,
so an agent keeps its understanding of a code base between tasks. The lecturer points to the course's lecture on
memory, which is not in the catalog.

### 50% versus 80%

Capability is one axis and **reliability** the other (≈19:43). At an 80% success threshold, horizons are far
shorter. The lecture reads the chart as grey for the 50% line and blue for 80%, with the 80% horizons around 8 to 10
minutes against the 50% line's 59 minutes, "a lot of headroom" in reliability (≈20:30–21:17). In the paper's text,
the doubling time at 80% (213 days) is similar to 50% (212 days), but "Claude 3.7 Sonnet has the longest 80%-horizon
among models we examined at around 15 minutes, in contrast to its 50%-horizon of 59 minutes" (§4.2.1). Horizons at
80% are "roughly 5x shorter" (§1). The lecture gives the same 15 and 59 minutes (≈22:03). Where the captions have the
80% horizon as "five weeks shorter", the paper says about five times shorter.

![METR, Figure 6](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-6.jpg)

*Kwa et al. (2025), Figure 6: trend in 80% success rate time horizon. The doubling time is similar to the 50% plot, but horizons are substantially lower. The 50% horizon trend is shown in grey.*

The gap means that deploying these models on real tasks will run into reliability problems "even for moderately
complex tasks", leaving headroom for how systems are built on top of them (≈22:03). The paper concludes the same:
"even models that sometimes succeed on difficult and diverse tasks cannot reliably perform tasks of moderate length"
(§4.2.1). It adds that measuring horizons at very high success rates, such as 95%, would need very large task sets
with near-zero label noise (§8.1).

### How agents fail

The paper hand-labelled 31 failed runs of GPT-4 1106 and 32 of o1 (≈22:03–24:19; §5):

| Failure type | GPT-4 1106 | o1 |
|---|---|---|
| Poor planning/tool choice | 4 | 6 |
| Incorrect mental math/reasoning | 6 | 7 |
| Premature task abandonment | 8 | 16 |
| Repeating failed actions | 12 | 2 |
| Other | 1 | 1 |
| Total | 31 | 32 |

*Kwa et al. (2025), Table 3. As o1 succeeds at more tasks, its failures come from more challenging tasks than GPT-4's.*

The lecture walks the categories: poor planning (not knowing how to break the task into steps) and poor tool
choice; incorrect mental math or reasoning; abandoning a task prematurely without working out what success means;
and repetitive loops, where a failed action stays the highest-probability action and gets repeated — frequent for
GPT-4 and lower for o1 (≈22:48–23:34). The paper's reading: over a third of GPT-4's failures were repeated failed
actions against 2 of 32 for o1, "quantitative evidence" that models have improved at adapting to mistakes, while half
of o1's failures were premature abandonment, which may reflect harder tasks or quirks of o1 (§5). The lecture adds that
the paper is older and newer models exist, but the failure modes are often similar (≈22:03–22:48). Failure analysis is
standard practice for a benchmark, and it is what the homework's self-refinement loops ask students to do (≈24:19–25:05).

### Limits of the benchmark

"No benchmark is ever perfect" (≈25:05). The lecture credits METR with the first attempt at measuring task time
horizons, then names three challenges (≈25:05–27:25), each one of the paper's external-validity experiments (§6):

- **Messier tasks go worse.** Performance is lower on tasks without a single correct answer or with more
  complexity. The paper scored HCAST and RE-Bench tasks on 16 "messiness" factors, such as being resource
  limited, novel, or in a dynamic environment. Controlling for length, agents do worse on messier tasks — about 8.1%
  lower mean success per point of messiness — but the *trends* over time are similar for more and less messy tasks,
  with "no evidence of either much slower performance trends, or a plateau" for the messier ones (§6.2, Figures 9 and
  10). The mean messiness score is 3.2 of 16, and none is above 8.
- **SWE-bench Verified.** The method replicates there, but with a shorter doubling time — around 70 days, against
  104 days on METR's suite for 2024 models (§6.3). The paper attributes this to SWE-bench's annotator time estimates,
  written for an engineer who has had a few hours with the codebase, which "differentially underestimate how long our
  contract baseliners take to complete the easiest SWE-bench verified tasks". The lecture gives the annotator
  explanation and adds that models have seen most GitHub repositories (≈25:50); the contamination point is not in the
  paper's text.
- **Internal pull requests.** On five uncontaminated issues from an internal METR repository, contract baseliners
  took 5x–18x longer than the repository maintainers. Agent performance is consistent with success curves derived
  from *contractor* time, so time horizons "may have better correspondence to the labor of a low-context human, rather
  than a high-context human" (§6.4). The lecture's gloss: the model operates like a contractor asked to solve a problem
  in a code base it has never seen, not like an expert with context (≈26:37).

![METR, Figure 11](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-11.jpg)

*Kwa et al. (2025), Figure 11: performance of frontier AI models using reported SWE-bench Verified results. The trend is exponential as in Figure 1, with a steeper slope.*

Extrapolating the trend, the paper puts a 1-month (167 working hours) time horizon on software tasks between
late 2028 and early 2031, as an 80% confidence interval — if the trend continues and generalizes to real tasks
(§7.1, §8.3). The lecture's summary quotes "2028 to 2031" (≈1:06:34).

![METR, Figure 12](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/metr-figure-12.png)

*Kwa et al. (2025), Figure 12: a sensitivity analysis of the extrapolated date at which frontier AI systems will have a horizon of 1 month, over 10,000 random perturbations per row. Boxes span the 25th to 75th percentiles and whiskers the 10th to 90th. The plot does not account for future changes in the trend or external validity concerns, which are most of the uncertainty.*

## GDPval (Patwardhan et al., 2025)

### Win rate against experts

GDPval changes the question from "can AI do this?" to "if we were to give this task to the model instead of a
human, is the output good enough?" (≈27:25–28:11). Its metric is a **win rate** in blinded pairwise comparisons: experts in
the relevant occupation see the request, the reference files and two or more unlabeled deliverables, and rank them.
Each gold-subset comparison took over an hour to grade on average (§2.5). The paper argues the metric has no "upper
limit": a win rate does not saturate, and the human baseline could later be replaced by stronger models (§1).

The tasks are built from real work. Experts needed at least 4 years of professional experience; the average was 14
years (§2.2) — "more than a decade", as the lecture puts it (≈28:11). Each task is a request, often with reference
files, and a deliverable, and its dollar value is its estimated completion time multiplied by the occupation's median
hourly wage (§2.3). All 1,320 tasks went through automated screening and at least three human reviews, five on
average (§2.4).

### Occupations and tasks

The lecture reads through the occupations — real estate, government, manufacturing, professional, scientific and
technical services, healthcare, finance, retail, wholesale, and information including film and video editors — and a
set of example tasks, from a 3D model of a cable reel stand for a manufacturing engineer to an itinerary for a family of
four for a concierge (≈28:11–29:44). The paper's Figure 1 shows example tasks from the full set; this page does not
match the lecture's examples to it.

![GDPval, Figure 2](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/gdpval-figure-2.jpg)

*Patwardhan et al. (2025), Figure 2: GDPval includes real-world work from 44 occupations.*

The lecture summarizes the scope as nine sectors, 44 occupations and about 1,320 tasks, 220 of them in an open gold
set, predominantly digital (≈33:45). The paper's selection has two steps (§2.1). It takes the sectors that **each
contribute over 5% of U.S. GDP** (Q2 2024), which gives nine; the lecture's "top 5% of the GDP" (≈33:45, ≈48:32) is a
different phrasing of that threshold. Within each sector it takes the 5 occupations contributing most to total wages
that are **predominantly digital**, classifying an occupation as digital if at least 60% of its O*NET tasks are digital,
weighted by relevance, importance and frequency. The lecture's "60% of the O*NET tasks that are computer-based and
digital made it into this evaluation dataset" (≈33:45) describes the same 60% threshold differently. The nine sectors,
their share of GDP and their top occupations are the paper's Table 1; the 44 occupations collectively earn \$3T a year
(§2.1).

The lecture's other task characteristics (≈34:32–35:17): tasks average about seven hours of expert work and some take
weeks; they are text-based and multimodal (CAD, video, audio, spreadsheets, presentations); many need interaction
with reference files; and they were vetted by experts as well specified. The paper's main body gives the average of 7
hours, "up to multiple weeks", the formats, and up to 17 reference files per task in the gold subset and 38 in the full
set (§1). The lecture's other figures are in the paper's appendix, which is not transcribed here: the gold subset's mean
dollar value per task is \$398.46 (appendix A.4, Table 3), 67.7% of tasks required interaction with at least one reference
file (A.4.1), and 89.07% of gold-subset tasks were rated well specified by the grading experts (A.4.3, Table 7). The captions
garble the name of a source of higher-value tasks the lecturer compares with (≈34:32). The lecture also places the 220-task
gold set on Hugging Face (≈33:45); the paper says it open-sources the gold subset's prompts and reference files (§4) without
naming where.

**Is this too subjective?** A student points out that much of this is subjective to evaluate (≈30:30). The lecturer
agrees, which is why the metric is a win rate, and adds that real-world work needs both context and subjectivity in how
it is judged. In the class discussion that follows, a student's private company wiki turns out to be good for finding
dead pages; the class notes that nurses appear but doctors do not, and that composing music is missing, where
verification is especially subjective. The lecturer separates whether models *can* do a task from whether we *should*
use them for it, for ethics and compliance (≈31:20–33:45).

### Headline results: approaching parity, linearly

GDPval evaluated GPT-4o, o4-mini, o3, GPT-5, Claude Opus 4.1, Gemini 2.5 Pro and Grok 4, sampling each model 3 times
per prompt and having 3 graders grade each sample (§3.1). "On the GDPval gold subset, 47.6% of deliverables by Claude
Opus 4.1 were graded as better than (wins) or as good as (ties) the human deliverable" (§3.1). The lecturer reads the
chart as wins plus ties in light blue and wins alone in dark blue, going from 12.4% for GPT-4o to the 30s for more recent
models and 47.6% for Claude Opus 4.1 (≈35:17–36:05).

![GDPval, Figure 5](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/gdpval-figure-5.jpg)

*Patwardhan et al. (2025), Figure 5: on human pairwise comparisons, models are beginning to approach parity with industry experts on the GDPval gold subset.*

![GDPval, Figure 6](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/gdpval-figure-6.jpg)

*Patwardhan et al. (2025), Figure 6: performance of OpenAI frontier models increased roughly linearly over time on the GDPval gold subset.*

The lecture's point is the shape of the trend: "a very different trend compared to METR", "more of a linear trend
roughly compared to the exponential trend that METR was talking about" (≈36:05). A student asks whether the two are even
comparable, since one y-axis is a win rate and the other a time horizon (≈36:54). The lecturer agrees they measure
different things, and says the contrast is a caution about exponential trends: people assume a one-hour horizon becomes
a couple of hours and then a couple of days, whereas GDPval shows how reliable models are on real work of that length,
broken down by profession (≈36:54–37:40).

### Model strengths and failure modes

The paper finds Claude Opus 4.1 the best performer, "excelling in particular on aesthetics (e.g., document formatting,
slide layout), while GPT-5 excelled in particular on accuracy (e.g., carefully following instructions, performing
correct calculations)" (§3.1). The lecture gives the same contrast (≈38:27), though the captions do not name the first
model. Clustering the experts' justifications, the paper finds Claude, Grok and Gemini most often lost on
instruction-following failures, while GPT-5 high lost mainly on formatting errors and had the fewest instruction-following
issues. Gemini and Grok "frequently promised but failed to provide deliverables, ignored reference data, or used the wrong
format", and all models sometimes hallucinated data or miscalculated (§3.3). The lecture: models "will promise to look at
the reference data, but then actually not look at it" and override it with a hallucination (≈38:27–39:12).

![GDPval, Figure 8](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/gdpval-figure-8.jpg)

*Patwardhan et al. (2025), Figure 8: across models, experts most often preferred the human deliverable because models failed to fully follow instructions on GDPval tasks.*

**How bad are the losses?** The lecture describes GPT-5's lost tasks as roughly half "acceptable but subpar", about 20%
where the model is genuinely better, and 29% bad or catastrophic, with some disagreement between graders (≈39:12–39:59).
This analysis is in the paper's appendix (A.2.6), which is not transcribed here: other expert graders re-rated the tasks
GPT-5 lost; the most common rating was "acceptable but subpar", about 29% were bad or catastrophic (about 3% catastrophic),
and 23% said the model was in fact better, which the paper relates to its level of inter-rater agreement.

The paper also shows that effort helps. More reasoning effort improved o3 and GPT-5, and a prompt that told GPT-5 to check
its deliverables — including rendering files as images — eliminated black-square artifacts that had affected over half of
its PDFs, cut egregious PowerPoint formatting errors from 86% to 64%, and raised win rates by 5 percentage points (§3.4,
Figure 9).

### Speed and cost with a human in the loop

Since this is a self-improving agents class, the lecture highlights a loop: try the model, and if the output is not good
enough, try again, up to $n$ times, before doing the task yourself (≈39:59–40:45). In the paper's setup, "an expert human
samples from a model, reviews outputs, and if unsatisfactory, resamples and repeats. If no satisfactory output is obtained,
the human completes the task themselves" (§3.2).

![GDPval, Figure 7](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/gdpval-figure-7.jpg)

*Patwardhan et al. (2025), Figure 7: in the scenarios analyzed, models show the potential to save time and money by coupling AI assistance with expert human oversight; speed and cost savings from a "try n times, and if still unsatisfactory, fix it yourself" approach.*

The numbers the lecture quotes — about 1.6 times in cost and 1.4 times in speed for GPT-5 relative to an unaided expert
(≈40:45) — are in the paper's appendix (A.2.1, Table 2), not the transcribed main body. There, with $w_i$ the model's win
rate on task $i$, $M$ its completion time or cost, $R$ an expert's review time or cost, and $H$ the human expert's
completion time or cost, the expected time of the strategy of trying up to $n$ times is

$$\mathbb{E}[T_{n,i}] = \left(M_{T,i} + R_{T,i}\right)\frac{1 - (1-w_i)^n}{w_i} + (1-w_i)^n H_{T,i}$$

and likewise for cost. GPT-5's row in that table, at a 39.0% win rate, gives a speed improvement of 90x naive, 1.12x
trying once and 1.39x trying $n$ times, and a cost improvement of 474x naive, 1.18x and 1.63x. Review averaged 109 minutes
and \$86 per deliverable. The paper warns that including review and redo time shrinks the payoff, and that the analysis
does not capture the cost of catastrophic mistakes (appendix A.2.1). The lecture's remark that a successful run still costs
"less than 10% of the human expert salary" (≈40:45) matches a finding of the *METR* paper, not GDPval: more than 80% of
METR's successful agent runs cost less than 10% of what a human expert would (Kwa et al., §8.2, Figure 13).

### Where models do well

By sector, the lecture says models were near parity in government, retail and wholesale; by duration, better at tasks up
to a few hours; and by modality, some models better at multimodal tasks and others at text (≈41:32–42:18). The paper's
appendix, not transcribed here, says the same of sectors ("e.g., Government, Retail Trade, and Wholesale Trade") and finds
win rates highest for tasks of 0–2 hours, declining as completion time increases (A.2.2, A.2.5).

The lecture then reads three dense per-occupation charts from the appendix (A.2.3), with the human-expert level as a red
line (≈42:18–45:25). The occupations it names as at or near expert level are counter and rental clerks, real estate
brokers, shipping, receiving and inventory clerks, buyers and purchasing agents, computer and information systems managers
and software developers; administrative services managers, compliance officers, medical and health services managers,
personal financial advisors and customer service representatives; and first-line supervisors of retail sales workers,
editors, private detectives and investigators, news analysts, and wholesale and manufacturing sales representatives. These
are the lecturer's readings of the charts; the list gives "a taxonomy, of the set of tasks that the models are starting
to perform well in", and the failing ones are a starting point for research (≈43:53–44:39).

A student notes that software developers vary a lot in experience (≈43:05). The lecturer guesses that the software tasks
lean towards maintaining a repository, where knowing the codebase matters, while in fields like industrial or mechanical
engineering the decade of experience matters more (≈43:05–43:53). The paper does not say this.

### Context is the missing piece

If you under-specify the prompt by removing context, models do worse: the lecture says a few points lower in win rate, and
that models "struggle to figure out what to work on" (≈45:25). The paper's appendix (A.2.7) describes the experiment:
prompts cut to 42% of their token length, omitting where to find data in the reference files, how to approach the problem
and the expected format, so the model had to "figure it out". GPT-5 did worse and "struggled to figure out context". The
paper notes this was run on an earlier version of the gold subset, so its win rates do not match the main results. The
limitation is in the main body too: GDPval provides the full context of a task in the prompt, "but in real life it often
takes effort to figure out the full context of a task and understand what to work on" (§5).

The lecture draws the lesson (≈45:25–47:46). When the human specifies everything in their head and the job is reasoning
and executing tool calls, models can do it. But much of real work is figuring out what to work on and what belongs in the
context, and "humans are basically architecting the set of problems". It connects this to METR's maintainers and
contractors: "real work is often context heavy", and GDPval measures whether a smart person could do the task, not whether
someone embedded in the work could. In the near term, AI assistance is cost-effective in professional workflows with human
oversight, and different models suit different task types.

**Questions** (≈47:46–51:40). Asked how much to trust the tasks' dollar values, the lecturer says the point is less the
dollars than which categories of task models are good at — deep research, code generation, understanding diverse
information sources — and which professions those fall in. Before this benchmark there was no quantitative way to say
that. And when a model "outperforms" a human, look at the prompt: a human with a lot of context wrote it all down, while
real workplace challenges are often about what to prioritize and where to put resources (≈50:54).

## DeepScholar-Bench (Patel et al., 2025)

### The task: write the related work section

The third reading benchmarks the task a student asked for earlier: have AI write a paper's literature review (≈51:40).
Where METR asks how long a task takes and GDPval whether the output is good enough, DeepScholar-Bench asks whether AI can
do **deep research synthesis**: study many references, retrieve the relevant information, synthesize it into a coherent
report, and give verifiable citations (≈51:40). The lecture lists OpenAI's, Gemini's and Perplexity's deep research
products, STORM from Stanford, and OpenScholar, and recalls that the class built a deep research agent in homework 3
(≈51:40–52:26; see [retrieval and deep research agents](retrieval-and-deep-research.md)).

The paper calls these systems **generative research synthesis**: "performing retrieval over the live web and
synthesizing many discovered sources into long-form, cited summaries". Evaluating them is hard because question-answering
benchmarks focus on short factual answers, while expert-curated datasets "risk staleness and data contamination"
(abstract). Its task: "given a description, $d$ of a paper, the goal is to retrieve a set of relevant sources, $S$, and
generate a related works sections, $W$, for the paper by synthesizing and citing the retrieved documents" (§2). In the
experiments, $d$ is the paper's abstract.

### A live dataset

The dataset is scraped from recent arXiv papers by an automated pipeline that can be re-run for new queries, which keeps
it current and limits contamination (≈52:26–53:13; §2.1). To avoid earlier versions leaking in, it takes only v1 papers;
it keeps papers listed as accepted at a conference that have an explicit related-work section and a well-formatted .bib
file; and it extracts the section and each citation's details from arXiv and OpenAlex (§2.1).

The instantiation evaluated in the paper takes papers published between April and June 2025, after the 5 April 2025
release of Llama-4, the main open-source model benchmarked. It draws on 18 distinct arXiv domains, excludes related-work
sections over 1,000 words, and has **63 papers**. On average a human-written section has 23 unique references, and over
63% of cited references are on arXiv (§2.2). The lecture's account differs in places: it describes "PhD level difficulty
across 22 domains" and a dataset re-run "monthly" (≈52:26–53:13). The v1 paper gives 18 domains and says the authors
"plan to continuously re-run" the pipeline and update the dataset; it gives no difficulty grading. The lecture's "papers
that are post-training of a major model" (≈53:13) is the Llama-4 cut-off.

![DeepScholar-Bench, Figure 2](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/deepscholar-figure-2.jpg)

*Patel et al. (2025), Figure 2: DeepScholarBench overview. Recent, high-quality arXiv papers are scraped from diverse domains and key attributes extracted by an automated, re-runnable pipeline; the task is to generate a related works section from information about a paper such as its title and abstract; the evaluation framework measures knowledge synthesis, retrieval quality and verifiability.*

### Three dimensions, seven metrics

The lecture presents the evaluation's three axes (≈53:13–54:46), and the paper defines seven metrics under them (§3,
Table 1):

| Dimension | Metric | What it measures |
|---|---|---|
| Knowledge synthesis | Organization | organization and coherence of the answer |
| Knowledge synthesis | Nugget Coverage | the answer's coverage of essential facts |
| Retrieval quality | Relevance Rate | average relevance of all referenced sources |
| Retrieval quality | Document Importance | how notable the referenced sources are, by citation counts |
| Retrieval quality | Reference Coverage | coverage of the key, important references |
| Verifiability | Citation Precision | percent of cited sources that support their accompanying claim |
| Verifiability | Claim Coverage | percent of claims fully supported by cited sources |

*Patel et al. (2025), Table 1, laid out with the dimension in its own column.*

**Knowledge synthesis** (§3.1). *Organization* is an LLM judge's pairwise preference between the generated section and the
human-written one, evaluated twice with positions swapped to avoid position bias, and reported as a win rate. *Nugget
Coverage* breaks the human-written section into "information nuggets" — essential facts — with an LLM, and scores the
fraction present in the generated report. The lecture: is the output coherent, and "is it capturing the key nuggets?"
(≈53:13).

**Retrieval quality** (§3.2). *Relevance Rate* has an LLM grade each retrieved source $s$ from 0 to 2 and averages over the
retrieved set $S$:

$$RR(S) = \frac{1}{2|S|}\sum_{s\in S} Rel(s)$$

*Reference Coverage* uses the human-written section's references, each labelled by an LLM judge as important — one that
could not be omitted or swapped for another — or not. With $E$ the set of important references,

$$RC(S, E) = \frac{1}{|E|} \sum_{s\in S} I[s \in E]$$

*Document Importance* compares the median citation count of the system's sources with the median over the human
exemplars' sources $S^\ast$, capped at one:

$$DI(S, S^\ast) = \min\left(\frac{\operatorname{median}\lbrace \operatorname{num-cites}(x) \mid x\in S\rbrace}{\operatorname{median}\lbrace \operatorname{num-cites}(x^\ast) \mid x^\ast\in S^\ast\rbrace}, 1\right)$$

The lecture's version: are the 10 references an agent fetched for your final report relevant to what you want to present;
are they highly cited; and is it finding the important papers (≈53:59–54:46)?

**Verifiability** (§3.3). *Citation Precision* counts a citation as precise if the source supports at least one claim in its
sentence. *Claim Coverage* gives a sentence a score of one if its claims are fully supported by the sources cited in it or
within a window of $w$ sentences either side, counting the query's paper description as an implicit source. An LLM judge
assesses each entailment. The lecture: one metric measures precision and the other coverage (≈54:46).

The metrics were validated against over 200 expert annotations (§5.4). The lecture gives the agreement as about 70% to 80%
(≈54:46):

| Evaluation Metric | LLM-Classified Labels | Human-Agreement Score with LLM |
|---|---|---|
| Organization | Pairwise Comparison (Lose / Tie / Win) | 78% |
| Nugget Coverage | Nugget Importance (Vital / Non-vital) | 72% |
| Retrieval Relevance Score | Graded Relevance (0/1/2) | 70% |
| Reference Coverage | Reference Importance (Not Imp./ Imp.) | 82% |

*Patel et al. (2025), Table 4.*

The paper also contributes **DeepScholar-base**, a reference pipeline built on the LOTUS API. It iteratively writes search
queries and searches, then filters out irrelevant sources, ranks the rest with an LLM-based top-k, and aggregates the
remainder into the report (§4).

![DeepScholar-Bench, Figure 3](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/deepscholar-figure-3.png)

*Patel et al. (2025), Figure 3: overview of DeepScholar-base. The system iteratively writes queries and performs web search, then passes the results through LOTUS semantic operators: a filter to discard irrelevant sources, a top-k ranking step, and an aggregation step that generates the final report.*

### Results: far from saturated

"The fun thing about this particular benchmark is that none of the existing systems actually exceeds 19%" (≈54:46). The
paper: "no method is able to achieve a score greater than .19 across all metrics", and on Nugget Coverage, Reference Coverage
and Document Importance every method stays well below .45 (§5.2.1). Every system accessed the web only through the arXiv API,
with results published after the query paper filtered out (§5).

![DeepScholar-Bench, Figure 1](../raw/images/08-agentic-evaluations-and-long-horizon-tasks/deepscholar-figure-1.jpg)

*Patel et al. (2025), Figure 1: performance of generative research synthesis systems on DeepScholar-Bench — (a) open-source systems (DeepScholar, STORM, OpenScholar), the DeepScholar baseline and a Search AI, each with Llama-4-Scout-17B-16E-Instruct; (b) Search AIs with o3, Claude-opus-4 and Gemini-2.5-pro, OpenAI's DeepResearch, and DeepScholar-base.*

An excerpt of the main results (Table 2):

| System | Org. | Nug. Cov. | Rel. Rate | Ref Cov. | Doc Imp. | Cite-P | Claim Cov ($w=1$) |
|---|---|---|---|---|---|---|---|
| Human-written Exemplars | .500 | 1.000 | .585 | 1.000 | 1.000 | .278 | .205 |
| STORM (Llama-4) | .119 | .183 | .218 | .003 | .006 | .238 | .586 |
| OpenScholar (Llama-4) | .309 | .278 | .017 | .008 | .013 | .010 | .138 |
| Search AI (o3) | .849 | .348 | .610 | .165 | .026 | .425 | .495 |
| Search AI (Claude) | .698 | .307 | .583 | .131 | .008 | .701 | .760 |
| OpenAI DeepResearch | .857 | .392 | .629 | .187 | .124 | .399 | .138 |
| DeepScholar-base (GPT-4.1, o3) | .857 | .405 | .659 | .162 | .008 | .617 | .614 |
| DeepScholar-base (GPT-4.1, Claude) | .786 | .370 | .586 | .167 | .007 | .936 | .817 |

*Patel et al. (2025), Table 2, excerpt; the full table is in the [paper file](../raw/papers/08-deepscholar-bench.md). The human exemplars' verifiability scores likely significantly underestimate human writing, because the evaluation only has the title and abstract of each source they cite.*

The lecture reads the results by axis (≈55:32–56:17), and the paper's analysis agrees (§5.2.1):

- **Knowledge synthesis.** OpenAI DeepResearch does well — best among prior methods on Organization (.857) and Nugget
  Coverage (.392) — but every prior method scores below .40 on Nugget Coverage. Systems "can generate well-organized and
  coherent summaries, they still struggle to extract and surface key facts". In the lecture's words: "it's great English,
  but not necessarily covering all the key facts".
- **Retrieval quality.** Relevance is reasonable — DeepResearch's .629 exceeds the human exemplars' — but Reference Coverage
  (.187) and Document Importance (.124) stay low: systems "struggle to find a comprehensive set of notable sources". The
  lecture's "less than 12.5%" is the highest Document Importance in the table.
- **Verifiability.** DeepScholar-base reaches a Citation Precision of .936 with GPT-4.1 and Claude — the lecture's "up to 90%
  precision" (≈56:17) — while DeepResearch, "despite the fact that it does a really good job at writing really nice English",
  has lower verifiability (.399 and .138). The paper puts DeepScholar-base's advantage over DeepResearch at 1.5–2.3 times on
  Citation Precision and 4.4–6.3 times on Claim Coverage (§5.2.2).

### Where systems fall short

The lecture's failure modes (≈56:17–57:56) come from the paper's ablation, which swaps in different retrievers and two
**oracle** settings that hand the system the human exemplar's important references (§5.3, Table 3):

| DeepScholar-base (GPT-4.1, Claude) | Org | Nug. Cov. | Rel. Rate | Ref Cov. | Doc Imp. | Cite-P | Claim Cov ($w=1$) |
|---|---|---|---|---|---|---|---|
| arxiv.org Retrieval | .786 | .370 | .586 | .167 | .007 | .936 | .817 |
| parallel.ai Retrieval | .865 | .444 | .675 | .160 | .017 | .846 | .781 |
| taviliy.com Retrieval | .929 | .327 | .550 | .070 | .015 | .711 | .578 |
| Oracle Retrieval (arxiv.org) | .782 | .487 | .686 | 1.000 | 1.000 | .955 | .899 |
| Oracle Retrieval (All) | .778 | .528 | .680 | 1.000 | .822 | .941 | .828 |

*Patel et al. (2025), Table 3, the DeepScholar-base (GPT-4.1, Claude) rows; the Llama-4 rows are in the paper file. "taviliy.com" is as printed.*

- **Finding comprehensive, foundational sources.** Even when agents find relevant documents, they miss the foundational
  papers an expert knows from experience; they "struggle to assess document importance beyond whether it's relevant"
  (≈56:17–57:07). With oracle retrieval the same pipeline "nearly saturates performance on Retrieval Quality and
  Verifiability", so retrieval is a large opportunity (§5.3).
- **Surfacing essential facts.** "even if you say, OK, here are all the papers that should have been included", systems
  get about 50% coverage of the key facts, and less without them (≈57:07). In Table 3 the oracle settings raise Nugget
  Coverage to .487 and .528, "a modest score": "even with high retrieval quality, existing LLM systems still struggle to
  effectively surface important facts and synthesize important insights" (§5.3).
- **Synthesis versus verifiability.** No system excels at both the quality of its synthesis and its verifiability (≈57:07).

So referring to reference data is a non-trivial problem in agentic evaluation: an agent may lack an expert's foundational
knowledge, may not extract the key information, and may not reach the precision–recall trade-off on citations that an
occupation requires (≈57:56).

**Why this benchmark?** A student asks why cover something this narrow (≈58:41). The lecturer: the lecture covered task length
and economic value, but for real-world tasks to be meaningful agents have to use knowledge bases well, and this is a
representative task for that (≈58:41–59:27). Another student describes the important references as a list collected from
human experts with PhDs (≈59:27). In the paper, the important references are the human exemplar's references as labelled by an
LLM judge, which agreed with human annotators 82% of the time (§3.2, Table 4). The lecturer adds that it is not only which
papers are retrieved but whether the section's claims — how this work differs from others — are backed by the citations
(≈1:00:16).

## Putting the three together

The lecture's synthesis (≈1:01:03–1:02:40): a research synthesis task might take a human 30 minutes to 8 hours, so it sits in
METR's long-horizon range, and tasks that "look good on that plot" at around 50 minutes and 50% success may still produce
poor quality. Research synthesis has clear economic value — in financial research, among others — but needs capabilities
current models do not always have: multi-step reasoning, comprehensive information gathering, and keeping verifiability while
combining the context from many tool calls, as in homework 3. "We are not just limited by task duration."

**Each benchmark leaves something out** (≈1:02:40–1:04:11):

- **METR** is automatically scored, involves no interaction with other agents, is not punishing of mistakes, and has no
  resource constraints. These are the paper's own list of systematic differences between its tasks and real tasks, which also
  includes static environments (§7.2.1). Real tasks can be scored more subjectively, be split across agents, and carry real
  resource constraints and costs of mistakes.
- **GDPval**'s tasks are fully specified, with all the context a human would have written into the prompt, and one-shot, with
  no back-and-forth to fix what the model did (§5). The lecture adds that this requires the model to already have tacit
  knowledge (≈1:03:25). The paper's limitations put it differently: tasks "that involve extensive tacit knowledge" are out of
  scope for the current evaluation (§5).
- **DeepScholar-Bench** covers the retrieval gap, and shows models still struggle to find comprehensive, high-quality sources,
  surface key facts and verify them — things an expert who does this daily has in their head (≈1:04:11).

**What we know, and what we don't** (≈1:04:11–1:06:34). Models have improved on isolated, well-specified tasks, especially in
software engineering and ML research — "we are automating away our own jobs" — and stronger models produce well-organized
outputs even when parts are wrong. Confidence is lower on tasks that need a lot of context or involve ambiguous prompts, on
adversarial environments, on 95% reliability, on generalization beyond software and knowledge work, and on finding and
verifying sources in a knowledge base with high accuracy and coverage.

**Summary** (≈1:06:34–1:08:07). METR's 50% horizon doubles every seven months, forecast to reach a month-long task between 2028
and 2031. GDPval's win rate has improved more linearly over two years, reaching 48% for some tasks while many categories stay
low, and many of its tasks need context or reference data. DeepScholar-Bench shows critical gaps when an agent must retrieve
context or read a knowledge base. Reliability and error recovery should keep improving, but performance will vary by task type,
and a longer horizon does not mean reliable, high-quality output. "So we need metrics which are duration-based, economic
value-based, and also synthesis quality-based. And you still need to validate them against humans who can solve these tasks."

## Closing Q&A

**Is the time-horizon trend like Moore's law?** (≈1:08:07–1:09:38) The lecturer's answer has two parts. SWE-bench Verified has
shown strong growth and coding will keep improving; but many tasks are not pull-request shaped, and the long tail stays hard.
The captions are partly inaudible here; the lecturer mentions Waymo and a last-20% remark, and names distributed systems as an
area of computer science where models find it very hard to get things right.

**How will your own work look in two years?** (≈1:09:38–1:11:16) AI timelines are moving too fast to predict a year out. An AI
scientist that forms hypotheses, runs experiments and closes the loop is not there yet; the lecturer's definition of AGI is "when
AI can build the next generation of models themselves". For now models are strong brainstorming partners — an "AI co-scientist"
— and getting the long tail of reliability will be extremely challenging in most areas.

**What does 50% success mean in METR?** (≈1:11:16–1:12:49) A student asks whether it is an average over all attempts on all
tasks, or at least 50% on every task. The lecturer answers that there are multiple agent runs per task, each task gets a success
rate from the fraction of successful attempts, and the points are plotted by horizon; a 50% task is "a flip of a coin". In the
paper, agents ran about 8 times per task (§3.3.2), and the 50% time horizon is where the logistic curve fitted to a model's
successes and failures across tasks of different human lengths predicts a 50% chance of success (§4.1). It is neither an average
across all tasks nor a per-task guarantee.

**Is software engineering going away?** (≈1:12:49–1:13:35) Computer science as a profession is not going anywhere; reasoning from
fundamentals matters, and AI needs someone to give it the context.

**What stops AI on the long tail?** (≈1:13:35–1:15:08) Both data and fundamental model capabilities. Startups are building
environments, and broad categories like legal and financial research have improved faster than very specific tasks where data is
scarce. In robotics, many startups treat data collection as the main way to close the gap.

## Related pages

- [Retrieval and deep research agents](retrieval-and-deep-research.md) — the deep research agents DeepScholar-Bench evaluates, and
  Search-o1's retrieval inside reasoning.
- [Agents and agentic workflows](agentic-workflows.md) — planning, tool use and error recovery, the capabilities METR credits for
  longer horizons.
- [Verifiers](verifiers.md) — LLM judges validated against humans (DeepScholar-Bench) and an automated grader for expert work
  (GDPval).
- [Scaling laws](scaling-laws.md) — METR's exponential trend in time horizon, against GDPval's roughly linear one.
- [Test-time scaling](test-time-scaling.md) — reasoning effort and $\text{best-of-}N$ on GDPval, and inference cost against human salary in
  METR.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md) — ReAct, which the lecture
  names among the tool-use advances behind longer horizons.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md) — Search-o1's deep research
  agent, the homework 3 design DeepScholar-Bench's task resembles.
