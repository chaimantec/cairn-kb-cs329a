---
title: RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning
authors: Jonas Gehring, Kunhao Zheng, Jade Copet, Vegard Mella, Quentin Carbonneaux, Taco Cohen, Gabriel Synnaeve
year: 2025
arxiv: https://arxiv.org/abs/2410.02089
license: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/)
source: arXiv LaTeX source (e-print), fetched 2026-09-14
pdf_pages: 23
course: CS329A lecture 4 (Learning from Feedback with Tools/Code) — site schedule row 4 reading
part: main body
companion: none — the appendices are not transcribed; see the arXiv version
---

# RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning — main body

Full text of Gehring et al. (2025), transcribed from the arXiv LaTeX source and reproduced under CC BY 4.0; copyright the authors. Figures are crops of the published PDF, shown with the paper's own caption; this knowledge base adds no description of its own, so what a figure shows is what its caption and the paper's text say. Citations are resolved to author–year; the bibliography is omitted — follow the arXiv link for it. The appendices are not transcribed in this knowledge base; they are in the arXiv version linked above.

## Contents

| § | Heading | Figures | Tables |
|---|---|---|---|
| — | Abstract | | |
| 1 | Introduction | Figure 1, Figure 2 | |
| 2 | Method | | |
| 2.1 | Iterative Code Synthesis | | |
| 2.2 | Reinforcement Learning with Execution Feedback | | |
| 3 | Experimental Results | | |
| 3.1 | Setup | | |
| 3.2 | Main Results | | Table 1 |
| 3.3 | Inference-time Behavior | Figure 3, Figure 4 | Table 2 |
| 3.4 | Ablation Studies | | Table 3 |
| 3.4.1 | Learning Iterative Code Synthesis | | |
| 3.4.2 | Single-turn Training | | |
| 4 | Related Work | | |
| 5 | Conclusion | | |

## Abstract

Large language models (LLMs) deployed as agents solve user-specified tasks over multiple steps while keeping the required manual engagement to a minimum. Crucially, such LLMs need to ground their generations in any feedback obtained to reliably achieve the desired outcomes. We propose an end-to-end reinforcement learning method for teaching models to leverage execution feedback in the realm of code synthesis, where state-of-the-art LLMs struggle to improve code iteratively compared to independent sampling. We benchmark on competitive programming tasks, where we achieve new state-of-the-art results with both small (8B parameters) and large (70B) models while reducing the amount of samples required by an order of magnitude. Our analysis of inference-time behavior demonstrates that our method produces LLMs that effectively leverage automatic feedback over multiple steps.

## 1 Introduction

**Figure 1.** Solve rates of Llama 3.1 Models after RLEF training on CodeContests, compared to previously reported results across sampling budgets (log scale).

![Figure 1 — solve rates after RLEF vs. prior work](../images/04-learning-from-feedback-with-tools-code/rlef-figure-1.png)

The consistent increase in capabilities of Large Language Models (LLMs) has prompted researchers and developers to benchmark and deploy them in increasingly complex environments (Brown et al., 2020; OpenAI, 2023; AI @ Meta, 2024). An emerging research direction is to employ LLMs as agents to solve tasks in multiple steps with little to no human oversight, querying external computation or data sources when needed or as dictated by manual scaffolding (Schick et al., 2023; Kapoor et al., 2024). For example, such autonomous use of LLMs is of interest for ensuring accurate answers to user queries with up-to-date information (Mialon et al., 2024), interaction with websites (Yao et al., 2022) or generating code to implement software features from high-level descriptions (Yang et al., 2024).

We posit that any decision-making agent offering a natural language interface has to possess two skills. First, the ability to accurately deduce a user's intent when prompted; for LLMs, this is typically achieved by fine-tuning to follow instructions according to user preferences (Ouyang et al., 2022; Rafailov et al., 2023). Second, feedback on intermediate results of the agent's actions has to be taken into account to arrive at the desired outcome. For example, a web page that contains a necessary bit of information might have gone offline, requiring another search engine query. In the context of code generation, feedback can provide information about implementation bugs as well as constraints that are inefficient or cumbersome to specify in full detail, e.g., software and hardware platform details or library dependencies. Intermediate feedback is therefore crucial to ground LLM generations in the concrete situations encountered at inference time.

**Figure 2.** **Left:** Overview of reinforcement learning with execution feedback (RLEF). The LLM is repeatedly prompted to implement code according to a problem description. Each attempt is evaluated on a public test set; upon failure, feedback is inserted into the conversation. If public tests are passing, or a specified turn limit is reached, execution on additional, private tests determines the reward. The model is then updated with PPO. **Right:** Example dialog with two model responses. Execution feedback hints at an inefficient first solution, to which the model responds to utilizing a cache. The code passing the public test sets will be evaluated on the full test set.

