---
title: Shrinking the Generation-Verification Gap with Weak Verifiers
authors: Jon Saad-Falcon, E. Kelly Buchanan, Mayee F. Chen, Tzu-Heng Huang, Brendan McLaughlin, Tanvir Bhathal, Shang Zhu, Ben Athiwaratkun, Frederic Sala, Scott Linderman, Azalia Mirhoseini, Christopher Ré
year: 2025
arxiv: https://arxiv.org/abs/2506.18203
license: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/)
source: arXiv LaTeX source (e-print), fetched 2026-09-13
pdf_pages: 51
course: CS329A lecture 3 (Robust Verification) — site schedule row 3 reading
part: main body
companion: none — the appendices are not transcribed; see the arXiv version
---

# Shrinking the Generation-Verification Gap with Weak Verifiers — main body

Full text of Saad-Falcon et al. (2025), transcribed from the arXiv LaTeX source and reproduced under CC BY 4.0; copyright the authors. Figures are crops of the published PDF, shown with the paper's own caption; this knowledge base adds no description of its own, so what a figure shows is what its caption and the paper's text say. Citations are resolved to author–year; the bibliography is omitted — follow the arXiv link for it. The appendices are not transcribed in this knowledge base; they are in the arXiv version linked above.

## Contents

| § | Heading | Figures | Tables |
|---|---|---|---|
| — | Abstract | | |
| 1 | Introduction | Figure 1 | |
| 2 | Related Work | | |
| 3 | Preliminaries | | |
| 4 | Weaver: A Framework for Weak Verifier Aggregation | | |
| 4.1 | How to Aggregate Multiple Verifiers: Weighted vs Unweighted Ensembles | Figure 2 | |
| 4.2 | Weaver: Weighted Ensembling of Verifier Scores with Minimal Labeled Data | | |
| 5 | Results | | |
| 5.1 | Weaver Shrinks the Gap with Frontier LMs | | Table 1 |
| 5.2 | Weaver Improves Compute-Accuracy Trade-Off for Scaling | Figure 3, Figure 4, Figure 5 | Table 2, Table 3 |
| 6 | Weaver Distillation: Improving Verification Efficiency at Inference | Figure 6 | |
| 7 | Discussion | | |
| 8 | Conclusion | | |
| 9 | Acknowledgements | | |

## Abstract

Verifiers can improve language model (LM) capabilities by scoring and ranking responses from a pool of generated candidates. Currently, high-quality verifiers are either unscalable (e.g., humans) or limited in utility (e.g., tools like Lean for formal proofs). While LM judges and reward models have become broadly useful as general-purpose verifiers, a significant performance gap remains between them and oracle verifiers (i.e. verifiers with perfect accuracy). To help close this gap, we introduce Weaver, a framework for designing a strong verifier by combining multiple weak, imperfect verifiers. First we find that weighted ensembles of verifiers, which typically require learning from labeled data, significantly outperform unweighted combinations due to differences in verifier accuracies. To reduce the dependency on labeled data, Weaver leverages weak supervision to estimate each verifier's accuracy and combines their outputs into a unified score that better reflects true response quality. However, directly applying weak supervision algorithms poses several challenges, including inconsistent verifier output formats and handling low-quality verifiers. Weaver addresses these challenges by using dataset statistics to normalize outputs and filter specific verifiers.

We study the effectiveness of Weaver in test-time repeated sampling settings, where a model generates multiple candidate responses and selects one from among them. Our evaluations demonstrate that Weaver significantly improves over $Pass@1$—the performance when simply selecting the first candidate response—across several reasoning and math tasks, achieving o3-mini-level accuracy with Llama 3.3 70B Instruct (a much cheaper non-reasoning model) as the generator, and an ensemble of 70B or smaller judge and reward models as the verifiers (87.7% average). This gain mirrors the jump achieved between GPT-4o and o3-mini (69.0% vs. 86.7%), which required extensive finetuning and post-training interventions. To reduce the computational costs of running verifier ensembles for Weaver, we train a compact 400M cross-encoder using Weaver's combined output scores. This distilled model retains 98.7% of Weaver's full accuracy while reducing verification compute by up to 99.97%.

## 1 Introduction

A core challenge in deploying language models (LMs) is *verification*: determining the quality or correctness of a model's response. This problem arises across various components of the LM pipeline, including dataset curation, model alignment, and inference-time decision-making. Verification relies on *verifiers*—functions that score responses. When combined with repeated sampling—generating multiple candidate responses from a LM—a perfect verifier can be used to select a correct candidate response, significantly enhancing model capability on tasks such as math, code, and reasoning (Snell et al., 2024; Brown et al., 2024; Puri et al., 2025). For example, Llama 3.1 8B Instruct can match Llama 3.1 70B Instruct and even GPT-4o performances on MATH500 (Hendrycks et al., 2021) and MiniF2F (Zheng et al., 2022) when paired with perfect verifiers for these mathematics tasks. However, without a perfect verifier, a *generation-verification gap* emerges (Song et al., 2025): a LM can generate a correct response, but we fail to identify it.

The generation-verification gap is prevalent across many tasks across mathematics, coding, scientific reasoning, instruction-following, and more. For some of these settings, we have access to *oracle verifiers* that can perfectly identify correct responses. A prominent example is Lean, a formal theorem prover that can be used for problems such MiniF2F (Zheng et al., 2022). However, this is often a limited setup, as not all mathematical proofs can be processed by Lean. Alternatively, humans could judge LM responses but manual evaluation is often expensive, noisy, and difficult to scale (Hosking et al., 2024; Clark et al., 2021; Karpinska et al., 2021). In contrast, LMs prompted as judges (Chiang et al., 2024) and reward models (Lambert et al., 2024; Singhi et al., 2025; Liu et al., 2025) can be applied off-the-shelf to tasks like mathematics, coding, scientific reasoning, instruction-following (Hendrycks et al., 2021; Rein et al., 2024; Jain et al., 2024; Li et al., 2023). However, these *weak verifiers* produce noisy, inconsistent scores, often exhibit poor calibration, and suffer from high false positive rates (Stroebl et al., 2024). We ask: *to what extent can we leverage weak verifiers to improve accuracy in the repeated sampling regime?*

