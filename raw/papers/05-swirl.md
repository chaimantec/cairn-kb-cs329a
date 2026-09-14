---
title: Synthetic Data Generation & Multi-Step RL for Reasoning & Tool Use
authors: Anna Goldie, Azalia Mirhoseini, Hao Zhou, Irene Cai, Christopher D. Manning
year: 2025
arxiv: https://arxiv.org/abs/2504.04736
license: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/)
source: arXiv LaTeX source (e-print), fetched 2026-09-14
pdf_pages: 33
course: CS329A lecture 5 (Planning and Multi-Step Reasoning) — site schedule row 5 reading
part: main body
companion: none — the appendices are not transcribed; see the arXiv version
---

# Synthetic Data Generation & Multi-Step RL for Reasoning & Tool Use — main body

Full text of Goldie et al. (2025), transcribed from the arXiv LaTeX source and reproduced under CC BY 4.0; copyright the authors. Figures are crops of the published PDF, shown with the paper's own caption; this knowledge base adds no description of its own, so what a figure shows is what its caption and the paper's text say. Citations are resolved to author–year; the bibliography is omitted — follow the arXiv link for it. The appendices are not transcribed in this knowledge base; they are in the arXiv version linked above.

## Contents

| § | Heading | Figures | Tables |
|---|---|---|---|
| — | Abstract | | |
| 1 | Introduction | | |
| 2 | Methodology | | |
| 2.1 | Multi-Step Data Collection | Figure 1 | |
| 2.2 | Step-Wise Reinforcement Learning Methodology | Figure 2 | |
| 2.3 | Step-Wise Inference-time Evaluation | Figure 3 | |
| 3 | Related Work | | |
| 4 | Experiments | | Table 1 |
| 4.1 | Evaluation Datasets | | |
| 4.2 | Results and Discussion | Figure 4, Figure 5, Figure 6, Figure 7, Figure 8 | Table 2, Table 3 |
| 5 | Conclusion | | |

## Abstract

Reinforcement learning has been shown to improve the performance of large language models. However, traditional approaches like RLHF or RLAIF treat the problem as single-step. As focus shifts toward more complex reasoning and agentic tasks, language models must take multiple steps of text generation, reasoning and environment interaction before generating a solution. We propose a synthetic data generation and RL methodology targeting multi-step optimization scenarios. This approach, called Step-Wise Reinforcement Learning (SWiRL), iteratively generates multi-step reasoning and tool use data, and then learns from that data. It employs a simple step-wise decomposition that breaks each multi-step trajectory into multiple sub-trajectories corresponding to each action by the original model. It then applies synthetic data filtering and RL optimization on these sub-trajectories. We evaluated SWiRL on a number of multi-step tool use, question answering, and mathematical reasoning tasks. Our experiments show that SWiRL outperforms baseline approaches by 21.5%, 12.3%, 14.8%, 11.1%, and 15.3% in relative accuracy on GSM8K, HotPotQA, CofCA, MuSiQue, and BeerQA, respectively. Excitingly, the approach exhibits generalization across tasks: for example, training only on HotPotQA (text question-answering) improves zero-shot performance on GSM8K (a math dataset) by a relative 16.9%.

## 1 Introduction

Large Language Models (LLMs) have demonstrated remarkable capabilities in Natural Language Processing (Gemini Team et al., 2024; Anthropic, 2024; OpenAI et al., 2024). However, they often struggle to answer complex queries that require reasoning and tool use across multiple steps (Wu et al., 2024), such as multi-hop question-answering, mathematical problem-solving, coding, and other agentic tasks, (Yang et al., 2018; Trivedi et al., 2022; Wu et al., 2024; Cobbe et al., 2021; Jimenez et al., 2024; Ehrlich et al., 2025; Li et al., 2022).

Traditional reinforcement learning (RL) approaches, such as RL From Human Feedback (RLHF) (Christiano et al., 2023), RL from AI Feedback (RLAIF) (Bai et al., 2022), and RL from Execution Feedback (RLEF) (Gehring et al., 2025), have focused on single-step optimization, leaving the challenge of multi-step tasks largely unaddressed. Many real-world problems require a sequence of interrelated actions; for example, when answering a challenging question, a model must determine not just what information to seek, but when to stop searching and synthesize its findings. Multi-step reasoning creates a compounding challenge, as incorrect intermediate steps often lead to incorrect final results, making it critical to maintain accuracy across the entire chain of actions or learn to effectively recover from such errors.