![Figure 2 — RLEF training loop and example dialog](../images/04-learning-from-feedback-with-tools-code/rlef-figure-2.png)

In this work, we aim to endow pre-trained LLMs with the aforementioned skills, task alignment and grounding in inference-time feedback, in the domain of code synthesis from natural language descriptions (Chen et al., 2021; Rozière et al., 2023). Here, feedback is naturally provided as the result of the execution of generated code in the form of error messages and unit test results. However, to date, utilizing such feedback for code generation with LLMs has failed to yield substantial improvements when taking computational demands into account; indeed, obtaining samples independently often results in higher accuracy for a fixed inference budget (Kapoor et al., 2024; Xia et al., 2024). As a test bed to investigate and improve grounding in execution feedback, we propose to frame code generation as an iterative task, repeatedly asking an LLM to produce code according to a provided natural language description (Figure 2). After each generation, code is evaluated on example test cases and the resulting feedback is provided as additional context for subsequent attempts. We thus obtain an interactive environment where actions correspond to code and observations correspond to execution feedback. Importantly, such a framing permits end-to-end optimization with reinforcement learning (RL) algorithms to maximize a reward signal – here, a binary reward based on whether the final code solution passes a set of held-out test cases.

We benchmark our training method incorporating repeated code actions and execution feedback in a reinforcement learning context (RLEF) on CodeContests (Li et al., 2022), a challenging competitive programming benchmark. Starting from Llama 3.1 models (AI @ Meta, 2024), we achieve substantial performance improvements, surpassing previous state-of-the-art results while reducing the amount of generations required by an order of magnitude (Figure 1). Our analysis shows that RLEF training unlocks the capability to leverage inference-time machine feedback, rendering LLMs effective in iterative, multi-turn scenarios. Our improvements from RLEF on CodeContests further generalize to HumanEval+ and MBPP+, two popular benchmarks for code synthesis, and to increased sample budgets compared to training time.

## 2 Method

### 2.1 Iterative Code Synthesis

We structure the task of code synthesis as a multi-turn conversation in which an LLM is repeatedly prompted to generate a code solution to a natural language problem description. After each solution, we provide an automatically generated response with results obtained by executing the solution's code against test cases. This setup is applicable to language models tuned for the common use-case of interacting with users in a chat setting, and follows previous work on self-repair for code generation (Shinn et al., 2023; Olausson et al., 2024).

Crucially, we utilize two different sets of test cases: a *public* test yields execution feedback that can be accessed during repeated attempts and forms the basis of selecting a final solution, whereas a *private* test set ultimately determines the correctness of the final solution. Separate test sets provide two main benefits. First, if test inputs and outputs are fixed, held-out tests guard against shortcuts during the optimization procedure in which an LLM can copy expected test outputs in subsequent answers, based on execution feedback. Second, running a full test suite may be computationally demanding and a limited set of public tests can accelerate the iterative code generation procedure. It may however be desirable to maximize test coverage for execution feedback at inference time, and we verify that this can indeed improve performance (Appendix B.2).

Our conversation flow for code generation is depicted in Figure 2. Concretely, we start the dialog with the problem description and query the LLM for an initial solution. The solution is verified against the public test set, which yields results in the form of passed and failed test cases, as well as potential syntax or runtime errors. If any public test fails, this execution feedback is formatted and appended to the dialog. The LLM is then queried for an updated code solution, with the original problem text, previous solutions, and their respective feedback provided in the prompt. If the solution passes all public tests, or a specified turn limit is reached, it is considered to be final and will be submitted for evaluation on the private test set. The kind reader is referred to Appendix C for a listing of our prompt and execution feedback templates.

### 2.2 Reinforcement Learning with Execution Feedback

The iterative code synthesis described in the previous section can be understood as a Markov Decision Process (MDP), and the language model as a policy (Sutton and Barto, 2018). For generality, we assume a partially observable MDP as our reward function utilizes a held-out, private test set which is not accessible to the policy (unless an exact textual representation of the desired program behavior is provided in the problem description). Observations and actions are provided as tokenized text sequences. Concretely, the initial observation $o_0$ is the problem description and actions $a_t$ at each step $t$ are textual responses. Successive observations $o_t$ consist of past observations and actions, including execution feedback obtained by evaluating the previous action $a_{t-1}$ on public test cases. Episodes terminate when public test evaluation succeeds or a specified step limit is reached. At the end of an episode, a scalar reward is provided corresponding to whether all public and private tests are passing. We do not use reward discounting (i.e., $\gamma=1$).

