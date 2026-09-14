---
title: Language Agent Tree Search Unifies Reasoning, Acting, and Planning in Language Models
authors: Andy Zhou, Kai Yan, Michal Shlapentokh-Rothman, Haohan Wang, Yu-Xiong Wang
year: 2024
arxiv: https://arxiv.org/abs/2310.04406
license: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/)
source: arXiv LaTeX source (e-print), fetched 2026-09-14
pdf_pages: 23
course: CS329A lecture 5 (Planning and Multi-Step Reasoning) — site schedule row 5 reading
part: main body
companion: none — the appendices are not transcribed; see the arXiv version
---

# Language Agent Tree Search Unifies Reasoning, Acting, and Planning in Language Models — main body

Full text of Zhou et al. (2024), transcribed from the arXiv LaTeX source and reproduced under CC BY 4.0; copyright the authors. Figures are crops of the published PDF, shown with the paper's own caption; this knowledge base adds no description of its own, so what a figure shows is what its caption and the paper's text say. Citations are resolved to author–year; the bibliography is omitted — follow the arXiv link for it. The appendices are not transcribed in this knowledge base; they are in the arXiv version linked above.

## Contents

| § | Heading | Figures | Tables |
|---|---|---|---|
| — | Abstract | | |
| 1 | Introduction | Figure 1 | |
| 2 | Related Work | | Table 1 |
| 3 | Preliminaries | | |
| 3.1 | Problem Setting and Prompting | | |
| 3.2 | Monte Carlo Tree Search (MCTS) | | |
| 4 | Unifying Reasoning, Acting, and Planning | | |
| 4.1 | LM Agent | | |
| 4.2 | LATS | Figure 2 | Table 2 |
| 5 | Experiments | | |
| 5.1 | HotPotQA | | Table 3 |
| 5.2 | Programming | | Table 4, Table 5 |
| 5.3 | WebShop | | Table 6, Table 7 |
| 5.4 | Ablation Study and Additional Analysis | | Table 8, Table 9, Table 10 |
| 6 | Conclusion | | |
| — | Impact Statement | | |
| — | Acknowledgements | | |

## Abstract

While language models (LMs) have shown potential across a range of decision-making tasks, their reliance on simple acting processes limits their broad deployment as autonomous agents. In this paper, we introduce Language Agent Tree Search (LATS) — *the first general* framework that *synergizes* the capabilities of LMs in reasoning, acting, and planning. By leveraging the in-context learning ability of LMs, we integrate Monte Carlo Tree Search into LATS to enable LMs as agents, along with LM-powered value functions and self-reflections for proficient exploration and enhanced decision-making. A key feature of our approach is the incorporation of an environment for external feedback, which offers a more deliberate and adaptive problem-solving mechanism that surpasses the constraints of existing techniques. Our experimental evaluation across diverse domains, including programming, interactive question-answering (QA), web navigation, and math, validates the effectiveness and generality of LATS in decision-making while maintaining competitive or improved reasoning performance. Notably, LATS achieves state-of-the-art pass@1 accuracy (92.7%) for programming on HumanEval with GPT-4 and demonstrates gradient-free performance (average score of 75.9) comparable to gradient-based fine-tuning for web navigation on WebShop with GPT-3.5. Code can be found at https://github.com/lapisrocks/LanguageAgentTreeSearch.

## 1 Introduction

General autonomous agents capable of reasoning and decision-making in a variety of environments (Wooldridge and Jennings, 1995) have been of longstanding interest in the field of artificial intelligence. While this has traditionally been studied in reinforcement learning, the recent rise of language models (LMs) (Brown et al., 2020; Chowdhery et al., 2023; Touvron et al., 2023; OpenAI, 2023) with strong reasoning and general adaptability offers an alternative paradigm. Not only have LMs excelled in standard natural language processing (NLP) tasks such as summarization (Nallapati et al., 2016) and language inference (Bowman et al., 2015), but they have also been adapted to an increasingly diverse set of tasks that often require advanced common-sense reasoning or quantitative skills (Cobbe et al., 2021; Saparov and He, 2023). In addition, LMs are capable of performing in complex environments that involve knowledge and reasoning, such as web navigation (Yao et al., 2022; Deng et al., 2023), tool-use (Schick et al., 2023), and open-ended games (Fan et al., 2022).

**Figure 1.** Overview of LATS. Serving as a unified framework, LATS leverages an external environment and an MCTS-based search algorithm to improve reasoning and decision-making.

![Figure 1 — overview of the LATS framework](../images/05-planning-and-multi-step-reasoning/lats-figure-1.jpg)

Reasoning and acting abilities have been further improved by prompting techniques that augment LMs with feedback or observations from an external environment, as exemplified by ReAct (Yao et al., 2023b) and other work (Gao et al., 2023; Shinn et al., 2023). This eliminates the need to rely entirely on the base abilities of LMs, enhancing them through external tools or semantic feedback. Despite such strengths, these methods are reflexive and fall short of humans' deliberate and thoughtful decision-making characteristics to solve problems (Sloman, 1996; Evans, 2010). In particular, they fail to consider multiple reasoning paths or to plan ahead. Recent search-guided LM work (Xie et al., 2023; Yao et al., 2023a; Hao et al., 2023) addresses this issue by searching over multiple reasoning chains. While enabling planning, such methods operate in isolation, lacking the incorporation of external feedback that can improve reasoning.