We explore *scaling verification*, specifically how to combine *multiple weak verifiers* to improve response selection for repeated sampling. As new pre-trained models become available, the pool of weak verifiers continues to expand and offer diverse, complementary sources of signal that could improve response selection if they can be aggregated effectively. Recent work has explored scaling verification through techniques such as self-verification or averaging LM judge scores (Lifshitz et al., 2025; Zhao et al., 2025; Chen et al., 2025) although other work has found limitations to scaling test-time compute when utilizing weak verifiers for response selection (Stroebl et al., 2024). We observe three key challenges towards ensembling weak verifiers:

**Figure 1.** **Weaver Framework**: We propose Weaver, a framework combining multiple weak verifiers to effectively scale repeated sampling without parameter finetuning on ground truth labels **(left)**. Weaver significantly outperforms majority voting and shrinks a model's *generation-verification gap* by 14.5%, on average, for GPQA Diamond and other datasets (Table 1) **(middle)**. By distilling Weaver from an ensemble of 70B verifiers to a single 400M cross-encoder, we can preserve 98.2% of the accuracy gains of Weaver while reducing inference compute cost by 99.97% **(right)**.

![Figure 1 — the Weaver framework overview](../images/03-robust-verification/weaver-figure-1.jpg)

1. **Naively aggregating weak verifiers is insufficient for reliable verification.** Weak verifiers such as LM-based judges or reward models produce noisy, biased, and poorly calibrated scores, leading to inconsistent performance (Stroebl et al., 2024; Lambert et al., 2024; Chiang et al., 2024). While using a naive unweighted average of verifier scores is straightforward, it implicitly assumes uniform verifier quality, causing low-quality verifiers to dominate and degrade the overall accuracy (Verga et al., 2024; Xu et al., 2024; Eisenstein et al., 2023). Moreover, while previous work has hypothesized that more sophisticated weighted ensembles should perform better, this claim has not been studied (Lifshitz et al., 2025).

2. **Effective ensembling with limited labeled data is challenging.** More sophisticated ensembling techniques typically learn verifier weights from labeled data, but such data is expensive and difficult to obtain. *Weak Supervision* (WS), a family of statistical techniques developed for data labeling, offers a potential solution through algorithms that aggregate multiple weak signals—such as crowd-worker annotations and expert-defined heuristics—while only requiring a small amount of labeled data (Ratner et al., 2016; Ratner et al., 2019; Fu et al., 2020). In traditional WS, practitioners can design and shape each weak signal to ensure sufficient quality (i.e., iteratively tweaking program-based heuristics), and guarantees of WS hinge on a baseline level of quality. Our weak signals, however, are fixed pre-trained language model verifiers, which have wildly varying accuracy—especially when applied to out-of-distribution tasks—and can emit incompatible outputs (logits, binary scores, Likert scores) (Lambert et al., 2024) that we cannot easily tweak. Due to these conditions, WS algorithms may not perform well when directly applied to verification.

3. **Verification is expensive to deploy at inference.** Verification can dominate inference-time costs (Singhi et al., 2025; Liu et al., 2025), since each verifier must process both the problem and its candidate response(s) (Lightman et al., 2023), often evaluating intermediate steps (Lightman et al., 2023) and multiple solution paths (Snell et al., 2024). In fact, achieving gains over unverified generation (i.e. majority voting) can require $10\times$ to $128\times$ the inference compute per query (Singhi et al., 2025; Lifshitz et al., 2025; Zhao et al., 2025; Chen et al., 2025).

In this work, we introduce Weaver, a framework for aggregating weak verifiers without supervised finetuning on ground truth labels (Figure 1). First, we demonstrate that if we have access to a large corpus of labeled training data (e.g., 50,000 query-response pairs), we can learn weighted ensembles that can outperform naive averaging by up to 11.2% points. This is because weighted ensembles take advantage of wide variability in verifier accuracy. However, in many real-world scenarios, we do not have access to such quantities of labeled data. Second, to reduce the dependency on labeled data, we adapt Weak Supervision to the verification setting by addressing challenges around inconsistent outputs and low-accuracy verifiers. Weaver filters out uninformative verifiers, normalizes verifier scores, and builds a latent variable model over these scores and the unknown true labels to estimate the verifier accuracies to be used as weights for the ensemble (Ratner et al., 2016; Hall, 2003).

Empirically, given a repeated sampling budget and a set of verifiers, Weaver improves over repeated sampling with unweighted averaging of verifier scores by 17.1% and with majority voting by 13.5% (Table 1; Figure 3). Compared to an LM's $Pass@1$, Weaver allows us to improve performance by 17.9% for 8B models and 14.5% for 70B models across reasoning and mathematics tasks (Tables 1 and 3). *This mirrors the performance jump from GPT-4o to o3-mini* (73.9% vs. 88.2%)—but only via increased sampling at test time rather than parameter tuning or post-training procedures. We also study how Weaver scales along different axes of test-time compute: generation, verifiers, model size, and inference budget (Section 5.2). We find that even as we increase the number of generations, many standard verification baselines (e.g. majority voting) quickly plateau (Figure 3). Naive ensembling saturates more slowly, but its gains are limited by sensitivity to the model choice and the number of verifiers.

Finally, to mitigate the compute costs of calling multiple weak verifiers for each response, we extend Weaver by training a 400M-parameter cross-encoder verifier using Weaver's selected responses. We demonstrate that using a distilled Weaver cross-encoder as a verifier *retains 98.7% of the accuracy gains* from the learned verifier ensemble while reducing compute costs by three orders of magnitude -- *saving 99.97% inference FLOPS* while still capturing an effective verification strategy (Section 6). Overall, our findings highlight that more reliable, scalable verification is possible even in the absence of ground-truth labels—paving the way for improved data filtering, model alignment, and inference-time decision-making.

## 2 Related Work