For optimizing a policy in the above environment we employ Proximal Policy Optimization (PPO), a common choice for fine-tuning large language models (Schulman et al., 2017; Ziegler et al., 2020; Ouyang et al., 2022). Following previous work, we include a KL penalty in our reward signal, acting both as an entropy bonus and as regularization towards the distribution of the LLMs we start from. In initial experiments we found that a possible failure mode concerns the generation of invalid code in non-final responses, which we address by providing a small penalty for invalid responses.

Denoting the policy to be optimized with $\pi$ and the initial policy with $\rho$, and abbreviating previous observations and actions with $c_t = o_0, a_0, o_1, a_1, \dots, o_{t}$ our reward function at step $t$ is

$$
R(s_t, a_t) = r(s_t, a_t) - \beta \log \frac{\pi(a_t \mid c_t)}{\rho(a_t \mid c_t)}, \quad r(s_t, a_t) = \begin{cases} 1, & \text{if end of episode and all tests pass} \cr -1, & \text{if end of episode and any test fails} \cr -0.2, & \text{if } a_t \text{ does not contain valid code} \end{cases}
$$

with a constant $\beta$ trading off between task reward and KL maximization. For PPO, we compute policy gradients by incorporating a concurrently learned value function as a baseline, i.e., we train the policy to maximize the advantage $A_t = - V(c_t) + \sum_{i=t}^T R(s_i, a_i)$; see Appendix A.1.

We note that while the above MDP considers full responses as actions, the underlying policy and value functions are implemented as language models outputting single tokens. Selecting a suitable action space for optimization hence requires consideration in our setup, and a suitable choice may depend on the concrete task at hand. We propose to model the policy at the token level while learning a value function for whole turns; compared to optimizing both models at either the turn or token level, this hybrid approach worked best in our early experiments. Hence, we predict the value of a response $a_t$ from the last token of its respective prompt, and we use a single advantage value for each token action within a response. Our response-based value estimation is closely related to Zhou et al. (2024); however, we do not train an additional Q-function. For the KL penalty, we found it beneficial to compute the probabilities of responses $\pi(a_t \mid c_t)$ as the geometric mean rather than product of token probabilities. This counteracts a possibly detrimental bias towards shorter generations, in particular for non-final responses.

## 3 Experimental Results

### 3.1 Setup

We perform experiments on the CodeContests benchmark introduced by Li et al. (2022) which requires generating a code solution to a problem specified in natural language along with a textual description of public test cases. Problems are of high difficulty and used in human competitive programming with a focus on algorithms, data structures and runtime efficiency. The correctness of solutions is evaluated with private tests that are hidden from contestants, which we implement in our setup by presenting feedback from public tests only. The CodeContests dataset consists of a training set and two test sets, "valid" and "test", consisting of 117 and 165 problems, respectively; we use the former for model and hyperparameter selection. We optimize our models on the training set, from which we discard 669 of the 13,328 problems due to missing public or private test cases. We prompt and train all models to output Python 3 code.

The Llama 3 family of models (AI @ Meta, 2024) comprises our initial policies, specifically the Instruct 8B and 70B parameter models of the 3.0 and 3.1 release. These models exhibit strong code generation performance out of the box and are able to follow instructions in the prompt, alleviating the need for an initial fine-tuning stage prior to RL training. During training and for evaluations, unless noted, we set the turn limit to allow for 3 LLM attempts at solving each problem. We perform 12,000 and 8,000 updates to the 8B and 70B models, respectively, and select checkpoints based on valid set performance. Hyper-parameters and further experimental details are provided in Appendix A.

We follow Li et al. (2022) in reporting results as $n\text{@}k$ average solve rates. The $n\text{@}k$ metric represents the expectation that any of $n$ solutions, selected from $k$ samples in total, is correct, i.e., passes all tests. In our multi-turn setup, each turn counts as a sample. This allows for fair comparisons with respect to sample budgets, which is particularly relevant when employing large LLMs with high inference cost in agentic scaffoldings (Kapoor et al., 2024).[^1]

[^1]: For simplicity, we consider a full LLM response as a single sample in our evaluations. We also note that for iterative code generation, the allocated sample budget may not be fully utilized as a successful public test run will result in early termination of a dialog.

### 3.2 Main Results

**Table 1.** Results on CodeContests of our initial and RLEF-trained models compared to prior work. The sample budget $k$ in $n\text{@}k$ refers to the number of LLM responses, e.g., $\text{1@3}$ for our results corresponds to a single rollout with up to three model responses. Best results per sample budget (up to 10, up to 100) in bold. The 70B model obtains state-of-the-art results after RLEF, and significantly outperforms AlphaCodium and MapCoder generally, and on the test set with a fraction of the samples. The RLEF-trained 8B model outperforms AlphaCodium with 100 samples and MapCoder (gpt-3.5-turbo) with 3 samples.