To address this challenge, we present Step-Wise Reinforcement Learning (SWiRL), an offline multi-step optimization technique. We consider a setting where the model has access to a tool, such as a search engine or calculator, and can run a sequence of tool use calls as needed to answer the question. Our goal is to teach the model how to decompose complex problems into a sequence of more manageable subtasks, when to call the tool, how to formulate a call to the tool, when to use the results of these queries to answer the question, and how to effectively synthesize its findings. In particular, we propose a two stage approach, in which we first generate multi-step synthetic data and then learn from these data using a step-wise reinforcement learning method. This approach has the key practical advantage that we can quickly generate large volumes of multi-step training data via parallel calls to avoid throttling the training process with slow tool use execution. In addition, this offline process enables greater reproducibility due to having a fixed dataset.

To generate multi-step synthetic training data, we provide an open-source LLM (Gemma 2 (Gemma Team et al., 2024b)) with access to a relevant tool (e.g., a search engine or calculator). We iteratively prompt the model to generate multi-step trajectories; at each step, the model is free to generate a chain of thought, and may either call a tool or produce a final answer, which we refer to as the model's action. If the model generates a tool use call, its query is automatically extracted from the overall response and executed in the environment, and the result is presented to the model in the next step. The trajectory ends when the model generates an answer to the original question, which it indicates using special markers. We convert each trajectory with $k$ actions into $k$ subtrajectories, containing the context from the beginning of the trajectory up to that action. We then use a step-wise reinforcement learning approach to optimize over this dataset, employing a generative reward model that evaluates each action in the context of its subtrajectory.

This granular approach enables us to apply direct feedback after each step of the trajectory, and to do so in a manner that is contextually aware. Unlike prior RL finetuning approaches used in frontier open-source models like DeepSeek-R1 (DeepSeek-AI and others, 2025) and Llama-3 (Grattafiori et al., 2024), we do not solely optimize for final performance, and use no golden labels; however, by optimizing for the reasonableness of each step given prior steps, SWiRL does in fact improve final performance.

In addition to evaluating SWiRL on challenging multi-hop question-answering and mathematical problem-solving tasks, we also study the generalization properties of this methodology. This is of key interest because there is an explosion of agentic applications for language models, and methods that generalize across datasets and tasks will be easier, cheaper and faster to adapt to new environments. We also measure the effectiveness of different synthetic data filtering strategies, study SWiRL's ability to generalize across datasets and tasks, measure the impact of model size and dataset size, and explore the mechanism driving these performance improvements.

Our contributions are as follows:

- We propose Step-Wise Reinforcement Learning (SWiRL), an approach to synthetic data generation and offline RL that advances multi-step reasoning and tool use.
- We demonstrate generalization across datasets. For example, training SWiRL on HotPotQA not only improves performance on the dataset itself, but also yields superior performance on other multi-hop question-answering datasets, e.g., 21.5% on GSM8K (Cobbe et al., 2021), 15.3% on BeerQA (Qi et al., 2021b), 11.1% on MuSiQue (Trivedi et al., 2022) and 14.8% on CofCA (Wu et al., 2024).
- We also show transfer across disparate tasks, namely mathematical reasoning to question-answering and vice versa. Training only on multi-hop HotPotQA question-answering improves performance on GSM8K (Cobbe et al., 2021) (a math dataset) by 16.9%, and training on GSM8K improves performance on HotPotQA (multi-hop question-answering) by 9.2%.
- We analyze the impact of synthetic data filtering strategies in a multi-step reasoning and tool use setting, and demonstrate that models learn best from datasets which have been filtered step-wise to ensure high-quality reasoning traces, but which are not filtered by outcome (correct final answer).
- We explore the impact of training dataset size and model size on SWiRL, observing that significant gains can be achieved even with just 1000 trajectories and that smaller models (Gemma-2-2b and 9b) can benefit from in-domain SWiRL, but do not display the same generalization as their larger counterpart, Gemma-2-27b.
- We demonstrate that SWiRL effectively improves the average process reward, even when evaluated on out-of-distribution tasks, suggesting that the downstream performance gains are driven by improved multi-step reasoning.

## 2 Methodology