**LM Judges and Reward Models**: Both LM judges and reward models are promising approaches for evaluating language model outputs, but their high false positive rates limit their reliability (Stroebl et al., 2024). LM judges can evaluate outputs without additional training (Liu et al., 2023; Wang et al., 2023; Fu et al., 2023), using approaches from simple prompting to chain-of-thought reasoning (Liu et al., 2023) to specialized fine-tuning (Saad-Falcon et al., 2023; Tang et al., 2024) to multi-LM inference architectures (Saad-Falcon et al., 2024; Kalra and Tang, 2025). However, they face poor generalization across contexts (Es et al., 2023; Saad-Falcon et al., 2023; Ravi et al., 2024) and systematic biases in position and self-preference (Chen et al., 2024; Pan et al., 2024; Zheng et al., 2023). Similarly, while reward models have become central to model alignment (Bradley and Terry, 1952; Christiano et al., 2017; Liu and Zeng, 2024), they struggle with noisy training signals from low inter-annotator agreement (Askell et al., 2021; Ouyang et al., 2022; Wang et al., 2024; Dubois et al., 2024) and learned biases favoring attributes like response length (Lambert and Calandra, 2023; Singhal et al., 2023; Dubois et al., 2024). Recent work has improved individual verifier reliability through better data collection, chain-of-thought reasoning, and natural language unit tests (Wang et al., 2023; Zhang et al., 2024; Saad-Falcon et al., 2024), yet fundamental challenges persist (Eisenstein et al., 2023; Chaudhari et al., 2024). Weaver advances beyond these approaches by combining multiple verification signals with adaptive weighting, thus leveraging the complementary strengths of weak verifiers while suppressing noise and reducing false positives.

**Weak Supervision**: Weaver builds upon statistical techniques from weak supervision, which emerged as a framework for programmatically generating training labels by aggregating multiple weak sources (Ratner et al., 2016; Ratner et al., 2020). While a majority of the work focuses on classification tasks (Ratner et al., 2019; Fu et al., 2020; Chen et al., 2022), recent advances have expanded to handle multi-task settings (Shin et al., 2021) and structured prediction (Vishwakarma and Sala, 2022). Weak Supervision has also been applied to LM prompting (Arora et al., 2022) and routing (Guha et al., 2024). Weaver applies Weak Supervision to answer verification, treating binary imperfect verification signals (e.g. reward models and LM judges) as weak supervision voters that classify candidate solutions as correct or incorrect. This novel application combines predictions by converting these diverse signals into binary verdicts, enabling Weaver to learn better verification strategies from weak but complementary verifiers.

**Verification as another compute axis and aggregation:** Recent work has explored verification as a new scaling axis (Lifshitz et al., 2025; Liu et al., 2025; Zhao et al., 2025; Singhi et al., 2025; Stroebl et al., 2024; Chen et al., 2025). However this work limits their analysis to one verifier, and instead scale how many times to verify (Zhao et al., 2025). Approaches that do leverage multiple verifiers often rely on substantial amounts of labeled data for aggregation or creating specialized verifiers (Kirchner et al., 2024; Lifshitz et al., 2025). With Weaver, we show that it is possible to combine verifiers without ground truth labels, even when they are not specialized. Other work has focused on combining multiple verifiers for post-training the base model using RLHF (Wang et al., 2024; Eisenstein et al., 2023; Wang et al., 2025).

## 3 Preliminaries

First, we define the problem of how to select among repeated samples. We then define verifiers and key evaluation metrics, including the generation-verification gap.

**Problem Definition**  Let $q \in \mathcal{Q}$ be a input text query, and let $r \in \mathcal{R} \sim \mathcal{M}(q)$ be a corresponding response sampled from language model $\mathcal{M}$ with non-zero temperature. For a given query-response pair $(q, r)$, we define $y: \mathcal{Q} \times \mathcal{R} \rightarrow \lbrace 0, 1\rbrace$ such that $y(q, r)$ is the correctness label of $r$ for $q$.

We are given an unlabeled test dataset $\mathcal{D}^{\text{test}} = \lbrace (q_i, \mathbf{r_i})\rbrace _ {i=1}^n$, where $\mathbf{r_i} = \lbrace r_{ij}\rbrace _ {j=1}^K$ consists of $K$ repeatedly sampled responses from $\mathcal{M}$ for each $q_i$. We also assume access to a small labeled development dataset $\mathcal{D}^{\text{dev}} \subset \mathcal{D}^{\text{test}}$, comprising 1% of the test set (e.g. 5 to 10 query-answer pairs), which is used to estimate global statistics such as the task difficulty probability, $\Pr(y_{ij} = 1)$. We do not have access to true labels $y_{ij} := y(q_i, r_{ij})$ for any $i, j$ in $\mathcal{D}^{\text{test}} \setminus \mathcal{D}^{\text{dev}}$.

For each $(q_i, \mathbf{r}_ i) \in \mathcal{D}^{\text{test}}$, our goal is to select a correct response $j^\star \in [K]$ that satisfies $y_{ij^\star} = 1$. We can broadly describe this selection rule using a scoring function $f: \mathcal{Q} \times \mathcal{R} \rightarrow \mathbb{R}$, namely $j^\star := \arg\max_j f^\star(q_i, r_{ij})$.

**Using verifiers**  A verifier, either a reward model or an LM prompted as a judge, can be expressed as a scoring function on query-response pairs $v: \mathcal{Q} \times \mathcal{R} \rightarrow \mathbb{R}$. For reward models, the verifier score is continuous, while for LM judges, the verifier score is typically discrete (for our setup, we use $[0, 1]$ and $\lbrace 0, 1\rbrace$, respectively). We assume that we have access to multiple verifiers $\mathcal{V} = \lbrace  v_1, \dots, v_m\rbrace$. We apply each of the $m$ verifiers to each $(q_i, r_{ij})$, for a total of $nmK$ scores on $\mathcal{D}^{\text{test}}$, with $s_{ijk} := v_k(q_i, r_{ij})$. We aim to use $\mathcal{V}$ to construct a verification strategy $f$.