| Model | Source | $n\text{@}k$ | Valid Set | Test Set |
|---|---|---|---|---|
| AlphaCode 9B | Li et al. (2022) | $\text{10@1000}$ | 16.9 | 13.3 |
| AlphaCode 41B + clustering | Li et al. (2022) | $\text{10@1000}$ | 21.0 | 16.4 |
| Code Llama 34B + PPO | Xu et al. (2024) | $\text{10@1000}$ | 19.7 | 22.4 |
| AlphaCodium gpt-3.5-turbo-16k | Ridnik et al. (2024) | $\text{5@100}$ | 25 | 17 |
| AlphaCodium gpt-4-0613 | Ridnik et al. (2024) | $\text{5@100}$ | 44 | 29 |
| MapCoder gpt-3.5-turbo-1106 | Islam et al. (2024) | $\text{1@23}$ | - | 12.7 |
| MapCoder gpt-4-1106-preview | Islam et al. (2024) | $\text{1@19}$ | - | 28.5 |
| Llama 3.0 8B Instruct | Ours | $\text{1@3}$ | 4.1 | 3.2 |
| + RLEF | Ours | $\text{1@3}$ | 12.5 | 12.1 |
| Llama 3.1 8B Instruct | Ours | $\text{1@3}$ | 8.9 | 10.5 |
| + RLEF | Ours | $\text{1@3}$ | 17.2 | 16.0 |
| Llama 3.1 70B Instruct | Ours | $\text{1@3}$ | 25.9 | 27.5 |
| + RLEF | Ours | $\text{1@3}$ | **37.5** | **40.1** |
| Llama 3.1 8B Instruct | Ours | $\text{10@100}$ | 21.7 | 24.8 |
| + RLEF | Ours | $\text{10@100}$ | 29.8 | 28.7 |
| Llama 3.1 70B Instruct | Ours | $\text{10@100}$ | 50.2 | 50.3 |
| + RLEF | Ours | $\text{10@100}$ | **54.5** | **54.5** |

In Table 1 we list our solve rates on the CodeContest valid and test sets for iterative code generation with up to three turns, along with previously reported results. When sampling from our models, we use temperatures 0.2 for $\text{1@3}$ and 1.0 for $\text{10@100}$, and nucleus sampling with top-p 0.95 in all cases (Holtzman et al., 2020). Each solve rate is estimated on 200 rollouts, using the estimator described in (Li et al., 2022). We compare against AlphaCode (Li et al., 2022) and PPO with rewards from test execution on the Code Llama 34B model from Xu et al. (2024), which both report results with a large number of samples. AlphaCodium (Ridnik et al., 2024) and MapCoder (Islam et al., 2024) are high-performing agentic frameworks built on top of the proprietary GPT models and combine chain-of-thought prompting, code execution, program repair, and, in the case of AlphaCodium, automatic test generation.

With RLEF training we improve markedly on the original Llama 3.1 models and outperform prior works by a significant margin. Notably, on the test set the 70B model beats AlphaCodium with GPT-4, the previous state-of-the-art, with a single rollout compared to 5 solutions from 100 samples (38.0 and 29). Likewise, the 8B model with RLEF is slightly ahead compared to the similar-sized AlphaCode 9B model (16.0 and 13.3), but with a sample budget of 3 in our case and 1,000 for AlphaCode. While we cannot compare directly to the more recent AlphaCode 2 (AlphaCode Team, 2023), a performance estimate of 34.2 on the valid set for $\text{10@100}$ puts our 70B model ahead (37.5) with just 3 samples.[^2] When considering a larger budget of 100 samples – corresponding to 33 rollouts – the stock 70B model beats all previously reported results, including AlphaCodium on the valid set. With RLEF training, we obtain further improvements to 54.5 on the valid and test set. The relative improvements over the initial models, while still significant, are reduced in the $\text{10@100}$ setting as compared to the $\text{1@3}$ setting. Kirk et al. (2024) observe that RL training of LLMs can reduce the diversity of outputs and we interpret our results as further evidence of their hypothesis.

[^2]: AlphaCode Team (2023) train and evaluate on non-disclosed competition problems but report a sample efficiency increase of 10,000x over AlphaCode, which achieves a 10@1M solve rate of 34.2 on the valid set.

Table 1 also highlights that the released Llama 3.1 models offer competitive performance on CodeContests from the start, which we attribute to a focus on coding capabilities during instruction tuning (AI @ Meta, 2024). However, our RLEF method is also highly effective on the previously released 3.0 8B model, improving $\text{1@3}$ solve rates from 4.1 to 12.5 and 3.2 to 12.1 on the valid and test set, respectively. Thus, RLEF may be useful as a partial substitute for instruction tuning for tasks where automatic evaluation is possible.

### 3.3 Inference-time Behavior