To overcome these challenges, we propose Language Agent Tree Search (LATS) — a *unified* framework for decision-making and reasoning with language models. As illustrated in Fig. 1, LATS *synergizes LM reasoning, acting, and planning* strategies by expanding ReAct (Yao et al., 2023b) into a search over a combinatorial space of possible reasoning and acting steps. This effort is nontrivial — adapting search algorithms to language agents and shifting from non-interactive tasks to interactive ones requires a substantial novel design on nodes, prompts, and search algorithms. In particular, nodes and prompts must effectively store and retrieve external feedback, with the search algorithm incorporating this information into useful heuristics for value assignment. Indeed, our empirical evaluation, as demonstrated on HotPotQA (Yang et al., 2018) in Sec. 5.1, reveals that a simple combination of existing methods is inadequate, even failing to surpass internal reasoning performance, despite having access to the ground truth answer from the environment.

Our *key insight* underpinning LATS is adapting Monte Carlo Tree Search (MCTS), inspired by its success in model-based reinforcement learning (Silver et al., 2017) and *the observation that many LM tasks allow reverting to earlier steps*, to language agents, repurposing pretrained LMs as agents with LM-powered value functions and self-reflections for cleverer exploration. Leveraging the general capabilities and in-context learning abilities of modern LMs, we use language as an interface between each component, allowing LATS to adapt planning to environmental conditions *without additional training*. To the best of our knowledge, LATS is *the first framework* that incorporates reasoning, acting, and planning to enhance LM performance. Notably, LATS doubles the performance of ReAct (Yao et al., 2023b) on HotPotQA (Yang et al., 2018) and raises the average score by $22.1$ on WebShop (Yao et al., 2022) with GPT-3.5. When used with GPT-4, LATS achieves a $92.7$ Pass@1 rate on HumanEval (Chen et al., 2021), setting the state of the art.

Our **contributions** are the following: 1) We introduce LATS, a framework based on Monte Carlo Tree Search to construct the best trajectory from sampled actions, enabling more flexible and adaptive problem-solving compared with reflexive prompting methods. 2) We propose a novel value function that guides the search process and incorporates successful heuristics such as self-refinement and self-consistency. 3) By integrating external feedback and self-reflection, LATS enhances model sensibility and enables agents to learn from experience, surpassing reasoning-based search methods. Through experiments across diverse domains, including programming, interactive question-answering (QA), web navigation, and math, we demonstrate the versatility of LATS for enhancing autonomous reasoning and decision-making.

## 2 Related Work

**Table 1.** Summary of related work on reasoning, acting, and planning. LATS is the *first* work incorporating designs from *all* three domains, allowing broad applicability in all corresponding tasks. We refer to reasoning as LM internal reasoning, acting as external decision-making, planning as the use of a search algorithm, self-reflection as the use of LM-generated feedback, and external memory as storing past text context for future updates of the solution.

| Approach | Reasoning | Acting | Planning | Self-Reflection | External Memory |
|---|---|---|---|---|---|
| CoT (Wei et al., 2022) | ✓ | ✗ | ✗ | ✗ | ✗ |
| ReAct (Yao et al., 2023b) | ✓ | ✓ | ✗ | ✗ | ✗ |
| ToT (Yao et al., 2023a) | ✓ | ✗ | ✓ | ✓ | ✓ |
| RAP (Hao et al., 2023) | ✓ | ✗ | ✓ | ✗ | ✓ |
| Self-Refine (Madaan et al., 2023) | ✓ | ✗ | ✗ | ✓ | ✗ |
| Beam Search (Xie et al., 2023) | ✓ | ✗ | ✗ | ✓ | ✗ |
| Reflexion (Shinn et al., 2023) | ✓ | ✓ | ✗ | ✓ | ✓ |
| **LATS (Ours)** | ✓ | ✓ | ✓ | ✓ | ✓ |

**LMs for reasoning.** For LMs, reasoning involves decomposing complex inputs into sequential intermediate steps towards a final answer (Cobbe et al., 2021), demonstrated with chain-of-thought (CoT) prompting (Wei et al., 2022) and its variants (Wei et al., 2022; Kojima et al., 2022; Wang et al., 2022). However, these methods, which create chains autoregressively in a single step, often suffer from error propagation as the number of steps increases (Guo et al., 2018; Chen et al., 2023b), due to compound errors. Various advancements aim to mitigate this issue; some approaches, such as self-consistency (Wang et al., 2022), employ majority voting over sampled chains, while others focus on multi-step decomposition, such as least-to-most prompting (Zhou et al., 2022). Recently, CoT has been improved with search algorithms (Yao et al., 2023a; Hao et al., 2023; Besta et al., 2023) that can sample trajectories more effectively. Tree-of-thought (ToT) prompting (Yao et al., 2023a) uses DFS or BFS-based (depth/breadth-first) search guided by an LM-generated heuristic, while reasoning via planning (RAP) (Hao et al., 2023) uses MCTS with rollouts simulated by LMs. However, they rely solely on LM internal knowledge and cannot adapt to useful external feedback.

**LMs for acting.** The strong reasoning and common-sense abilities of LMs have been further adapted for decision-making or acting tasks as a policy model in interactive environments. In robotics, LMs have been employed as high-level controllers of control policies (Ahn et al., 2022; Huang et al., 2022; Driess et al., 2023). Similar work (Baker et al., 2022; Wang et al., 2023) has also adapted LM agents to complex multimodal games such as Minecraft (Guss et al., 2019; Fan et al., 2022). LMs are particularly useful in text-based environments (Liu et al., 2018; Shridhar et al., 2020; Liu et al., 2024), where acting-based prompting techniques such as ReAct (Yao et al., 2023b) have seen success. Similar to CoT, ReAct is limited by its simplicity and cannot effectively adapt to environment conditions. Many extensions have been proposed to address this issue, including self-refine (Madaan et al., 2023) and Reflexion (Shinn et al., 2023), which use self-improvement to enhance reasoning and decision-making, and AdaPlanner (Sun et al., 2023), which incorporates both positive and negative feedback. However, these methods focus on refining an individual trajectory and do not consider alternative choices at each step. In addition, recent work (Huang et al., 2024) has suggested that LMs cannot self-correct their internal reasoning, making it critical to use external feedback. Alternatively, to pure decision-making environments, the reasoning and practical abilities of LMs have been enhanced by providing access to external tools, such as APIs, search engines, calculators, and other models (Schick et al., 2023; Shen et al., 2023; Surís et al., 2023). We summarize prior work in Tab. 1.