Our methodology, Step-Wise Reinforcement Learning (SWiRL), consists of two stages. In the first stage, we generate and filter synthetic data. In the second stage, we use a step-wise reinforcement learning approach to optimize a generative base model on the synthetic trajectories. SWiRL does not require golden labels or human annotations, and instead relies entirely on model-based judgments for data generation, filtering, and RL optimization. The overall flow of our methodology is depicted in Figure 1 (Stage 1) and Figure 2 (Stage 2).

### 2.1 Multi-Step Data Collection

**Figure 1.** In SWiRL Stage 1, we generate and filter multi-step synthetic trajectories. At each step, the model is free to generate a chain of thought, call a tool such as a search engine or calculator, and/or produce an answer to original question. Process-filtered data corresponds to trajectories in which every step is judged to be reasonable by a model judge (Gemini 1.5 Pro Thinking). Outcome-filtered data corresponds to trajectories with a final answer that matches the golden label.

![Figure 1 — SWiRL Stage 1 synthetic data generation and filtering](../images/05-planning-and-multi-step-reasoning/swirl-figure-1.jpg)

In Stage 1 (see Figure 1), we generate synthetic trajectories consisting of multiple steps of reasoning and tool use, which we use as training data for the step-wise RL methodology described in the next section. To compile a large-scale collection of synthetic trajectories, we augment a language model with a tool (e.g., a search engine or calculator), and iteratively prompt the model to generate multi-step trajectories. At each step, the model is asked to choose whether to call a tool or produce a final answer, and is always free to generate chains of thought (which it typically does). If the model generates a tool use call, it is parsed from the overall response, executed in the environment, and the result is presented to the model in the next step. See Appendix E for the prompt, which contains a question, explicit instructions regarding multi-step tool utilization, and the results of prior tool use calls.

For each multi-step synthetic trajectory, we define the following annotations. The trajectory itself is denoted by $\tau = (s_1, a_1, \dots, s_K, a_K)$. The first state $s_1$ is the original prompt. Each following state $s_i$ contains the entire context so far, containing state $s_{i-1}$, action $a_{i-1}$, and the environment (tool call) response to $a_{i-1}$. Each action $a_i$ is the model response, given state $s_i$. The last action, $a_K$, is the model's answer to the original prompt.

In this work, we compiled a dataset of 50,000 synthetic trajectories seeded by 10,000 multi-step questions from the HotPotQA training set (Yang et al., 2018) (i.e., 5 trajectories per question), and a mathematical reasoning dataset of 37,500 synthetic trajectories seeded by the 7,500 questions in the GSM8K training set (Cobbe et al., 2021). Note that, for HotPotQA, we filtered out "Easy" questions, which can typically be answered with a single search query. To prevent synthetic trajectories from being excessively long, we set a maximum step count of 5 for HotPotQA questions, and 10 for GSM8K questions (which typically require 2-8 steps to solve).

Having compiled these datasets, we consider four different filtering strategies and measure their impact on performance (Figure 1): (1) No filtering; (2) Process filtering, where we retain trajectories in which each step was deemed reasonable given all previous steps. Concretely, a model (Gemini 1.5 Pro Thinking, in our case) is prompted to render a binary judgment as to whether action $a_i$ is reasonable given the context $s_{i}$. See Appendix E for our prompt. No golden labels are used; (3) Outcome filtering, where we select trajectories based solely on whether the final response, $a_K$, matches the golden answer; and (4) Process and outcome filtering, in which we take the intersection of both filtering approaches and retain only trajectories that exhibit both step-wise soundness and correct final outcomes.

Recent approaches to synthetic data distillation, such as Deep-Seek R1 (DeepSeek-AI and others, 2025), have demonstrated that synthetic data filtered for correct outcomes can lead to good performance with single-step RL and supervised finetuning (SFT). In this work, we sought to explore whether this pattern would hold in a multi-step, tool use setting, and to explore the impact of both outcome and process filters. Like these prior work, we observed that filtering multi-step trajectories for correctness was effective for SFT, and in fact critical for good performance. However, we found that SWiRL, unlike SFT, can learn even from trajectories that end in incorrect final answers. In fact, we achieve our best results by including process-filtered data, regardless of the correctness of the outcome.

### 2.2 Step-Wise Reinforcement Learning Methodology