**Evaluation metrics**  The $Pass@1$ metric is the probability that an LM's first response is correct. $Pass@K$ generalizes this metric and is defined as the probability that there exists a correct response among $K$ generated responses:

$$Pass@K = \frac{1}{n} \sum_{i = 1}^n \mathbf{1}(\exists j \in [K]: y_{ij} = 1).$$

This metric is independent of the verification strategy, and depends on the choice of $\mathcal{M}$, $K$, and the task dataset. The success rate of a verification strategy $\hat{f}$ is $\frac{1}{n} \sum_{i = 1}^n y_{i\hat{j}}$, where $\hat{j} = \arg\max_{j \in [k]} \hat{f}(q_i, r_{ij})$. Success rate is dependent on the verification strategy and bounded by Pass@K, and equality is obtained with oracle verification (i.e., $\hat{f} = f^\star$ can always select a correct $j$ as long as it exists).

We define the *generation-verification gap* as Pass@K − Success Rate. A large positive gap indicates that although correct answers are generated, the verification strategy fails to select them consistently. We aim to close this gap and will use it to evaluate verification strategies.

## 4 Weaver: A Framework for Weak Verifier Aggregation

In Section 4.1, we demonstrate that naively averaging multiple verifier scores to select responses significantly underperforms weighted ensembles; however, common methods for computing weights require labeled data (Schapire, 2013; Ying et al., 2015). We introduce Weaver (Section 4.2), a method for weighted aggregation of verifier scores with minimal data that draws inspiration from Weak Supervision. Unlike prior work, Weaver adapts weak supervision to verification by addressing challenges unique to verifier aggregation, such as inconsistent score formats and the presence of low-quality or adversarial verifiers. To our knowledge, this is the first framework to successfully apply weak supervision to ensemble verifier scores for response selection.

### 4.1 How to Aggregate Multiple Verifiers: Weighted vs Unweighted Ensembles

A straightforward approach for using multiple verifiers is a naive ensemble—selecting the response with the highest average verifier score: $f(q_i, r_{ij}) = \frac{1}{m} \sum_{k=1}^m s_{ijk}$. This approach (Lifshitz et al., 2025) does not consider the relative accuracy of verifiers. However, we observed that there is significant variation in the success rates of individual verifiers—spanning a range of up to 37.5%—suggesting that naive ensembles could be suboptimal (Table 16, in the appendix).

An alternative is to use a weighted ensemble. One approach is to use a labeled dataset to identify and use the top-performing verifier, effectively assigning a weight of $0$ to discarded verifiers. Other strategies include using Logistic Regression or a Naive Bayes classifier, where the scoring function $f(q_i, r_{ij})$ is the probability $\Pr(y_{ij} = 1 | s_{ij1}, \dots, s_{ijm})$. These classifiers are fit using labeled data and can be either modeled as a logistic function or factorized using Bayes' rule and independence assumptions, respectively.

In Figure 2, we compare a naive ensemble with weighted ensembles for several tasks, using Llama 3.3 70B Instruct to generate responses and using a collection of 33 7B-72B reward models and LM judges as verifiers (Appendix C.1). We see that using a weighted ensemble can achieve up to 11.2 points higher success rate than the naive ensemble. However, all weighted ensembles shown are "oracle" methods: they are computed using $y_{ij}$ for all $i\in [n], j \in [K]$, although in practice these labels are unknown for $\mathcal{D}^{\text{test}}$. In fact, when we instead use $0.01 n$ labeled samples, accuracy drops by 20.1% on average (Table 17, in the appendix). This raises the question of how to best construct weighted ensembles with limited labeled data.

**Figure 2.** **Weighted Verifier Ensembles Outperform Naive Verifier Ensembles**: By using oracle data to keep the best verifiers (i.e. $\text{top-}K$ *verifier ensembles*) or learn aggregation weights for verifiers (i.e. *supervised weighted ensembles*), we can improve beyond naive combinations of the verifiers available by 3.6% and 7.8%, on average, respectively.

![Figure 2 — naive vs. oracle-weighted verifier ensembles](../images/03-robust-verification/weaver-figure-2.jpg)

### 4.2 Weaver: Weighted Ensembling of Verifier Scores with Minimal Labeled Data

We first describe the WS method we use in Weaver to construct a weighted ensemble over binary verifier scores. Because verifiers often produce scores in inconsistent formats and exhibit low accuracies—challenges not typically encountered in traditional WS—we introduce a binarization and verifier discarding strategy in Appendices B.2 and B.3 to discard low-quality verifiers and ensure that only sufficiently reliable binary scores are used as input to the WS method.

#### 4.2.1 Weak Supervision Algorithm

In Weak Supervision, the input is an unlabeled dataset, where each entry has multiple binary "votes" on the true label. Applied to our setting, each entry is a query-response pair, forming a dataset of size $nK$, and verifier scores $s_{ijk}$ are binarized into votes $\bar{s}_ {ijk} \in \lbrace 0, 1\rbrace$ for all $i, j, k$. Our goal is to predict the probability that a response is correct, $\Pr(y_{ij} = 1 | s_{ij1}, \dots, s_{ijm})$ for all $i, j$.

**WS model**  We can view all $y_{ij}$ across query-response pairs as samples of an unknown random variable $Y$ and each $\bar{s}_ {ijk}$ across $i, j$ as samples of a random variable $S_k$. WS then defines a latent variable graphical model over the random binary vector $\lbrace Y, S_1, \dots, S_m\rbrace$, where $Y$ is latent while $S_1, \dots S_m$ are observable. While existing WS methods assume various models, one common assumption is that $S_i \perp S_j | Y$ for each $S_i, S_j$. That is, $S_i$ and $S_j$ are conditionally independent given $Y$; intuitively, each verifier is assumed to capture independent aspects of the correctness of the response (Figure 22 in Appendix C.4). Under this assumption, we can write the posterior probability of a correct generation as the following, for some given binary verifier scores $\lbrace \bar{s}_ 1, \dots, \bar{s}_ m\rbrace$:

$$\Pr(Y = 1 | S_1 = \bar{s}_ 1, \dots, S_m = \bar{s}_ m) = \frac{\prod_{i = 1}^m \Pr(S_i = \bar{s}_ i | Y = 1) \Pr(Y = 1)}{\Pr(S_1 = \bar{s}_ 1, \dots, S_m = \bar{s}_ m)}. \tag{1}$$

The weighted ensemble score for each query-response pair can thus be written in terms of: 1) $\Pr(S_1 = \bar{s}_ 1, \dots, S_m = \bar{s}_ m)$, which is intractable to compute from the data for large $m$; 2) $\Pr(Y = 1)$, which can be estimated from $\mathcal{D}^{\text{dev}}$; and 3) $\Pr(S_i = \bar{s}_ i | Y = 1)$, or equivalently $\Pr(S_i = 1 | Y = 1)$, which is the verifier's "accuracy parameter"—this cannot be computed directly since we do not have access to $Y$. Next, we discuss how to estimate these accuracy parameters, $\Pr(S_i = 1 | Y = 1)$, without labels.

**WS parameter estimation**  We outline a parameter estimation technique first introduced in Ratner et al. (2020). Due to the assumption that $S_i \perp S_j | Y$, the following equation holds:

$$\begin{aligned} \Pr(S_i, S_j) &= \Pr(S_i, S_j | Y = 1) \Pr(Y = 1) + \Pr(S_i, S_j| Y = 0) \Pr(Y = 0) \cr  &= \Pr(S_i | Y = 1) \Pr(S_j| Y = 1) \Pr(Y = 1) + \Pr(S_i | Y = 0) \Pr(S_j | Y = 0) \Pr(Y = 0). \end{aligned} \tag{2}$$

Note that $\Pr(S_i, S_j)$ can be computed from the known verifier scores, and $\Pr(Y=1)$ is estimated from $\mathcal{D}^{\text{dev}}$. Then, Equation (2) is a quadratic equation over the accuracy parameters. We can write this equation for every pair $S_i, S_j$, and for every pair of values $\lbrace 0, 1\rbrace ^2$ they can take. Furthermore, we can write another type of equation over the accuracy parameters:

$$\Pr(S_i = 1) = \Pr(S_i = 1 | Y = 1)\Pr(Y = 1) + \Pr(S_i = 1 | Y = 0) \Pr(Y = 0). \tag{3}$$

This is a consistency property that holds regardless of the conditional independence assumption, and we can write this equation for each of the $m$ $S_i$'s. Because we know that the accuracy parameters should follow Equations (2) and (3), we can construct an objective function that aims to minimize the difference between the left and right hand sides of these equations. We write this efficiently in matrix notation. Let $P \in \mathbb{R}^{2\times 2}$ be a diagonal matrix with diagonal $[\Pr(Y=0) \mskip{5mu} \Pr(Y=1)]$. Define $\mu \in \mathbb{R}^{m \times 2}$ to be the matrix of accuracy parameters, and define $O \in \mathbb{R}^{2m \times 2m}$ to be a matrix over the joint probabilities of pairs of $S_i, S_j$; more formally:

$$\begin{aligned} \mu_{2i-1:2i, 1:2} &= \begin{bmatrix} \Pr(S_i = 0 | Y = 0) & \Pr(S_i = 0 | Y = 1) \cr  \Pr(S_i = 1 | Y = 0) & \Pr(S_i = 1 | Y = 1) \end{bmatrix}, \mskip{5mu}\mskip{5mu} O_{2i-1:2i, 2i-1:2i} = \begin{bmatrix} \Pr(S_i = 0) & 0 \cr  0 & \Pr(S_i = 1) \end{bmatrix} \mskip{5mu} \forall i \in [m] \cr  O_{2i-1:2i, 2j-1: 2j} &= \begin{bmatrix} \Pr(S_i = 0, S_j = 0) & \Pr(S_i = 0, S_j = 1) \cr  \Pr(S_i = 1, S_j = 0) & \Pr(S_i = 1, S_j = 1) \end{bmatrix} \mskip{5mu} \forall i \neq j \in [m] \end{aligned} \tag{4}$$

Let $\text{off-diag}$ denote the elements of a matrix that lie outside its $2\times 2$ block diagonal. Then, to estimate $\mu$ that satisfies both Equations (2) and (3), we have the following objective:

$$\text{minimize}_ {\mu} \bigl\Vert \thinspace O_{\text{off-diag}} - (\mu\thinspace P\thinspace \mu^T)_ {\text{off-diag}}\bigr\Vert ^2 + \bigl\Vert \thinspace \mathrm{diag}(O) - \mu\thinspace P\thinspace \mathbf{1}^T\bigr\Vert ^2 \tag{5}$$

We optimize Equation (5) using gradient descent to estimate the verifier accuracy parameters. These estimates are then used in Equation (1) to select the response with the highest estimated posterior. To further improve modeling of verifier accuracies, we explore whether partitioning the query distribution by empirical difficulty can yield better weak supervision estimates. As detailed in Appendix B.4, we cluster queries based on the observed ratio of correct to incorrect generations, and fit a separate Weaver model within each difficulty bucket. We provide more details in Section B.1.

## 5 Results

In Section 5.1, we provide empirical results on Weaver's performance compared to other approaches for selecting responses in repeated sampling. In Section 5.2, we study how Weaver's performance scales along several axes: the number of responses, model size, verifier counts, and inference compute.

**Datasets, Verifiers, and Baselines**  Our reward models range in size from 8B to 72B, are all open-source, and are obtained from RewardBench (Lambert et al., 2024), a popular evaluation tool for reward models. We prompt open-source language models from Chatbot Arena (Chiang et al., 2024) to serve as judges. Unless specified, we use Llama 3.3 70B Instruct to generate responses and use all 33 reward models and judges. We evaluate on MATH500, GPQA Diamond, MMLU College, and MMLU Pro. See Appendix C.1 for more details.