**Table 2.** $\text{1@3}$ solve rates in single-turn (ST) and multi-turn (MT) setups for base and RLEF models. On CodeContests, iterative code generation yields modest gains at best and drops in performance at worst, unless RLEF training is employed. Improvements from RLEF on CodeContests in the multi-turn setting carry over to HumanEval+ and MBPP+, which require a slightly different execution feedback formatting. Solve rates estimated on 20 rollouts per problem, temperature 0.2.

| Model | CC. Test ST | CC. Test MT | HumanEval+ ST | HumanEval+ MT | MBPP+ ST | MBPP+ MT |
|---|---|---|---|---|---|---|
| Llama 3.1 8B Instruct | 11.8 | 10.5 | 65.3 | 63.9 | 58.3 | 60.5 |
| + RLEF | 9.7 | 16.0 | 67.5 | 69.5 | 57.0 | 63.1 |
| Llama 3.1 70B Instruct | 26.2 | 27.4 | 73.2 | 75.0 | 66.9 | 70.2 |
| + RLEF | 30.3 | 40.1 | 78.6 | 80.4 | 67.6 | 72.2 |
| gpt-4o-2024-05-13 | 25.3 | 24.3 | 82.8 | 80.7 | 68.8 | 71.7 |

*The source's "CC. Test", "HumanEval+" and "MBPP+" column headers each span two sub-columns (ST, MT) via `\multicolumn`; here the spanning header is repeated for each sub-column, and the blank spacer columns used purely for layout in the source are omitted.*

In Table 2 we first take a closer look at single- and multi-turn performance with a fixed budget of 3 LLM generations ($\text{1@3}$). This corresponds to our iterative setup with up to three model responses, or three independent responses for single-turn results. We further consider generalization to two popular code generation benchmarks, HumanEval+ and MBPP+ (Liu et al., 2023b), which we modify to match our iterative code generation setup with "base" tests for inference-time execution feedback and "plus" tests for solve rate estimation (see Appendix C.4 for details). Our results demonstrate that, when considering a fixed sample budget, base models rarely benefit from access to faulty solutions and execution feedback in the multi-turn code generation setup. This also applies to gpt-4o-2024-05-13, which shows stronger performance when sampling solutions independently on CodeContests and HumanEval+. After RLEF training, the 8B and 70B Llama 3.1 model both benefit from execution feedback and can therefore achieve larger gains on top of improved single-turn scores, with the exception of the 8B model on CodeContests and MBPP+ where single-turn performance drops. While multi-turn gains from RLEF are most pronounced on CodeContests, the training domain of our models, we also observe notable improvements on HumanEval+ and MBPP+.

**Figure 3.** Behavior analysis of initial and RLEF-trained models with respect to public test results, for 8B (top) and 70B (bottom) models. Within 20 rollouts per problem (5640 in total) we count errors in the initial solution (turn 1); errors turned into correct code in turn 2 and 3; code changes across successive solutions according to the chrF metric. RLEF-trained models make fewer errors initially, can fix errors more reliably and perform larger code edits; initial models frequently repeat previous solutions. With random execution feedback, error recovery is severely impaired.

![Figure 3 — error and repair behavior with RLEF](../images/04-learning-from-feedback-with-tools-code/rlef-figure-3.png)

Next, we seek to determine where the gains of RLEF training stem from. Based on the improved single-turn results in Table 2 we hypothesize that, for the 70B model, these are partly due to training on the specific domain of competitive programming questions. More importantly, higher scores in the iterative setting for both the 8B and 70B model could be attributed to either an increased capability of sampling diverse solutions within a rollout, or more targeted self-repair based on execution feedback. For probing the sensitivity of our models to the observed feedback, we perform inference-time ablations with *random* execution feedback. We implement random feedback by executing a faulty solution to an unrelated problem, but still end the dialog if the current solution passes public tests (details in Appendix C.2).

In Figure 3 we consider errors on public tests (to which the execution feedback relates) over 20 rollouts on the valid and test set combined. We observe that after RLEF training, both the 8B (top row) and 70B (bottom row) models produce fewer wrong outputs in their initial response but are more prone to exceeding the allocated time limit. In subsequent responses, recovery from all error categories is significantly improved. With random feedback, however, we see a clear impairment of self-repairs, demonstrating that RLEF allows LLMs to effectively leverage the provided feedback. We further gauge changes from one response to the next by computing the a character n-gram F-Score (Popović, 2015, chrF) among successive codes (Figure 3, right). This underscores a shortcoming of the Instruct models without RLEF in that they perform only minimal code edits; indeed, we observe that they frequently output the same code solution despite inline feedback pointing out errors.