**Figure 2.** In SWiRL Stage 2, we perform step-wise RL to train on the synthetic multi-step trajectories from Stage 1. Each step contains an action, which corresponds to a tool call or the final response. The model is free to generate chains of thought during each step. The environment responses are captured in the prior steps of the synthetic trajectories, which were generated offline. Granular feedback is provided by a generative reward model, which is used to perform RL optimization directly on each action, given the prior context.

![Figure 2 — SWiRL Stage 2 step-wise RL training](../images/05-planning-and-multi-step-reasoning/swirl-figure-2.png)

As shown in Figure 2, we propose a RL approach capable of learning effectively from the synthetic multi-step trajectories generated in Stage 1. At each step, a base model is optimized to predict either the next intermediate step or the final response based on preceding context. At each step $i$, the model has access to the full contextual history, including the original prompt, all previous model-generated steps and any applicable environment response corresponding to those steps.

Thus, our objective function is the expected sum of step-wise rewards:

$$J(\theta) = E_{s\sim \mathrm{T}, a\sim \pi_\theta(s)} \left[R(a \mid s) \right]$$

Here, $\pi_\theta$ is the base model parametrized by $\theta$, which is finetuned via SWiRL (Note that we also use $\pi_\theta$ to generate synthetic data.) $\mathrm{T}$ denotes the set of all states in the synthetic multi-step trajectories, i.e. each incremental state $s$ within each trajectory $\tau$. The reward signal $R(a \mid s)$ is derived from a generative reward model, specifically Gemini 1.5 Pro in our experiments, which assesses the quality of the generated response $a$ given the context $s$. No golden labels are used.

We optimize the above expected reward using the same policy gradient algorithm as used in Gemma 2 for optimizing the human feedback reward (Gemma Team et al., 2024a; Gemma Team et al., 2024b). Our granular, step-by-step finetuning paradigm enables the model to learn both local decision-making (next-step prediction) and global trajectory optimization (final response generation) while being guided by immediate feedback on the soundness of each prediction.

### 2.3 Step-Wise Inference-time Evaluation

As shown in Figure 3, at inference time, we iteratively prompt the model to either call a tool or produce a final answer. If the model generates a search query (indicated by `<search_query>` `</search_query>` tags), we parse out that query, embed it with a Gecko model, perform a nearest neighbor lookup in the corresponding vector database, and inject the retrieved article into the model's context window. If the model generates a calculator tool call (indicated by `<math_exp>` `</math_exp>` tags), we parse out the mathematical expression, execute it with a SymPy interpreter, and inject the calculated results into the context window. This process terminates when the model either produces an answer (signaled by producing `<answer>` `</answer>` tags) or reaches the maximum number of queries (5 for question-answering datasets, and 10 for mathematical reasoning datasets). See Appendix F for example trajectories.

**Figure 3.** At inference time, we iteratively prompt the model to call the tool as many times as necessary (up to a limit) before answering the original user question.

![Figure 3 — SWiRL inference-time multi-step tool use](../images/05-planning-and-multi-step-reasoning/swirl-figure-3.jpg)

## 3 Related Work

**Reinforcement Learning for LLM Finetuning.** One prominent approach, Reinforcement Learning from Human Feedback (RLHF) (Ouyang et al., 2022; Christiano et al., 2023), consists of training a reward model on human preference labels at the response level, followed by RL optimization using Proximal Policy Optimization (PPO) (Schulman et al., 2017). Building upon this framework, Reinforcement Learning with AI Feedback (RLAIF) (Bai et al., 2022) has emerged as a scalable alternative that leverages AI models to generate feedback based on predefined principles or constitutions, reducing the need for costly human annotations. RL from Execution Feedback (RLEF) (Gehring et al., 2025) uses environment feedback, such as pass rate on coding test cases, to calculate the reward, which it then optimized via PPO. Besides PPO, other RL optimizations, such as Direct Preference Optimization (DPO) (Rafailov et al., 2023) and its successors (e.g., Azar et al. (2023); Ethayarajh et al. (2024); Meng et al. (2024); Lanchantin et al. (2025)) as well as GRPO (Shao et al., 2024) have also proven to be effective for finetuning LLMs to maximize a target reward.

A limitation of the above approaches is that they focus on single-step optimization with the reward being calculated only at the end of the episode, leading to suboptimal performance for multi-step optimization (Liu et al., 2024; Wang et al., 2024). In SWiRL, we focus on scenarios where multiple steps of reasoning and tool calls are necessary prior to generating a response. Unlike the above methods, SWiRL enables the model to receive feedback on its granular step-wise actions which leads to better multi-step reasoning and tool use across longer horizons.