**Tree-based search.** Tree-based search, where multiple branches of outcomes are explored during search, is widely used in many planning algorithms (Swiechowski et al., 2021; LaValle, 1998) and reinforcement learning (RL) (Hafner et al., 2019; Du et al., 2023; Wu et al., 2023) algorithms for its good exploration-exploitation trade-off. Note that though tree-based search necessitates an environment model that can expand from an arbitrary state (Vodopivec et al., 2017), often requiring extra training in RL (Hafner et al., 2023), such a problem *does not* exist for most LM tasks. This is because we can conveniently revert to any state by setting the input to be the context and the corresponding previous output from the LM for many tasks. Thus, we operate on the tree-based framework and use MCTS (Swiechowski et al., 2021) to fully unlock the potential of LMs. In addition, we avoid the cost of training a value function over language descriptions by leveraging the in-context learning (Brown et al., 2020) abilities of LMs. Concurrent work (Liu et al., 2023) also explores combining search algorithms with LM agents but uses an off-the-shelf search algorithm, which may not be optimal for LMs. Finally, following Yao et al. (2023a) and Hao et al. (2023), we note that we use *planning* and *search algorithms* interchangeably in this paper.

## 3 Preliminaries

### 3.1 Problem Setting and Prompting

We first define our problem and outline a few established methods that leverage language models for reasoning *or* decision-making. In LM reasoning or decision making, we are given an input $x$ in natural language and a pretrained language model $p_\theta(x)$ parameterized by $\theta$; our goal is to generate a final output $y\sim p_\theta(x)$ that corresponds to the answer (reasoning) or completes the task (decision-making). Both $x$ and $y$ are language *sequences*, which are comprised of a list of *tokens* (the basic elements of natural language, often words), denoted as $x = (x[1], \dots, x[l_x])$ and $y = (y[1], \dots, y[l_y])$ where $l_x$ and $l_y$ are the length. The LM decodes text autoregressively, i.e., without other inputs, the probability for an LM to generate a sequence $y$ is given by $p_\theta(x) = \prod_{i=1}^{l_x} p_\theta(x[i] | x[1 \dots i-1])$. Usually, to improve reasoning, *prompts* are provided along with the input $x$, which are specific instructions or few-shot input-output examples. We denote the generic process where an input $\texttt{prompt}_ {\mathrm{IO}}(x)$ is transformed into an output $y$ by LM: $y \sim p_\theta(\texttt{prompt}_ {\mathrm{IO}}(x))$.

**Chain-of-thought (CoT) prompting** (Wei et al., 2022) caters to scenarios where the direct mapping from $x$ to $y$ is intricate, e.g., when $x$ is from a mathematical query or challenging question. It hinges on creating *thoughts* $z_1, \dots, z_l$ that act as stepping stones between $x$ and $y$; each thought $z_i$ is a language sequence. To employ CoT prompting, thoughts are extracted sequentially as $z_i \sim p_\theta^{\mathrm{CoT}}(x, z_{1\cdots i-1})$, with the final output being $y \sim p_\theta^{\mathrm{CoT}}(x, z_{1 \cdots l})$.

**Tree-of-thought (ToT) prompting** (Yao et al., 2023a) extends CoT prompting by exploring multiple reasoning paths over thoughts. It frames problems as a search over a tree, where each node $s=[x, z_{1\cdot i}]$ represents a partial solution state comprising the original input $x$ and the thought sequence $z_{1\cdots i}$. Thoughts $z_{i}$ are generated by proposal or sampling with CoT $z_i \sim p_\theta^\mathrm{CoT}(x, z_{1\cdots i-1})$. Search algorithms like depth-first (DFS) or breadth-first (BFS) search are used to systematically explore the tree, guided by heuristics based on LM evaluations $V(s)$ of each state.

**ReAct** (Yao et al., 2023b) extends language models to tasks where the mapping from $x$ to $y$ is enhanced by or requires interactions with an external environment, such as a game or API. This technique constructs an action space $\hat{A} = A \cup Z$ that adds permissible actions $a \in A$ to the reasoning traces $z \in Z$ from CoT. Observations $o$ from the environment are used to improve both reasoning and acting. To solve problems with ReAct, after each observation, actions are generated from $p_\theta$ sequentially as $a_i \sim p_\theta^\mathrm{ReAct}(x, o_{1\cdots i-1}, a_{1\cdots i-1})$, with the final output being $y \sim p_\theta^\mathrm{ReAct}(x, o_{1\cdots l}, a_{1\cdots l})$. In this paper, consistent with other LM agent methods such as ReAct and Reflexion (Shinn et al., 2023), we focus on decision-making tasks *where reverting between iterations is feasible*.