We compare Weaver against verifier-free baselines as well as standard verification strategies. First Sample, also known as Pass@1, only uses the first response and does not scale test-time compute or verification. Majority Voting involves repeated sampling but not verification, picking the most common final answer from the responses (Brown et al., 2024; Snell et al., 2024; Chen et al., 2024). We compare against the highest scoring reward model and a naive ensemble of the top-10 reward models on RewardBench. We also evaluate two recently proposed methods that scale verification but do not use different verifier models or weighted ensembles: Self-Verification (Zhao et al., 2025) and Multi-Agent Verification (Lifshitz et al., 2025). Lastly, we report the oracle Pass@K rate, which establishes an upper bound for the success rate of these verification strategies.

### 5.1 Weaver Shrinks the Gap with Frontier LMs

In Table 1, we evaluate Weaver along with baseline verification methods, the first sample performance of frontier LMs, and the Pass@100 metric. We use LlaMA 3.3 70B Instruct to generate $K=100$ responses per query. We find that Weaver's weighted ensembling of multiple verifiers allows us to outperform majority vote by 15.5% and come within 4.2% of the Pass@100 oracle metric. Furthermore, Weaver rivals the performance of frontier reasoning models—coming within 0.5% of OpenAI's o3-mini (OpenAI, 2025)—even though we use a non-reasoning model for generation.

**Table 1.** **Weaver Outperforms Baseline Verification Methods and Shrinks Gap with Frontier LMs.**

| Methodology | Generations (K) | MATH500 | GPQA Diamond | MMLU College | MMLU Pro | Average |
|---|---|---|---|---|---|---|
| *Baselines* | | | | | | |
| First Sample | 1 | 78.0% | 42.9% | 82.6% | 69.9% | 68.4% |
| Majority Voting | 100 | 83.0% | 47.4% | 84.1% | 74.4% | **72.2%** |
| Highest Scoring RM on RewardBench (Yang et al., 2024; Lambert et al., 2024) | 100 | 78.2% | 49.7% | 86.0% | 77.0% | **72.7%** |
| Naive Ensemble of Top-10 RMs on RewardBench (Lambert et al., 2024) | 100 | 75.4% | 41.3% | 88.1% | 71.4% | 69.1% |
| Self-Verification (Zhao et al., 2025) | 100 | 78.1% | 43.1% | 82.0% | 69.5% | 66.9% |
| Multi-Agent Verification (Lifshitz et al., 2025) | 100 | 81.3% | 47.8% | 84.1% | 72.6% | 71.6% |
| Weaver | 100 | 93.4% | 72.1% | 94.9% | 90.2% | **87.7%** |
| *Frontier Approaches* | | | | | | |
| GPT-4o (OpenAI, 2023) | 1 | 77.4% | 35.9% | 87.1% | 75.4% | 69.0% |
| Claude 3.7 Sonnet (Anthropic, 2025) | 1 | 69.2% | 48.0% | 86.1% | 78.1% | 70.4% |
| Llama 4 Maverick (Meta, 2025) | 1 | 87.6% | 68.9% | 91.1% | 81.0% | 82.2% |
| o3-mini (OpenAI, 2025) | 1 | 94.4% | 74.0% | 92.2% | 86.0% | **86.7%** |
| Oracle Verification (Pass@100) | 100 | 98.6% | 81.0% | 96.0% | 92.0% | **91.9%** |

*The italicized "Baselines" and "Frontier Approaches" rows stand in for a vertical, rotated `\multirow` label spanning the corresponding rows in the LaTeX; the Weaver row sits between the two groups, outside either span. Cells bolded above are `\textbf{}` or `\underline{}` in the source (both are rendered bold here).*

### 5.2 Weaver Improves Compute-Accuracy Trade-Off for Scaling

By proposing to combine multiple weak verifiers instead of one, we introduce yet another axis for test-time scaling. In this section, we study how well scaling verification with Weaver interacts with common previously studied axes for verification, summarized in Table 2.

**Table 2.** **Scaling Dimensions for Generation and Verification Models**

| Scaling Dimension | Base Model | Verifier Type | Visuals |
|---|---|---|---|
| **Sample Count**: More Generations | Temperature-based sampling | Majority Vote, Weak Verifier, Top-K, Weaver | Figure 3 |
| **Model Size**: Larger Models | Llama 8B → 70B | RM-8B → RM-70B | Table 3 |
| **Verifier Count:** More Models | Llama 8B/70B | RMs and LM Judges | Figure 4 |
| **Inference Compute:** More FLOPs for Gen./Ver. | Temp-based sampling | Weak Verifiers + Weaver | Figure 5 |

**(1) Scaling Candidate Generations**: we study the performance of verification methods as we increase the number of repeated samples in Figure 3. Based on prior work (Bradley and Terry, 1952; Chen et al., 2021), as the number of responses increases, we are more likely to see a correct response (i.e. Pass@K increases), and hence more likely to select a correct response given a good verification strategy. However, differences in verification translate into different scaling rates. We evaluate the performance of Weaver and baselines for $K = 2^0$ to $2^{10}$, comparing to o3-mini and Pass@K as well. Across all tasks, Weaver yields the most substantial gains when scaling the number of generations. Weaver consistently narrows the generation-verification gap with the oracle upper bound (Pass@K) while alternative verification strategies plateau after a few generations. The effect is particularly pronounced on difficult tasks like GPQA. We detail the scaling trends observed in Figure 3 in Appendix C.3.

**Figure 3.** **Scaling Generations Boosts Performance with Weaver**: The generation-verification gap shrinks when increasing $K$ and leveraging Weaver, outperforming alternative verification methods by an average 18.3%.

![Figure 3 — success rate vs. number of generations](../images/03-robust-verification/weaver-figure-3.jpg)

**(2) Scaling Model Sizes:**  In Table 3, we study how Weaver applied on smaller models (both verifiers and for generating responses) can allow us to match the performance of larger models, enabling weak-to-strong verification. We consider an 8B setting—using LlaMA 3.1 8B to generate responses along with 8B verifiers—and compare this to a 70B setting (LlaMA 3.3 70B Instruct, 8B-72B verifiers) as well as o3-mini. We see that Weaver applied at the 8B scale comes within 1.6% of the majority vote baseline at the 70B scale, and Weaver at 70B surpasses o3-mini by 1.0%, demonstrating a weak-to-strong verification phenomenon. Verifier calibration details are available in Appendix C.5.