**Multi-step Optimization with RL.** Recent work including DQO (Liu et al., 2024) and OREO (Wang et al., 2024) propose offline reinforcement learning to improve multi-step reasoning for LLMs. However, neither focuses on enhancing a model's ability to use tools or interact with an external environment. Additionally, unlike our approach, which optimizes at the (reasoning) step level, DQO relies on token-level actions, which as shown in (Wang et al., 2024), are generally less effective than step-level actions. Moreover, OREO requires training a separate value network and policy, and relies on iterative co-optimization of both models. The process of maintaining, training, and serving these two models can be prohibitively expensive, particularly for larger models. PRIME (Cui et al., 2025) proposes an online approach to improve multi-step reasoning, but does not enable tool use or offline training. Tulu-3 (Lambert et al., 2025) uses verifiable rewards to train a language model to do better at math, but requires access to golden labels.

**Reasoning Improvement with Synthetic Data**. Several approaches have been proposed for generating synthetic reasoning data. These methods either rely on golden labels to filter the data or use a combination of golden labels and process or outcome reward models (Zelikman et al., 2022; Singh et al., 2024). For example, STaR (Zelikman et al., 2022) generates chain-of-thoughts (CoT) for reasoning questions, filters for those that result in correct answers, and performs Supervised Fine-Tuning (SFT) on those reasoning traces. The paper also proposes an augmentation technique called "rationalization", in which for each question the model answered incorrectly, the model is provided with the correct answer and prompted to generate a CoT that leads to that answer. Rejection finetuning (RFT) (Yuan et al., 2023) is another method that relies on collecting reasoning traces from the model and using those with correct outcomes for SFT. ReST (Gulcehre et al., 2023) demonstrates strong performance on machine translation by iteratively generating data and then finetuning on that data using either a supervised or reinforcement learning objective. $\text{ReST}^{\text{EM}}$ (Singh et al., 2024) is an extension of ReST which outperforms training on human data alone for math and coding evaluations, but which plateaus after a few iterations, presumably due to overfitting. Our method also uses a model-based approach to generate multi-step trajectories. However, we show that using a model to label the steps within each reasoning trajectory leads to higher out-of-domain generalization than using only the trajectories which contain correct final answers, meaning that we do not require golden labels. In addition, we enable the model to use tools iteratively to perform multi-hop question answering and mathematical reasoning.

**Process vs. Outcome Based Optimization**. There have been a number of attempts to compare the effectiveness of process and outcome-based approaches in the domain of math and reasoning (Lightman et al., 2023; Uesato et al., 2022; Snell et al., 2024). For example, (Lightman et al., 2023) showed that (Outcome Reward Models) ORMs are more effective than (Process Reward Models) PRMs at the task of ranking samples from a fixed generator model, whereas Uesato et al. (2022) demonstrated that outcome supervision yields comparable accuracy to process supervision at lower cost, but that the reasoning traces from the resulting model exhibit lower fidelity. Both rely on expensive human annotations and golden labels, and do not explore the impact of PRMs and ORMs in reinforcement learning optimization, or the differential effect of data filtering on supervised vs. RL optimization objectives.

## 4 Experiments

**Table 1.** Comparison of Accuracy (**PM**†: Partial Match) across Multiple Datasets: **HotpotQA**, **CofCA** (Average of 2-hop, 3-hop, and 4-hop), and **MuSiQue**. Baseline results were drawn from Wu et al. (2024). The Gemma-2 models, both SWiRL and the base model, were not given access to the context documents, but were allowed to sequentially query a vector database. The SWiRL model was trained on HotPotQA using process-filtered data, and for consistency with baseline results, evaluated on GPT-4o with the same prompts as Wu et al. (2024) on 300 randomly subsampled questions. See Appendix G for example ids.

| **Datasets** | HotpotQA | CofCA (Avg) | MuSiQue |
|---|---|---|---|
| **Metrics** | PM† | PM† | PM† |
| **Proprietary LLMs** | | | |
| **GPT-4** | 74.8 | 51.9 | 63.9 |
| **GPT-3.5** | 62.8 | 40.7 | 53.1 |
| **Gemini 1.0 Pro** | 63.5 | 33.3 | 46.9 |
| **Bing Chat** | 72.1 | 41.6 | 52.3 |
| **O1-preview** | **76.9** | **58.5** | **67.9** |
| **Open Source LLMs** | | | |
| **Llama 2-7b** | 38.5 | 28.9 | 34.2 |
| **Mistral-7b** | 34.9 | 25.6 | 29.2 |
| **Qwen 2-7b** | 39.3 | 30.7 | 33.5 |
| **Base Gemma 2-27b** | 58.6 | 31.7 | 35.4 |
| **SWiRL Gemma 2-27b (Ours)** | **67.8** | **39.3** | **43.6** |