While the previously described prompting techniques improve LM performance on reasoning tasks, they falter on difficult tasks that involve multifaceted decision-making due to several shortcomings: 1) *Flexibility*: Base prompting designs (CoT or ReAct) autoregressively sample from the LM, neglecting potential alternative continuations from specific states. 2) *Sensibility*: Reasoning-based methods (CoT, RAP (Hao et al., 2023), or ToT) rely solely on the internal representations of the LM and cannot consider external observations. This dependency risks fact hallucination and error propagation while setting a performance ceiling. 3) *Adaptability*: Current planning strategies (RAP or ToT) use simple search algorithms such as BFS or cannot leverage environmental feedback to improve planning. Additionally, the agent is static and cannot reuse previous experience or learn from trial and error. While RAP also adopts MCTS, it is constrained to tasks where the LM can become a world model and accurately predict states. These shortcomings limit the ability of LMs to be deployed as general problem-solving agents and form the motivation for LATS.

### 3.2 Monte Carlo Tree Search (MCTS)

Monte Carlo Tree Search (MCTS) is a heuristic search algorithm that is proved successful on many decision-making environments, such as Atari (Ye et al., 2021) and Go (Silver et al., 2016). MCTS builds a decision tree where every node in the tree is a state and edge is an action. MCTS runs for $k$ episodes; for each episode, it starts from the root (i.e., initial state) and iteratively conducts two steps to expand the tree: 1) *Expansion*, where multiple children states $s$ are explored from the current parent state $p$ by sampling $n$ actions, and 2) *Selection*, where the children with the highest UCT *(Upper Confidence bounds applied to Trees)* (Kocsis and Szepesvári, 2006) value is selected for expansion by the next iteration. The UCT of a child state $s$ is calculated as follows:

$$UCT(s)=V(s)+w\sqrt{\frac{\ln N(p)}{N(s)}} \tag{1}$$

where $N(s)$ is the number of visits to a node $s$, $V(s)$ is the value function (expected return) from the subtree of $s$, $w$ is the exploration weight, and $p$ is the parent node of $s$. When the end of an episode is reached, a *backpropagation* is carried out: the return $r$ is used for updating every $V(s)$ along the path with the formula $V(s)=\frac{V_{\text{old}}(s)(N(s)-1)+r}{N(s)}$, where $V_{\text{old}}(s)$ is the old value function. Normally, the major shortcoming of MCTS is that it requires an environment model to undo previous steps and form a searching tree, which could be a strong assumption. However, this limitation *does not* exist for many LM tasks, as we can conveniently reset to any step by simply copy-pasting historical text input. Such a special property is the key motivation of our work.

## 4 Unifying Reasoning, Acting, and Planning

### 4.1 LM Agent

Depending on the base prompting framework design, LATS supports sequential reasoning or decision-making tasks. At time step $t$, an agent receives an observation $o_t \in O$ from the environment and takes an action $a_t \in A$ following some policy $\pi(a_t | x, o_{1\cdots t-1}, a_{1\cdots t-1})$. We initialize the agent with $p_\theta$ to leverage the useful language representations of an LM as a base decision-maker. We follow the ReAct instantiation, in which the action space $\hat{A} = A \cup Z$ consists of both the space of permissible actions $A$ and the language space of reasoning traces $Z$. Actions directly affect the environment and result in observation, while thoughts are used to formalize decisions by organizing information, planning future actions, or injecting internal knowledge. The exact instantiation of the action space depends on the particular environment — for decision-making tasks actions might consist of commands on a website, while for reasoning tasks the action space might be limited to a few external tools or APIs. In environments without feedback, such as reasoning tasks, we use CoT as the base prompting framework.

Instead of greedily decoding one trajectory or solution, we sample $n$ actions from $p_\theta$ using the current state. This is based on the intuition that for complex decision-making tasks, there is likely to be a range of potential trajectories or reasoning paths that are correct (Evans, 2010). Sampling a diverse set of candidates at each step mitigates the stochastic nature of LM text generation and enables greater exploration in both the decision-making and reasoning space. We wrap $p_\theta$ within our proposed search algorithm to deliberately construct the best trajectory from sampled actions.

### 4.2 LATS

**Figure 2.** Overview of the six operations in LATS. A node is *selected*, *expanded*, *evaluated*, then *simulated* until a terminal node is reached, and then the resulting value is *backpropagated*. If the trajectory fails, a *reflection* is generated and used as additional context for future trials. These operations are performed in succession until the budget is reached or the task is successful.

![Figure 2 — the six operations of LATS](../images/05-planning-and-multi-step-reasoning/lats-figure-2.png)

The main component of LATS is a search algorithm that controls the problem-solving process with planning. To find the most promising trajectory and systemically balance exploration with exploitation, we adopt a variant of MCTS that frames decision-making as a tree search, in which each node $s=[x, a_{1\cdots i}, o_{1\cdots i}]$ represents a state comprising the original input $x$, action sequence $a_{1\cdot i}$, and observation sequence $o_{1\cdot i}$, where $i$ is a token in the text sequence.

Our main technical contribution is *adapting MCTS to language agents*. LATS repurposes $p_\theta$ as an agent, state evaluator, and feedback generator, leveraging the useful language representations of modern LMs to facilitate planning. While standard MCTS and RAP (Hao et al., 2023) rely on internal dynamics models to facilitate simulation, LATS uses environment interaction and does not require a world model. As depicted in Fig. 2, LATS consists of a series of operations — *selection, expansion, evaluation, simulation, backpropagation, and reflection* — performed in succession until the task is successfully completed or a computational limit is reached after sampling $k$ trajectories. The full pseudocode of LATS can be found in Sec. A in the Appendix.

**Selection.** In the first operation, the algorithm identifies a segment of the current tree most suitable for subsequent expansion. Starting from the root node, denoted as the initial state $s_0$, a child node is selected at each tree level until a leaf node is reached. To balance exploration and exploitation, we use the UCT algorithm as shown in Eq. 1.