**Table 3.** **Weaver Reduces Gap between Model Classes: 8B and 70B, 70B and Frontier LM**

| Generator Model | Verifier Model | Aggregation Strategy | MATH | GPQA Diamond | MMLU College | MMLU Pro | Average |
|---|---|---|---|---|---|---|---|
| Llama 3.1 8B Instruct | N/A | Majority Vote | 69.0% | 30.5% | 72.7% | 56.4% | 57.2% |
| Llama 3.1 8B Instruct | 8B and below | Weaver | 80.0% | 47.1% | 85.7% | 67.2% | **70.0%** |
| **Δ w. Weaver** | | | +11.0% | +16.6% | +13.0% | +10.2% | +12.8% |
| Llama 3.3 70B Instruct | N/A | Majority Vote | 83.0% | 44.9% | 84.1% | 74.4% | 71.6% |
| Llama 3.3 70B Instruct | 72B and below | Weaver | 93.4% | 72.2% | 94.9% | 90.2% | **87.6%** |
| **Δ w. Weaver** | | | +10.4% | +27.3% | +10.8% | +15.8% | +16.0% |
| o3-mini | N/A | First Sample | 94.4% | 74.0% | 92.2% | 86.0% | **86.7%** |

*Each "Δ w. Weaver" row flattens a `\multicolumn{3}{r}` cell that spans the Generator Model, Verifier Model, and Aggregation Strategy columns in the LaTeX; the label is shown here in the first of those columns, with the other two left blank.*

**(3) Scaling Verifier Count:** Two axes for scaling verification are **(1)** the number of verifiers used and **(2)** the number of scores sampled from each verifier. Figure 4 shows how performance changes as we ensemble 1 to 15 verifiers using both naive averaging and Weaver. Verifiers are greedily added in order of individual accuracy, from highest to lowest. Aggregating more verifiers improves performance by up to 8.5% over the top-1 verifier. As shown in Figure 4, Weaver consistently outperforms naive ensemble averaging across both *Oracle Top-5 Verifiers* and *Total Verifiers* configurations for verifier ensembling, with improvements ranging from +2.4% to +10.1% across all datasets. The performance gains are particularly pronounced on GPQA Diamond (+10.1%) and MMLU Pro (+5.1%), demonstrating Weaver's effectiveness in aggregating verifier signals through learned weights rather than simple averaging. However, gains diminish as more models are added—reflecting the classic ensemble bias-variance tradeoff: initial improvements stem from variance reduction, while additional verifiers contribute redundant signal due to correlated biases on hard examples (Abe et al., 2024). We compare alternative score calibration strategies beyond Weaver's binary transformation in Appendix C.7, and find that the default binarization yields the strongest downstream selection performance. We also explore scaling the number of scores per verifier—via prompt tuning or temperature variation—in Appendix C.5. While this yields modest improvements, increasing verifier count remains the more effective strategy. That said, both methods are complementary and can be combined for further gains.

**Figure 4.** **Weaver Outperforms Naive Ensemble across Oracle Top-5 Verifiers and Total Verifiers Configurations:** Results are shown for Weaver ensembles and naive ensembles of the *Oracle Top-5 Verifiers* (highest-performing verifiers on dataset selected using ground truth) and *Total Verifiers* (all available verifiers). Weaver consistently outperforms naive ensemble averaging, with improvements ranging from +2.4% to +10.1%.

![Figure 4 — Weaver vs. naive ensemble as verifier count scales](../images/03-robust-verification/weaver-figure-4.png)

**(4) Scaling Test-Time Compute:** We study how performance scales in the total compute used for both verification and repeated generations. Figure 5 shows the relationship between inference-time compute and success rate for different generation-verification systems. For each method, we scale the number of generations exponentially from 1 to 100 and plot the required inference compute for generation and verification together versus the success rate. Note that Figure 5 differs from Figure 3, since Majority Voting requires $0$ verification inference calls while Weaver requires 30+ calls for the weak verifiers. We find that Weaver achieves the highest maximum success rate; notably, majority voting plateaus at around $2^2$ to $2^3$ ExaFLOPs per query while Weaver continues scaling until 512 ExaFLOPs. However, the additional compute required for Weaver can be prohibitive. We explore how to reduce this computational burden while retaining Weaver's performance in the next section.

**Figure 5.** **Weaver Improves the Accuracy-Compute Performance Trade-Offs.** Success rate (%) as a function of total inference compute per query (generation and verification compute, log scaled) for different verification strategies. Each point represents a different number of candidate generations (from $2^0$ to $2^{7}$). Weaver achieves the highest accuracy while requiring more compute than Majority Voting but demonstrates continued scaling benefits, while Weaver Distilled maintains most of Weaver's performance gains with 97.3% compute savings and substantial accuracy improvements over baseline methods.

![Figure 5 — accuracy vs. inference compute trade-off](../images/03-robust-verification/weaver-figure-5.jpg)

## 6 Weaver Distillation: Improving Verification Efficiency at Inference

We explore distillation strategies for fine-tuning a smaller LM as a task-specific verifier. In particular, we train *cross-encoders*; the input is a concatenated query-response pair, while the output is Weaver's pseudolabel generated from Weak Supervision, namely $\Pr(y_{ij} = 1 | s_{ij1}, \dots, s_{ijm})$ (see Section 4.2). For the model, we selected ModernBERT-Large (396M) (Warner et al., 2024). For more details, please see Appendix C.6.

**Figure 6.** **Distilling Weaver into a 400M Cross-Encoder Almost Entirely Captures the Performance of Weaver, Yielding 99.97% Compute Savings.** $^{\ast}$ We train/evaluate on an 80:20 split.

![Figure 6 — Weaver vs. distilled cross-encoder Pareto frontier](../images/03-robust-verification/weaver-figure-6.jpg)