*The "Proprietary LLMs" and "Open Source LLMs" rows are a single-cell `\multicolumn` group header in the LaTeX; each is shown here as a row with the bold label in the first cell and the remaining cells left empty.*

### 4.1 Evaluation Datasets

To evaluate performance on multi-step search tool use, we selected five challenging multi-hop question-answering and mathematical reasoning datasets:

- **HotPotQA** (Yang et al., 2018) is comprised of multi-hop questions from a variety of domains. Human annotators constructed the questions to be answerable only by combining information from multiple paragraphs of Wikipedia.
- **MuSiQue** (Trivedi et al., 2022) is a multi-hop question-answering dataset constructed by chaining together multiple single-hop questions.
- **CofCA** (Wu et al., 2024) is a multi-hop dataset constructed to be answerable only by querying a counterfactual version of Wikipedia. It contains 2- to 4-hop questions.
- **BeerQA** (Qi et al., 2021a) is an extension of HotPotQA designed to include an even greater number of hops than the original dataset.
- **GSM8K** (Cobbe et al., 2021) is a dataset composed of grade school math word problems, which typically take 2-8 steps to solve.

For question-answering datasets, we set up a vector database containing all articles from each data split using Gecko-1B with 768-dimensional embeddings (English) (Lee et al., 2024).

For the experiments in Table 1, we follow the same procedure as Wu et al. (2024), evaluating performance on 300 randomly subsampled examples from the target dataset, using the same language model as a judge (GPT4o) and the same prompt. For every other experiment in this paper, we used Gemma-2-27b as our judge, as this was more cost effective, with the exception of GSM8K for which we used Gemini 1.5 Pro as it exhibited noticeably better numeric evaluation. Model-based evaluation is emerging as a scalable and less brittle alternative to exact match and F1 metrics (Zheng et al., 2023; Gu et al., 2025), but does introduce a new source of stochasticity into the evaluation. See Appendix D for our own manual inspection and error analysis of three different model judges.

As described in Section 2.3, for each question, we iteratively prompt the model to either call a tool or produce a final answer, and limit the maximum number of queries to 5 for question-answering datasets, and 10 for mathematical reasoning datasets.

### 4.2 Results and Discussion

**Figure 4.** Impact of Data Filtering on SWiRL's Performance. Synthetic data for training is derived from HotPotQA. SWiRL learns to perform multi-hop question answering even when trained on unfiltered synthetic data. SWiRL's best performance comes from training on process-only filtered data, where the data is selected based on the soundness of each step within its reasoning traces, but which includes both correct and incorrect responses. Note that in all cases, the model is provided with the tool and is allowed to call it multiple times.

![Figure 4 — impact of data filtering on SWiRL performance](../images/05-planning-and-multi-step-reasoning/swirl-figure-4.png)

**Impact of Data Filtering on Model Performance:** We evaluated the influence of various filtering mechanisms on downstream task accuracy, as shown in Figure 4. Concretely, we consider 4 different types of filtering: no filtering, outcome-based filtering that ensures correct final answers, process-based filtering that ensure that each step is correct as judged by a model, and both process and outcome-based filtering.

In all experiments, we fix the number of trajectories used for finetuning (with the exception of our ablation study on the impact of scaling dataset size), and we provided all models with access to an appropriate tool. Notably, process-only filtering consistently yields the highest accuracy, suggesting that focusing on the procedural aspects of data refinement is more important than the correctness of a training trajectory. While both unfiltered and filtered data demonstrated an improvement over the baseline model, filtering for correctness usually harms performance; with the exception of MuSiQue, outcome-filtered or outcome and process-filtered data is less effective than unfiltered data. We hypothesize that this is because SWiRL actually benefits from having access to both positive and negative examples. These results underscore the relative unimportance of outcome-based filtering, which requires golden labels. They also demonstrate that our process RL method can effectively learn from even trajectories with incorrect final answers.