**Expansion.** After selecting a node, the second operation expands the tree by sampling $n$ actions from $p_\theta$, as described in the prior section. The environment receives each action and returns corresponding feedback as an observation. This results in $n$ new child nodes added to the tree. This tree is stored in an external long-term memory structure.

**Evaluation.** The third operation assigns a scalar value to each new child node for selection and backpropagation. This value effectively quantifies the agent's progress in task completion, serving as a heuristic to steer the search algorithm towards the most promising regions of the tree. As LATS does not involve training, we propose a novel value function for this setting based on two components: (1) a *self-generated* LM score and (2) a *self-consistency* score.

Inspired by ToT, we repurpose $p_\theta$ into a value function by prompting it to reason about a given state. To obtain a scalar value, we instruct $p_\theta$ to end its reasoning trace with a score indicating the correctness of the trajectory. Our key distinction from ToT is that we obtain this value after obtaining the environmental feedback, improving value assignment. This also enables scaling to more challenging environments, as it is difficult for LMs to improve their responses without external feedback (Huang et al., 2024). Additionally, to further improve value assignment, we introduce an additional heuristic based on self-consistency (Wang et al., 2022), in which actions sampled multiple times at the same state tend to be more accurate. This results in the overall value function:

$$V(s) = \lambda \ast \text{LM}(s) + (1-\lambda) \ast \text{SC}(s) \tag{2}$$

where $\lambda$ is a hyperparameter. Notably, our method offers enhanced flexibility over programmed heuristics (Campbell et al., 2002) and greater efficiency than learned heuristics (Silver et al., 2017).

**Simulation.** The fourth operation expands the currently selected node until a terminal state is reached. At each depth level, we sample and evaluate nodes with the same operations but prioritize nodes of the highest value. Reaching a terminal state provides objective feedback on the correctness of a trajectory. If the task is completed successfully, then LATS terminates the search. If the solution is partially successful or unsuccessful, then we perform two additional operations as described below. The success of a trajectory is determined by the design of the specific environment, such as finalizing a purchase in web navigation environments.

**Backpropagation.** This operation updates the values of the tree based on the outcome of a trajectory. For each node $s_0,s_1,\dots, s_l$ in the trajectory from root (initial state $s_0$) of the searching tree to leaf (terminal state $s_l$), its value is updated to reflect the outcome of the simulation by $N(s_i)=N(s_{i-1})+1$ and $V(s_i)=\frac{V(s_{i-1})N(s_{i-1})+r}{N(s_i)}$, where $r$ is the reward. These updated values are used in the UCT formula (Eq. 1) to guide the selection of the next node.

**Reflection.** In addition to the environmental feedback, we leverage *self-reflection* to further refine the decision-making process (Shinn et al., 2023; Madaan et al., 2023). Upon encountering an unsuccessful terminal node, $p_\theta$ is prompted with the trajectory and final reward to provide a verbal self-reflection that summarizes the errors in the reasoning or acting process and proposes superior alternatives. We store both failed trajectories and corresponding reflections in the memory. In subsequent iterations, these are integrated as additional context to the agent and value function, refining both through in-context learning. This imparts a semantic gradient signal more useful than a scalar value, enabling the agent to learn from trial and error without the cost of expensive optimization such as reinforcement learning.

**Discussion.** Conceptually, LATS has several notable advantages as a general framework for reasoning and decision-making with LM agents. (1) *Generality*: LATS supports both reasoning and decision-making tasks by defining a shared space of thoughts and actions. (2) *Deliberation*: Leveraging MCTS and LM value function in LATS ensures a principled search that selects options with high value while exploring promising alternatives. (3) *Adaptability*: Incorporating external feedback through observations and self-reflection in LATS enables greater adaptation during problem-solving. (4) *Flexibility*: LATS can accommodate different scenarios, environments, and resource stipulations by modifying state design and tree dimensions. (5) *Modularity*: The base LM agent, reflection generator, and value function can be independently altered and adapted to individual LM properties.

**Table 2.** GPT-3.5 *reasoning*-based prompting results on HotpotQA. LATS achieves the highest exact match (EM) for reasoning. We sample $n=5$ nodes during expansion and $k = 50$ trajectories.

| Prompt Method | HotpotQA (EM) $\uparrow$ |
|---|---|
| Base LM | 0.32 |
| CoT (Wei et al., 2022) | 0.34 |
| CoT - SC (Wang et al., 2022) | 0.38 |
| ToT (Yao et al., 2023a) | 0.55 |
| RAP (Hao et al., 2023) | 0.60 |
| RAP ($n = 10$) | 0.60 |
| LATS (CoT) | **0.62** |

## 5 Experiments

To demonstrate the general applicability of LATS, we evaluate our method on a variety of domains that require reasoning and acting: programming (Chen et al., 2021; Austin et al., 2022), HotPotQA (Yang et al., 2018), WebShop (Yao et al., 2022), and Game of 24 (Yao et al., 2023a).

### 5.1 HotPotQA

**Table 3.** GPT-3.5 *acting*-based prompting results on HotpotQA. LATS achieves the highest exact match (EM) for acting. We sample $n=5$ nodes and use $k = 50$ trajectories. We also evaluate sampling ReAct $k$ times and using both CoT and ReAct base prompting designs for LATS, which achieves the best performance. Note that LATS outperforms ToT and RAP with ReAct prompting, which are the simple adaptations of search algorithms to decision-making.