Figure 6 shows the performance of Weaver on the Llama-70B generations against the cross-encoder on GPQA Diamond. Across tasks, we find that the distilled cross-encoder is able to capture 98.2% of the performance of Weaver. When running Weaver with all the verifiers, it costs 35.35 exaFLOPs for each query's set of 100 samples. Running a 400M cross-encoder costs 1.01 exaFLOPs for evaluating 100 samples and *reduces compute cost by more than three orders of magnitude, saving 99.97% of the FLOPs originally required for running the 70B verifiers*. We also outperform majority voting by 23.2% while only incurring a 0.57% increased inference cost over only generating the responses. We see similar results for additional datasets in Figure 22 (Appendix C.6).

These results suggest that, through distillation, we can capture the combined strengths of the weak verifiers used for Weaver, and deploy generalizable and lightweight cross-encoders that use only a fraction of the parameters used for generation. This reduces our hardware constraints considerably; *rather than utilizing an 8-GPU node per 70B verifier (i.e. Nvidia H200s with 80B memory), we only require a single A100 GPU with 32GB of memory for our cross-encoder*.

## 7 Discussion

Several research directions remain to be explored with Weaver:

1. **Specialized Verifier Development**: Our work highlights the varying effectiveness of different weak verifier categories across task domains. Future research should investigate specialized verifier architectures tailored to specific tasks, such as enhanced mathematical reasoning capabilities for numerical problems (Yang et al., 2024) or improved code execution simulation for programming tasks (Jain et al., 2024; Quan et al., 2025).

2. **Dataset Distribution**: For particularly difficult datasets, such as AIMO 2024, Weaver has trouble selecting a correct answer since there are so few correct responses compared to the other datasets (Table 13; Table 14). By scaling the number of generated responses, we are able to improve performance by increasing the absolute number of correct responses (Figure 3) but improved generation techniques, verifier scoring, and aggregation techniques can help us better close the gap with $Pass@K$ for these harder tasks.

3. **Weaver for RLHF**: With Weaver generated predictions, we can improve the quality of labels used for RLHF on reasoning and mathematics, improving beyond an individual verifier. Previous work has explored reward model ensemble approaches towards RLHF, minimizing poor performing RMs while maximizing the complementary strengths of accurate RMs (Wang et al., 2024; Eisenstein et al., 2023). Fine-tuning generation models can further improve the accuracy-compute trade-off of Weaver beyond solely distilling a lightweight verifier (Section 6), improving the positive/negative generation ratio and thus making verification an easier task for Weaver models.

4. **Multi-Modal Verification**: Extending Weaver to multimodal tasks involving images, audio, or video would broaden its applicability but introduces new challenges in verification across modalities (Phan et al., 2025; Wang et al., 2020). Research is needed on how verification signals can be effectively combined across different data types.

## 8 Conclusion

In this paper, we present Weaver, a framework that addresses the fundamental challenge of scaling test-time compute through effective verification strategies. Our contributions advance the state of knowledge in three key dimensions. First, we establish that weighted aggregation of weak verifiers substantially outperforms both individual verifiers and majority voting across reasoning and mathematics tasks, with weighted aggregation exceeding majority voting by an average of 12.3% across all tasks explored (Table 1; Figure 2). Second, we developed a principled approach for unsupervised estimation of verifier accuracies using weak supervision, enabling effective ensemble weighting without fine-tuning on costly ground-truth annotations. This allows us to close the generation-verification gap by 12.8% for 8B models and 16.0% for 70B models (Table 3). By leveraging Weaver with 70B models, we marginally outperform frontier closed-source models such as OpenAI's o3-mini (87.7% vs. 86.7%) on average across the tasks explored (Table 1). Third, we improve the accuracy-compute trade-off by distilling Weaver into lightweight 400M-parameter cross-encoders. These distilled models retain 98.2% of Weaver's performance while reducing inference compute by 99.97% (Section 6). This enables high-throughput, cost-efficient verification without sacrificing accuracy, and demonstrates that scalable verification can be achieved without repeatedly querying large models. These findings suggest that strategically combining and distilling weak verifiers enables scalable, label-efficient, and compute-efficient verification—paving the way for better data filtering, model alignment, and inference-time decision-making without additional training of the base generator model.

## 9 Acknowledgements

We thank the members of the Hazy Lab, Linderman Lab and Scaling Intelligence Lab for their constructive feedback during the composition of the paper. In particular, we would like to thank Daniel Biderman, Bradley Brown, Ryan Ehrlich, Sabri Eyuboglu, Anna Goldie, Neel Guha, Simon Guo, Jordan Juravsky, Hermann Kumbong, Jerry Liu, Avanika Narayan, Anne Ouyang, Benjamin Spector, Shayan Talaei, Benjamin Viggiano, and Michael Zhang. We also thank Marlowe and Together AI for providing compute resources that enabled our experiments.

We gratefully acknowledge the support of NIH under No. U54EB020405 (Mobilize); NSF under Nos. CCF2247015 (Hardware-Aware), CCF1763315 (Beyond Sparsity), CCF1563078 (Volume to Velocity), and 1937301 (RTML); US DEVCOM ARL under Nos. W911NF-23-2-0184 (Long-context) and W911NF-21-2-0251 (Interactive Human-AI Teaming); ONR under No. N000142312633 (Deep Signal Processing); Stanford HAI under No. 247183; Google DeepMind; Google Research; Google Cloud; NXP; Xilinx; LETI-CEA; Intel; IBM; Microsoft; NEC; Toshiba; TSMC; ARM; Hitachi; BASF; Accenture; Ericsson; Qualcomm; Analog Devices; Salesforce; Total; the HAI-GCP Cloud Credits for Research program; the Stanford Data Science Initiative (SDSI); members of the Stanford DAWN project: Meta, Google, and VMWare; and members of the Stanford SEAMS project: IBM and Felicis.

The U.S. Government is authorized to reproduce and distribute reprints for Governmental purposes notwithstanding any copyright notation thereon. Any opinions, findings, and conclusions or recommendations expressed in this material are those of the authors and do not necessarily reflect the views, policies, or endorsements, either expressed or implied, of NIH, ONR, or the U.S. Government.