**Generalization Across Disparate Tasks and Tools:** To measure generalization across training tasks, we evaluated the mathematical reasoning capabilities of a model trained on multi-hop question-answering with search tool use (HotPotQA). Specifically, we evaluated the performance of this model on GSM8K, a mathematical reasoning task, providing the model with a SymPy interpreter to use as a calculator. This experiment was run on a different random subsample of 300 examples. As shown in Table 2, applying SWiRL on out-of-distribution data and tasks still improves performance.

**Table 2.** SWiRL Generalization Performance. Finetuning on synthetic traces from HotPotQA or GSM8K improves performance on both in-distribution and out-of-distribution tasks. Interestingly, training on a different domain and tool (e.g. math and a calculator) improves performance on question-answering with a search engine and vice versa, suggesting the effectiveness of SWiRL in improving general multi-step reasoning and tool use capability.

| | GSM8K (math) | HotPotQA (qa) | CofCA (qa) | BeerQA (qa) | MuSiQue (qa) |
|---|---|---|---|---|---|
| Base Model | 0.65 | 0.65 | 0.54 | 0.59 | 0.45 |
| SWiRL on GSM8K (math) | 0.79 | 0.71 | 0.56 | 0.68 | 0.49 |
| SWiRL on HotPotQA (qa) | 0.76 | 0.73 | 0.62 | 0.68 | 0.50 |

*The column labels above appear as two separate header rows in the LaTeX (the dataset name, then its task type); they are combined into one header row per column here.*

**Figure 5.** Comparison of SFT and SWiRL. SWiRL greatly benefits from process-only filtered traces, and unlike SFT, is capable of learning from traces with both correct and incorrect outcomes. Note that in all cases, the models have access to the same tool (retriever or calculator) and are allowed to call it multiple times.

![Figure 5 — SFT versus SWiRL performance comparison](../images/05-planning-and-multi-step-reasoning/swirl-figure-5.png)

**Comparison of Supervised Finetuning and SWiRL:** Figure 5 compares the performance of Supervised Fine-Tuning (SFT) and SWiRL on various downstream tasks. The results show that SFT leads to worse overall performance compared to SWiRL. In fact, we found that SFT actually degrades performance compared to the base model (see Appendix B), which is consistent with prior work showing that SFT can harm reasoning capabilities (Chen et al., 2025). Interestingly, we observe that SFT performs better if we apply it to data that is both process and outcome-filtered, rather only process-filtered, whereas SWiRL learns best from data that is only process-filtered. We attribute this to SFT's tendency to memorize, rather than generalize (Chu et al., 2025; Setlur et al., 2024), which can hinder the model's performance on new, unseen scenarios. In contrast, SWiRL has the ability to improve model performance by targeting per-step reward maximization. SWiRL enables the model to develop a deeper understanding of the necessary steps (e.g. multiple steps of query generation and retrieval), which leads to enhanced planning and generalization. Additionally, in Appendix B, we ran SFT on synthetic trajectories generated by Gemini 1.5 Pro (the reward model in SWiRL), but found that this did not improve performance.

**Effect of Tool Use:** As discussed in Section 2.3, at inference time, we use the proposed multi-step eval as shown in Figure 3 and we iteratively prompt the model to make tool calls as necessary to answer the question. As shown in Figure 6, both base and SWiRL models improve with SWiRL's multi-step tool use inference, but SWiRL-training offers even further improvements. Notably, the SWiRL model exhibits substantial improvements, even without access to a tool, suggesting that SWiRL training improves the model's ability to break down complex problems into multiple manageable subtasks.

**Figure 6.** Performance of SWiRL With and Without Multi-Step Tool Use. SWiRL's multi-step tool use inference improves the performance of both the base model and the SWiRL-finetuned model, but benefits the latter substantially more.

![Figure 6 — SWiRL performance with and without tool use](../images/05-planning-and-multi-step-reasoning/swirl-figure-6.png)