| Prompt Method | HotpotQA (EM) $\uparrow$ |
|---|---|
| ReAct (Yao et al., 2023b) | 0.32 |
| ReAct (best of $k$) | 0.38 |
| Reflexion (Shinn et al., 2023) | 0.51 |
| *ToT (ReAct)* | 0.39 |
| *RAP (ReAct)* | 0.54 |
| LATS (ReAct) | 0.63 |
| LATS ($n = 3$) | 0.58 |
| LATS ($n = 10$) | 0.65 |
| LATS (CoT + ReAct) | **0.71** |

For a task that can be approached with both reasoning-based and acting-based strategies, we consider HotPotQA (Yang et al., 2018), a multi-hop question-answering benchmark that requires retrieval over two or more Wikipedia passages. For the action space, in addition to LM thoughts, we follow the setup from Yao et al. (2023b), which provides the agent with API calls to search and retrieve information. The output of these API calls and self-generated reflections form the observation space. Note that consistent with previous work (Yao et al., 2023b; Shinn et al., 2023), we use an oracle setup for HotPotQA, in which the environment provides feedback about the answer's correctness upon receiving an answer. This enables a fair comparison between our method and baselines in scenarios where the quality of feedback is high, allowing us to focus our evaluation on how well the agent incorporates external feedback. We use a subset of 100 questions and three few-shot examples for each method. For ToT, we use DFS as the base search algorithm. For all methods that involve sampling, including LATS, we sample $k=50$ trajectories. More details are in Appendix Sec. D.

We evaluate internal reasoning strategies by removing actions and observations from the context, corresponding to CoT (Wei et al., 2022) and its variants, CoT-SC (Wang et al., 2022), ToT (Yao et al., 2023a), and RAP (Hao et al., 2023). These methods rely solely on the agent's existing knowledge to answer the question. We further consider acting-based methods ReAct, Reflexion, and LATS, which augment the agent with the interactive API environment and primarily evaluate its information retrieval abilities. We also design a simple integration of search algorithms with LM agents, extending ToT and RAP with ReAct prompting to handle external observations. In addition, while LATS is designed for scenarios where external feedback can enhance reasoning, we also implement a reasoning-only version with CoT as the base prompting framework. Moreover, we combine internal and external reasoning in LATS by first prompting with a CoT-based prompt and then switching to a ReAct-based prompt upon failure. This is closer to how humans might approach this task by using tools to retrieve additional information only when the answer is not already known.

**Results.** We observe in Tab. 2 and Tab. 3 that both internal reasoning and external retrieval strategies perform well on HotPotQA. Due to their large-scale training corpus, modern LMs already encode factual knowledge and can often directly answer the question correctly. While CoT can slightly enhance performance on questions requiring reasoning, larger gains are observed with search methods ToT and RAP (Tab. 2, Row 4, 5), which can sample and explore more outputs. We observe similar results for acting-based methods. LATS surpasses ReAct, even when sampling the same number of trajectories, by expanding more nodes with principled search. This is demonstrated when modifying $n$, the number of nodes expanded during each iteration. Increasing $n$ can consistently improve performance, although at greater computational and inference costs. LATS also outperforms RAP on internal reasoning, but has higher performance on the decision-making setting of HotPotQA than the reasoning setting. Contrary to LATS, the ReAct versions of ToT and RAP (Tab. 3, Row 4, 5) *perform even worse than the reasoning-only setting* of HotPotQA, which indicates that the acting-based setting is more challenging and *adaptation of search algorithms to decision-making scenarios is non-trivial*. Combining internal and external reasoning in LATS results in the highest performance, indicating the importance of external feedback in augmenting reasoning even in tasks where the base LM can already perform.

### 5.2 Programming

**Table 4.** GPT-3.5 and GPT-4 Pass@1 accuracy on HumanEval. Prompting with LATS achieves the best performance. We sample 5 solutions during expansion for 8 iterations.

| Prompt Method | Model | Pass@1 $\uparrow$ |
|---|---|---|
| CoT (Wei et al., 2022) | GPT-3.5 | 46.9 |
| ReAct (Yao et al., 2023b) | GPT-3.5 | 56.9 |
| Reflexion (Shinn et al., 2023) | GPT-3.5 | 68.1 |
| ToT (Yao et al., 2023a) | GPT-3.5 | 54.4 |
| RAP (Hao et al., 2023) | GPT-3.5 | 63.1 |
| LATS (ReAct) | GPT-3.5 | **83.8** |
| Base LM | GPT-4 | 80.1 |
| Reflexion | GPT-4 | 91.0 |
| LATS (ReAct) | GPT-4 | **92.7** |

To demonstrate the importance of external observations for complex reasoning tasks, we evaluate the baselines and LATS on programming with HumanEval (Chen et al., 2021)[^1] and MBPP (Austin et al., 2022). Both datasets measure the correctness of synthesized programs in Python from natural language docstrings. We use individual solutions as the action space and test suite and compiler feedback as the external observation. We follow Chen et al. (2023a) and use an LM to generate a synthetic test suite of syntactically valid "assert" statements for each question. For each step, the solution is evaluated on this test suite, and the results, including successful and failed tests and compiler output, are added to the context as an observation.

For this task, the reasoning and acting baselines share an action space, but acting methods are able to incorporate observations as additional context. For LATS, since each action corresponds to a complete solution, we skip the simulation step of LATS and directly use the percentage of passed tests as the backpropagated reward. We use $k = 8$ iterations, set the number of generated tests at $4$, and sample $n = 5$ solutions during expansion. After the search is completed, we select the solution with the highest value and evaluate it on the real test suite for the pass@1 accuracy evaluation. More details can be found in Appendix Sec. D.