The analysis of Figure 3 above suggests that, with RLEF, samples within a rollout are of higher diversity (less similar codes) but that edits are also targeted in that random execution feedback results in fewer successful repairs. This finding is echoed in Figure 4a, in which we compare models with true and random feedback across different turn limits. Here, we compare pass@1 and pass@10 metrics, irrespective of different sample budgets due to varying turn limits (Chen et al., 2021). While pass@1 captures the precision with which we arrive at a correct final solution, pass@10 reflects the ability to recall a correct solution (i.e., whether any of 10 solutions passes the private tests). On both valid and test sets, random feedback results in a drop in pass@1 which is amplified as the turn limit is increased. This provides further evidence for less targeted repair capabilities with random feedback, as programs can be repaired less reliably. Notably, with ground truth feedback, the probability of producing a correct solution keeps increasing with higher turn limits. For pass@10, the difference between true and random execution feedback is less pronounced. As this metric can be optimized by sampling many diverse candidate solutions within a dialog, these results indicate that with random feedback, our models resort to sampling a succession of diverse, potentially correct solutions.

Finally, we evaluate the generalization across turn limits with respect to a given sample budget. In Figure 4b, we perform rollouts with temperature 1.0 to emphasize performance at higher sample budgets by increasing the diversity of generations. We compute $10\text{@}k$ solve rates by distributing $k$ samples equally across rollouts with different turn limits. For the 8B model (top row), prior to RLEF training, best performance can be obtained with independent samples (1 turn), with the exception of the test set above 30 samples. The initial 70B model performs better with 3 or 5 turns, although, for small budgets, single turn performance is competitive. After RLEF, we observe that 3, 5 and 10 turns yield a consistent improvement over independent sampling, with best performance obtained with 5 turns. In all cases, increasing the turn limit to 10 provides no benefits under a fixed sample budget.

**Figure 4.** **(a)** Pass@1 and pass@10 across turn limits with RLEF-trained models, providing either true or random execution feedback (temperature 0.2). With random feedback pass@1 is reduced while pass@10 suffers only slightly, indicating that programs can be repaired less consistently. **(b)** Impact of turn limits on $10\text{@}k$ solve rates per sample budget (top: 8B model, bottom: 70B model) with temperature 1.0. With RLEF, iterative code generation can leverage up to 5 turns to achieve compute-optimal performance.

![Figure 4 — pass@1/pass@10 and turn-limit ablations](../images/04-learning-from-feedback-with-tools-code/rlef-figure-4.png)

### 3.4 Ablation Studies

**Table 3.** $\text{1@3}$ solve rates starting from Llama 3.1 models, temperature 0.2. **(a)** Comparison of different methods for acquiring the iterative code synthesis capabilities. RLEF is the most effective training method, followed by supervised fine-tuning (SFT). We find few-shot prompting to be detrimental to Instruct models. **(b)** Conventional single-turn (ST) compared to our multi-turn (MT) training with our RL loop. MT training yields larger improvements compared to ST, and improvements carrying over to multi-turn over single-turn inference is restricted to the 70B model.

**(a)**

| Model | Method | Valid | Test |
|---|---|---|---|
| 8B Instruct | – | 8.9 | 10.5 |
|  | Few-Shot | 8.5 | 8.5 |
|  | SFT | 10.3 | 10.0 |
|  | RLEF | 17.2 | 16.0 |
| 70B Instruct | – | 25.9 | 27.5 |
|  | Few-Shot | 22.5 | 20.3 |
|  | SFT | 27.7 | 27.2 |
|  | RLEF | 37.5 | 40.1 |

**(b)**

| Model | Training | Valid ST | Valid MT | Test ST | Test MT |
|---|---|---|---|---|---|
| 8B Instruct | – | 9.4 | 8.9 | 11.6 | 10.5 |
|  | ST | 10.3 | 10.2 | 9.9 | 10.9 |
|  | MT | 16.2 | 17.2 | 9.5 | 16.0 |
| 70B Instruct | – | 25.6 | 25.9 | 25.9 | 27.5 |
|  | ST | 28.3 | 31.1 | 27.3 | 32.9 |
|  | MT | 25.8 | 37.5 | 30.3 | 40.1 |

*The source's "Valid" and "Test" column headers in (b) each span two sub-columns (ST, MT) via `\multicolumn`; here the spanning header is repeated for each sub-column. The model name "8B Instruct" / "70B Instruct" is split across two source rows by a manual line break inside the cell; here it is written once, in the first row of each group.*

#### 3.4.1 Learning Iterative Code Synthesis

