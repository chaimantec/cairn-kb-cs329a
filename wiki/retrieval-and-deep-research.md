# Retrieval and deep research agents

A language model's knowledge is fixed in its weights when training ends. **Retrieval** gives it outside
information at the moment it answers, and a **deep research agent** goes further: it searches repeatedly,
reads what comes back and synthesizes an answer. CS329A meets the idea in several forms. It appears as a
workflow pattern in lecture 1 and as a student's proposal for improving test-time answers in lecture 2. In
lectures 4 and 5 search is an agent's tool. In lecture 7 it becomes a search call inside a reasoning model's
chain of thought, which is the design homework 3 builds on.

## Deep research as a workflow

[Lecture 1](01-course-overview.md) uses deep research as an example of what agents can already do: simpler
tasks such as deep research can be done end to end, while most deployed systems are still hand-built agentic
workflows (≈42:57–44:28). In one cartoon of such a workflow, an LLM is called on many inputs and the results
are aggregated into a summary, as in deep research (≈43:43). Web search is one of the tool calls such
workflows are built from (≈44:28–45:14). Deep research is also the lecture's example of the
**parallelization** pattern, investigating different keywords at once and then aggregating (≈45:14–47:33).
See [agents and agentic workflows](agentic-workflows.md).

## Retrieval as a way to improve answers

In [lecture 2](02-test-time-compute-scaling.md)'s discussion of what to do beyond repeated sampling, one
student proposed a hybrid with a knowledge graph and documentation — retrieval-augmented generation (RAG) —
consulted when accuracy falls short (≈21:26–22:13). The lecturer's list of ways to improve test-time scaling
beyond repeated sampling includes "self-study, search, and tool use" (≈22:13–23:01).

## Search as an agent's action

[Lecture 4](04-learning-from-feedback-with-tools-code.md)'s **ReAct** gives a model a deliberately simple
Wikipedia API for multi-hop question answering (HotpotQA) and fact checking (FEVER). It has three actions:
search for an entity, look up a string on the current page, and finish with an answer (≈17:10; Yao et al.,
§3.1). The reasoning decides what to retrieve. Combining ReAct with self-consistent chain of thought
beats either alone, which the lecturer reads as value in combining the model's internal knowledge with
external knowledge (≈18:41–19:28). Grounding is the gain. In the paper's hand-labelled trajectories,
hallucination causes 56% of chain of thought's failures and none of ReAct's. Retrieval brings its own failure,
though: uninformative searches cause 23% of ReAct's (Yao et al., §3.3, Table 2). When the environment's
feedback is noisy, students propose retrieving several times, among other remedies (lecture 4,
≈23:27–24:13).

[Lecture 5](05-planning-and-multi-step-reasoning.md)'s **SWiRL** trains a model for multi-step tool use,
where answering a multi-hop question with a search engine is the first example task (≈50:51). It learns
when to call the tool, what query to write and when to stop, from offline trajectories with no live tool
calls during training (≈53:10–1:01:08). A model trained on HotPotQA with a search tool improves on HotPotQA
from 65 to 73, and training on either task transfers to the other (≈1:08:14–1:09:03; Goldie et al., Table 2).

## Search inside a reasoning chain

[Lecture 7](07-self-improvement-and-deep-research-agents.md) builds a deep research agent on a large reasoning
model with **Search-o1** (Li et al. 2025). Its starting point is that long reasoning chains hit gaps in the
model's knowledge. The gaps surface as words like "perhaps" and "wait" and then propagate through the rest of
the chain (≈47:03–47:51; §1).

**Why retrieving once is not enough.** Standard RAG turns the question into a query, retrieves documents
once and answers. A multi-part problem needs different information at different steps (≈48:38–49:23). The
paper finds that standard RAG "do[es] not effectively address the knowledge gaps compared to direct
reasoning" (§1).

**Agentic RAG.** The model writes a search query between special symbols whenever it needs one, reasoning
pauses while the search runs, and the results go into the chain. This can happen many times in one session
(≈49:23, ≈54:59–55:46; §3.3).

**Reason-in-Documents.** Retrieved pages are long and noisy, and reasoning models are weak at long
documents. So a separate generation by the same model analyses the documents against the query and the
reasoning so far, and passes back only the refined knowledge (≈55:46–56:34; §3.4). The lecture compares it
to taking notes on references rather than collecting them all (≈56:34).

**Results.** With QwQ-32B-Preview on GPQA's diamond set, Search-o1 scores 63.6 overall against 58.1 for
direct reasoning, 58.6 for standard RAG and 61.6 for agentic RAG without Reason-in-Documents (Table 1). On
multi-hop QA its average exact match is 29.6% higher than standard RAG's and 5.3% higher than agentic RAG's
with the same model (§4.5). It also gets better as it is given more documents per search, where "retrieving
even one document can surpass Direct Reasoning and standard RAG models that use ten retrieved documents"
(§4.4, Figure 3).

**Open questions from the class.** How does the model know what it does not know, and so what to search
for? The lecturer's advice for a real application is to fetch information on the question's key entities
rather than trust the weights (≈53:17–54:59). Would a prompt telling agentic RAG to summarize the documents do
as well on recent models? That is left for homework 3; it depends on how well a model reasons over a long
context (≈57:20–58:55). And retrieval quality itself — precision and recall — sits outside the paper's claim,
which is about reasoning over documents once retrieved (≈1:05:12–1:06:00).

**Prompting or training.** Search-o1 closes the loop with prompting. The lecturer contrasts it with
Search-R1, which is not on the reading list and teaches the model to search with reinforcement learning
(≈1:08:19). See [reinforcement learning](reinforcement-learning.md).

## Lectures

- [Lecture 1 — Course Overview](01-course-overview.md): deep research as an end-to-end agent task, and web
  search and parallelization in agentic workflows.
- [Lecture 2 — Test-Time Compute Scaling](02-test-time-compute-scaling.md): retrieval and search proposed as
  ways to improve beyond repeated sampling.
- [Lecture 4 — Learning from Feedback with Tools/Code](04-learning-from-feedback-with-tools-code.md): ReAct's
  Wikipedia search and lookup actions, grounding against hallucination, and uninformative searches.
- [Lecture 5 — Planning and Multi-Step Reasoning](05-planning-and-multi-step-reasoning.md): SWiRL's training
  for multi-step search-tool use without live tool calls.
- [Lecture 7 — Self-Improvement and Deep Research Agents](07-self-improvement-and-deep-research-agents.md):
  Search-o1's agentic search and Reason-in-Documents on a large reasoning model — the dedicated treatment.