**Results.** Tab. 4 and Tab. 5 show that both search and semantic feedback are crucial for better performance. Despite not using observations, ToT and RAP are competitive with Reflexion. LATS has the highest performance on both datasets. RAP uses a search algorithm similar to LATS, which reveals the importance of external feedback for difficult reasoning tasks such as programming. With GPT-4, using LATS sets the state of the art for HumanEval, validating that LATS can be used with more advanced LMs for higher performance.

[^1]: Some baselines use 161 questions from HumanEval. We use all 164 questions for LATS and find minimal performance differences, so we report baselines for both settings.

**Table 5.** GPT-3.5 Pass@1 accuracy on MBPP. Prompting with LATS achieves the highest performance. We sample 5 solutions during expansion for 8 iterations.

| Prompt Method | Pass@1 $\uparrow$ |
|---|---|
| CoT (Wei et al., 2022) | 54.9 |
| ReAct (Wei et al., 2022) | 67.0 |
| Reflexion (Shinn et al., 2023) | 70.0 |
| ToT (Yao et al., 2023a) | 65.8 |
| RAP (Hao et al., 2023) | 71.4 |
| LATS (ReAct) | **81.1** |

### 5.3 WebShop

For a complex decision-making environment with practical applications, we consider WebShop (Yao et al., 2022), an online shopping environment composed of a website with 1.18M real-world products and 12k human instructions. Agents must navigate a website through a variety of commands to purchase an item matching a user specification. We use the preconstructed action space of search and click commands and browser feedback and reflections for the observation. The performance is gauged using two metrics: an average score, reflecting the percentage of user-specified attributes met by the selected product, and a success rate, indicating the frequency with which the chosen product fulfills all given conditions. We compare against acting-based prompting methods and RL-based approaches. We evaluate on 50 instructions, expand $n=5$ children for LATS, and set $k=30$ for LATS, ReAct (best of $k$), and Reflexion. More details and prompts are in Appendix Sec. D and Sec. G.

**Results.** We find in Tab. 6 that GPT-3.5 with ReAct is competitive to imitation learning (IL) and can exceed reinforcement learning techniques with stronger prompting strategies. Sampling $k=30$ trajectories with ReAct and Reflexion results in a similar performance, suggesting the semantic feedback is not as helpful in complex environments like WebShop. Similar to Shinn et al. (2023), we find that generated reflections are often generic and do not provide useful feedback, resulting in a tendency for the agent to become stuck in local minima. However, using LATS indeed results in a noticeable improvement, indicating a more effective exploration for the same number of iterations.

**Table 6.** Score and success rate (SR) on WebShop. Results are organized into prompting, RL-based training, and human performance. For the same number of iterations, LATS improves both score and SR and surpasses RL-based training.

| Method | Score $\uparrow$ | SR $\uparrow$ |
|---|---|---|
| ReAct (Yao et al., 2023b) | 53.8 | 28.0 |
| ReAct (best of k) | 59.1 | 32.0 |
| Reflexion (Shinn et al., 2023) | 64.2 | 35.0 |
| LATS (ReAct) | **75.9** | **38.0** |
| IL (Yao et al., 2022) | 59.9 | 29.1 |
| IL+RL (Yao et al., 2022) | 62.4 | 28.7 |
| Fine-tuning (Furuta et al., 2024) | 67.5 | 45.0 |
| *Expert* | 82.1 | 59.6 |

**Table 7.** Results on Game of 24 with GPT-3.5. We sample $n=5$ nodes and $k=30$ trajectories.

| Prompt Method | Game of 24 (Success Rate) $\uparrow$ |
|---|---|
| CoT (Wei et al., 2022) | 0.08 |
| Reflexion (Shinn et al., 2023) | 0.12 |
| ToT (Yao et al., 2023a) | 0.20 |
| RAP (Hao et al., 2023) | 0.40 |
| LATS (CoT) | **0.44** |

### 5.4 Ablation Study and Additional Analysis

We further test the reasoning ability of LATS on Game of 24, and also conduct additional experiments on HotPotQA to demonstrate the effect of each component of LATS (results shown in Tab. 8). More ablations for token consumption on HotPotQA are in Tab. 9 in Appendix Sec. C.

**Reasoning on Game of 24.** To show how LATS can be applied to purely internal reasoning tasks, we additionally evaluate on Game of 24 (Yao et al., 2023a), a mathematical reasoning task where the agent must construct 24 out of a set of numbers and basic operations. We use CoT as the base prompting design and employ the same operations as in other settings. We find in Tab. 7 that LATS outperforms previous methods proposed specifically for reasoning. This is due to our proposed value function, which incorporates self-consistency as an additional heuristic.

**Table 8.** Ablation results on LATS and baseline variants in HotPotQA. We use ReAct as the base prompt and sample $n=5$ children and $k=50$ trajectories. LATS requires every component and operation for optimal performance.

| Prompt Method | HotPotQA (EM) $\uparrow$ |
|---|---|
| ToT (ReAct) | 0.39 |
| RAP (ReAct) | 0.54 |
| LATS (No LM Heuristic) | 0.37 |
| LATS (DFS) | 0.42 |
| LATS (No Reflection) | 0.58 |
| LATS (ReAct) | **0.63** |

**Self-reflection.** LATS uses self-reflection to provide additional semantic signals for the agent. In Tab. 8 (Row 5, 6), we observe a $0.05$ performance drop when self-reflection is removed from LATS, validating its usefulness. This is a smaller gain than the $0.19$ gain that Reflexion has over ReAct as shown in Tab. 3, suggesting overlap between the questions where an answer can be improved by self-reflection and search. This variant outperforms RAP (ReAct), reflecting our improvements to MCTS.