We investigate whether LLMs can, apart from our RL training, be effective in multi-turn code generation using few-shot prompting (Brown et al., 2020) and supervised fine-tuning (SFT). Lacking suitable ground truth training examples for SFT, we mine rollouts on the CodeContests training set with Llama 3.1 70B Instruct and filter them based on the correctness of final solutions. We then fine-tune Base and Instruct versions of the Llama 3.1 8B and 70B parameter models on the mined corpus and also source it for few-shot examples (Appendix A.3). The results in Table 3a show that few-shot prompting is detrimental to the instruction-tuned models. In Appendix B.1 we report few-shot $\text{1@3}$ solve rates for pre-trained models and find that they achieve lower performance compared to zero-shot prompting for instruction models (1.2 and 1.8 for 8B, 4.6 and 5.8 for 70B on valid and test set, respectively). Supervised fine-tuning improves Instruct model performance on the validation set only; we do not see improvements on the test set. For pre-trained models, we see improvements from SFT but lower scores compared to instruction-tuned models (Appendix B.1). With RLEF we obtain significantly higher solve rates compared to SFT models, underscoring the efficacy of our RL training loop.

#### 3.4.2 Single-turn Training

In Table 3b we compare our iterative code generation setup to traditional, single-turn generation where the model is not presented with inference-time feedback. We use the same training loop for single generations, albeit without the penalty for invalid code (Section 2.2) as this is subsumed by the reward signal for incorrect solutions. For Llama 3.1 Instruct 8B, single-turn training (ST) hurts performance on the test set. The 70B model benefits from single-turn training and improves over multi-turn SFT results in Table 3a. Moreover, we observe transfer in that applying the single-turn model in a multi-turn setting improves $\text{1@3}$ solve rates. We attribute this to the existent but comparabily weak multi-turn capabilities of the vanilla 70B Instruct model. Overall, we see strongest performance with the RLEF method employing multiple turns at training and inference time.

Further ablations can be found in the appendix. In Appendix B.3 we evaluate the effect of training a dedicated repair model on outputs of the single-turn 8B training run in Table 3b, similar to (Le et al., 2022). Together, the single-turn and repair model obtain $\text{1@3}$ solve rates of 14.8 on the validation set and 12.6 on the test set; an improvement over the single-turn model alone (10.2 and 10.9) but significantly below the corresponding multi-turn model (17.2 and 16.0). In Appendix B.4 we show that withholding public test execution feedback during training results in significantly worse performance. Finally, in Appendix B.5 we experimentally validate the design choice of a turn-level value function (Section 2.2).

## 4 Related Work

Generating program code with LLMs to automate and assist software development has been studied extensively in recent years, with evaluations predominantly focusing on code synthesis from natural language descriptions (Clement et al., 2020; Chen et al., 2021; Austin et al., 2021). A major boost in performance is obtained by including large quantities of source code in pre-training and selecting or generating suitable data for subsequent fine-tuning for instruction following (Li et al., 2023; Gunasekar et al., 2023; Rozière et al., 2023; AI @ Meta, 2024).

More recently, several works investigated prompting and flow engineering techniques to improve performance at inference time, including the verification of generated code via compilation and execution, followed by re-prompting. Shinn et al. (2023) and Chen et al. (2024b) use feedback from unit tests to correct previously wrong generations and found it crucial to include model-generated error analysis in the prompt for successive generations. LDB (Zhong et al., 2024), AlphaCodium (Ridnik et al., 2024) and MapCoder (Islam et al., 2024) can be regarded as agentic frameworks as they provide rich manual scaffolding for code generation, chaining several LLM calls (e.g., for chain-of-thought planning, test generation, and program repair) combined with code execution. These approaches are effective on difficult benchmarks, such as the CodeContests dataset we consider in this work, but significantly increase inference cost by requiring dozens of LLM calls per solution.

Recent works highlight further issues with scaffolds like AlphaCodium or MapCoder. Olausson et al. (2024) show that sampling code solutions independently is competitive to repairing faulty code, that large models are required to provide effective feedback on errors, and that multiple rounds of repair are not effective. Kapoor et al. (2024) focus on inference cost and demonstrate that independent sampling beats the approaches from Shinn et al. (2023) and Zhong et al. (2024) when considering equal sampling budgets. With our method, the self-repair capabilities of LLMs can be dramatically enhanced, resulting in superior performance of iterative code generation for both small and large sample budgets. At the same time, we propose to trade complex, domain-specific prompt engineering and scaffolding for domain-specific fine-tuning.