**Impact of Scaling Finetuning Dataset and Model Size:** Our experiments on scaling the fine-tuning dataset size reveal a clear trend: SWiRL has the ability to leverage larger datasets, even when using only process-filtered data, as shown in Figure 7. As the fine-tuning dataset size increases, a consistent enhancement in model performance is observed across our target multi-step reasoning tasks. While a limited dataset of 100 data points appears insufficient for the model to effectively generalize, a significant improvement is evident with 1,000 data points, showing solid gains across all datasets. Furthermore, scaling up to 10,000 data points continues to yield further performance enhancements, confirming the efficacy of our method in capitalizing on larger datasets for improved reasoning capabilities. Interestingly, in GSM8k, we also observe that performance improves as we scale up the number of synthetic trajectories, even though these trajectories represent a different domain (question-answering vs. mathematical reasoning) and a different tool (search vs. calculator).

We also varied model size, observing that smaller models (2b and 9b) may benefit from in-domain SWiRL, but do not display the same generalization as their larger counterpart, Gemma-2-27b. See results in Appendix C.

**Figure 7.** Performance as a Function of Synthetic Dataset Size. Synthetic training data is derived from HotPotQA, and accuracy is evaluated by Gemma-2-27b. As we scale the dataset size, we observe consistent improvements in model performance. With only 1000 data points, the model robustly improves both on in- and out- of distribution datasets. In all cases, multi-step SWiRL inference during evaluation.

![Figure 7 — performance versus synthetic dataset size](../images/05-planning-and-multi-step-reasoning/swirl-figure-7.png)

**Comparison Against the Reward Model**: Here, we compare the performance of Gemma-2-27b before and after being finetuned via SWiRL against the reward model used in SWiRL, Gemini 1.5 Pro. Similar to other experiments in this paper, Gemini-2-27b is used for synthetic data generation and Gemini 1.5 Pro is used as the reward model. The results are shown in Figure 8. As can be seen, SWiRL significantly outperforms the base model on all benchmarks and even outperforms Gemini 1.5 Pro on some out-of-distribution benchmarks, including CofCA and BeerQA. These results suggest that SWiRL is not merely distilling a stronger reward model (Gemini 1.5 Pro).

**Figure 8.** Comparing the performance of SWiRL with the base model and Gemini 1.5 Pro. Finetuning with SWiRL improves performance on all benchmarks, and even enables the model to outperform Gemini 1.5 Pro on some out-of-distribution benchmarks, suggesting that SWiRL is not merely distilling the larger reward model (Gemini 1.5 Pro). Note that, in all cases, each model has access to the same tool (retriever or calculator) and is allowed to call it multiple times.

![Figure 8 — SWiRL versus base model and Gemini 1.5 Pro](../images/05-planning-and-multi-step-reasoning/swirl-figure-8.png)

**Effect on Mean Process Label Accuracy**: In previous subsections, we evaluated the effect of SWiRL on downstream task accuracy. Here, we examine how SWiRL achieves these performance improvements. In Table 3, we show the average process label accuracy for the baseline model vs. a SWiRL finetuned model on 500 trajectories (seeded by 100 questions) for both HotPotQA and GSM8K. To calculate the score per step, we use the same model and prompt as we used for process filtering, as described in Section 4. We take a macro-average of the process label scores within and then across trajectories. We observe that both for in-distribution and out-of-distribution tasks, the SWiRL model generates trajectories with higher average process labels, suggesting that the higher final accuracies are driven by better multi-step reasoning.

**Table 3.** Impact of SWiRL on Process Correctness. After our multi-step RL optimization, we observe that the average correctness of each step improves over the base model on both in- and out- of distribution tasks.

| | HotPotQA (in distribution) | GSM8K (out of distribution) |
|---|---|---|
| Base (Mean Process Label) | 82.5% | 87.5% |
| SWiRL on HotPotQA (Mean Process Label) | 91.0% | 91.6% |

*The column labels above appear as two separate header rows in the LaTeX (the dataset name, then whether it is in- or out-of-distribution); they are combined into one header row per column here.*

## 5 Conclusion

In this work, we propose a synthetic data generation and offline reinforcement learning approach to multi-step reasoning and tool use. This approach outperforms baselines by an average 15% across challenging multi-hop question-answering and mathematical reasoning tasks. We explore the effect of different data filtering strategies in a multi-step, tool use setting, and find that our RL approach is effective even on unfiltered data, but performs best on process-filtered data. Unlike supervised finetuning, our RL approach can learn from trajectories with incorrect final answers and actually benefits from the presence of a mixture of both correct and incorrect final answers. SWiRL demonstrates strong generalization properties, improving performance on mathematical reasoning (GSM8K) by 16.9% when trained on multi-hop question-answering (HotPotQA) and 9.2% vice versa.