**Table 9.** Performance, sample complexity of different methods, average number of nodes expanded, and token consumption upon success by methods with tree-based search. $n$ is the number of children nodes expanded at every step and $k$ is the number of trajectories. LATS has the same sample complexity as other methods with tree-based search and expands less nodes upon success, which indicates lower token cost.

| Method | Performance $\uparrow$ | Sample complexity $\downarrow$ | Token Consumption $\downarrow$ |
|---|---|---|---|
| ReAct (Best $k=250$) | $0.42$ | $O(k)$ | - |
| CoT-SC ($n=1, k=250$) | $0.40$ | $O(k)$ | - |
| LATS ($n=1, k=50$) | $0.48$ | $O(k)$ | - |
| ToT (ReAct, $n=5,k=50$) | $0.49$ | $O(kn)$ | $210,215$ |
| RAP (ReAct, $n=5,k=50$) | $0.54$ | $O(kn)$ | $176,500$ |
| LATS ($n=5,k=50$) | $0.63$ | $O(kn)$ | $173,290$ |

**Table 10.** Comparison of the cost of different methods on HotPotQA. LATS achieves the highest accuracy and the lowest average number of nodes/states required for success at various $k$ trajectories sampled.

| Method | $k$ | HotPotQA $\uparrow$ | # of Nodes $\downarrow$ |
|---|---|---|---|
| ToT | 10 | 0.34 | 33.97 |
| RAP | 10 | 0.44 | 31.53 |
| LATS | 10 | 0.44 | 28.42 |
| ToT | 30 | 0.39 | 47.54 |
| RAP | 30 | 0.50 | 37.71 |
| LATS | 30 | 0.52 | 34.12 |
| ToT | 50 | 0.49 | 84.05 |
| RAP | 50 | 0.54 | 70.60 |
| LATS | 50 | **0.61** | **66.65** |

**Search algorithm.** MCTS is a more principled search algorithm than variants like A* (Zhuang et al., 2023) or DFS and is the basis for observed performance gains. We observe the effects of using DFS, and incorporate the LM-based heuristic used in ToT in which branches with low values are pruned. This removes the selection and backpropagation operations, and we observe a $0.21$ drop in performance in Tab. 8 (Row 4) when sampling the same number of nodes but outperforms ToT (ReAct). Despite also benefiting from ground-truth feedback, LATS uses it better than ToT and RAP and can outperform these methods. We also find in Tab. 8 (Row 3) that LM scoring, the main component of our value function, is crucial for leveraging external feedback and strong performance.

**Sample complexity and token consumption.** One possible concern of LATS is that the tree-structured search might consume much more tokens than existing methods. To further study the computational cost of LATS compared to prior methods, we examine the sample complexity (i.e., asymptotic token cost) of all methods considered in this paper and count the average number of nodes expanded by our method and other tree-structured methods (ToT and RAP) upon successful search on HotPotQA. We present the results in Tab. 9 and Tab. 10, which show that our method has the same sample complexity as other tree-based search methods and requires fewer overall tokens and states. The token cost gap will be even larger when taking failed trajectories into account, since our method has a higher success rate and reaches the computational budget limit less often. This is also true when sampling a smaller number of trajectories; on average, LATS requires 3.55 fewer nodes than RAP and 12.12 fewer nodes than ToT. These findings underscore our improvements to MCTS and adaptation to LM agents, resulting in a more principled and efficient search mechanism.

## 6 Conclusion

This work introduces Language Agent Tree Search (LATS), the first framework to unify reasoning, acting, and planning for enhanced LM problem-solving. LATS addresses key limitations of prior prompting techniques by deliberately constructing trajectories with search algorithms, incorporating external feedback, and enabling agents to learn from experience. Our evaluation demonstrates the ability of LATS to harness LM capabilities for various decision-making tasks while maintaining its reasoning ability *without additional training*. The proposed synergies between search, interaction, and reflection offer a versatile approach to autonomous decision-making, highlighting the potential of LMs as generalist agents.

**Limitations and future directions.** LATS has two main limitations that should be considered before its application. First, it has a higher computational cost compared to simpler prompting methods like ReAct or Reflexion, which may limit its practicality in certain situations. Second, LATS assumes the ability to revert to earlier states in decision-making environments, which may not be universally applicable in all possible environments. Despite these limitations, it is worth noting that LATS still achieves better performance and efficiency compared to similar methods, and the number of nodes expanded at each step provides a trade-off between performance and efficiency. Additionally, we expect inference-time compute costs to decrease over time, thereby increasing the usefulness of LATS and other “System-2” LM approaches. Finally, the reversion property is feasible in many real-world applications, opening up new opportunities in the LM decision-making community. Future directions include scaling LATS to more complex environments or multi-agent frameworks and improving efficiency to reduce costs. A more detailed discussion about the limitations of LATS can be found in Appendix Sec. B.

## Impact Statement

LATS is a framework that enhances LM performance through interactions with an environment. This improvement in autonomous decision-making may facilitate harmful uses of LMs. On the other hand, LATS enhances interpretability and the potential for greater alignment, as it involves high-level linguistic reasoning and actions through several rounds of decision-making and reflection rather than relying on autoregressive generation. Finally, enhancing the capabilities of LM agents may raise security risks, such as executing malware. We encourage further research to fully understand and mitigate the risks of LMs.

## Acknowledgements

We thank Daniel Campos for useful feedback on earlier versions of this paper. This work was supported in part by NSF Grant 2106825, NIFA Award 2020-67021-32799, the Jump ARCHES endowment through the Health Care Engineering Systems Center at Illinois and the OSF Foundation, and the IBM-Illinois Discovery Accelerator Institute. This work used NVIDIA GPUs at NCSA Delta through allocations CIS220014, CIS230012, and CIS230218 from the ACCESS program.