Fine-tuning large language models with reinforcement learning is a popular method for aligning their output to user preferences (Ziegler et al., 2020; Touvron et al., 2023; OpenAI, 2023; DeepSeek-AI et al., 2024; AI @ Meta, 2024). Here, the learning signal is provided by special-purpose reward models. For code synthesis, however, rewards can be determined by executing LLM generations against available test cases (Le et al., 2022; Shojaee et al., 2023; Dou et al., 2024; Yu et al., 2024). Le et al. (2022) pre-train an LLM for code generation and subsequently fine-tune it with both policy gradients and next-token loss on rewards from execution. From the rollouts obtained during fine-tuning, they train further models for predicting test outcome labels and for mapping incorrect to ground truth solutions, which allows for inference-time code correction based on test results ("critic sampling"), albeit without explicitly presenting the output from execution. Subsequently, Liu et al. (2023a) extends this work with an extended, fine-grained reward function. Finally, Xu et al. (2024) fine-tune a stronger, code-specific LLM in a simpler setup with a binary reward from unit tests and observe substantial improvements from RL on the difficult competitive programming benchmark we consider here. We likewise propose a simple setting without extra inference scaffolding or usage of ground truth solutions. Crucially, we expand the natural-language-to-code setting to an iterative environment where execution feedback is not only provided as a scalar reward but also in textual form. This allows us to acquire both code synthesis and code repair capabilties with a single model, and to shift focus from large-sample inference regimes to obtaining high accuracy with low sample budgets.

Concurrently to our work, Kumar et al. (2024) propose a two-stage RL method (SCoRe) to improve the self-correction capabilities of LLMs and train them to output two successive solutions. In contrast to our method, SCoRe does not leverage execution feedback at inference time and instead asks the model to reconsider its initial solution. While this approach allows for potential applications to domains where automatic feedback is not available, it cannot benefit from the information provided in the feedback message. Furthermore, inference-time feedback can help the model generalize to new environments after training. Finally, Chen et al. (2024a) address code generation with human feedback and develop an appropriate supervised fine-tuning strategy based on training a separate code repair model. In our work, we effectively leverage automatically generated feedback, formatted in natural language, with a single model only.

Past work on applying reinforcement learning to LLMs on longer-horizon decision-making tasks placed an emphasis on acquiring the necessary grounding in the environment. Carta et al. (2023) report that RL tuning with PPO (Schulman et al., 2017) is superior to supervised training for grounding in text-based navigation games as measured by successful task completions. Zhou et al. (2024) propose a family of RL algorithms for LLMs and test them in text games (versus an oracle LLM) and for buying produces using a simplified web shop API, and Zhai et al. (2024) tackle environments with visual observations, adapting the parameters of a pre-trained vision LLM. While our work follows similar motivations, we address a fundamentally different domain – code synthesis – which features a significantly larger action space compared to previous work, i.e., the space of valid Python programs.

## 5 Conclusion

In this work, we proposed reinforcement learning from execution feedback (RLEF), a fine-tuning method for LLMs that endows them with a crucial capability for autonomous operation: grounding future generations in environment feedback. We applied RLEF to iterative code synthesis and obtained substantial improvements in solve rates on the CodeContests competitive programming benchmark while reducing the required sample budget for inference. The RLEF-trained models further generalize to increased turn limits and to HumanEval+ and MBPP+, two popular code generation benchmarks that exhibit simpler programming questions and different execution feedback formatting. Our in-depth analysis revealed that, while an increase in correct first-turn generations and in the diversity of successive generations offers a major contribution of performance, our models also meaningfully take execution feedback into account and resolve errors over multiple turns.

**Limitations.** While our results demonstrate effective usage of inference-time feedback, the code synthesis task we consider is limited to improving a single solution to a given problem. Generalizing our method to environments with larger tasks that require decomposition, either via manual scaffolding or, eventually, in a self-directed manner, remains the subject of further research. Iterating on the execution results of unit tests naturally requires test cases, which may not be readily available. We regard a potential combination with automatic unit test generation (Watson et al., 2020; Jain et al., 2024) as an interesting avenue for further experiments.

**Broader Impact.** Successful grounding of LLMs for code generation execution feedback will amplify their utility when applied to impactful tasks such as assisting software development and performing quality control. In general, however, increasing the capabilities of LLMs, now widely deployed in a range of applications, requires quality control and guard-railing to promote safety and minimize potentially harmful output. We limit our study to the generation of source code, where we confine the execution of model-generated output to local sandboxes. We believe the framework of Shavit et al. (2023) regarding the governance of AI agents to be a useful resource for practitioners.

**Reproducibility Statement.** We perform all experiments with publicly available models and datasets. Section 3.1 describes the dataset and pre-processing steps, the exact Llama model versions used, and details our evaluation metric. The loss function and hyper-parameters for training, as well as a description of the compute infrastructure can be found in Appendix A.1. Appendix A.3 describes (narrow) hyper-parameter ranges for supervised fine-tuning, and Appendix A.2 contains notes regarding code execution during training and evaluation. All prompts are listed in Appendix C.

**Acknowledgements.** We thank Chris Cummins, Olivier Duchenne, Fabian Gloeckle, Baptiste Roziere, Sten Sootla, Nicolas Usunier, and Sida Wang for helpful technical contributions, suggestions, and insightful discussions.
