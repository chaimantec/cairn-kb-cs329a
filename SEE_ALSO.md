# See also — related knowledge bases

Other Cairn knowledge bases whose material genuinely bears on this course. Each entry gives the
repo URL to pass as `kb` to `kb_read` / `kb_list`, and says what that KB is good for so it is
worth deciding *before* spending a turn on it. This file is the only place the chat learns that
these siblings exist.

The extra links point at `raw.githubusercontent.com` rather than `github.com/.../blob/...`
because a raw URL returns plain markdown, so they also work as `web_fetch` targets.

- **CS336 — Language Modeling from Scratch** (Stanford, Percy Liang and Tatsunori Hashimoto,
  Spring 2026).
  KB: `https://github.com/chaimantec/cairn-kb-cs336` — pass this as `kb` to `kb_read` / `kb_list`.
  Good for the machinery CS329A takes as given: scaling laws measured and fitted, inference systems,
  post-training data, SFT and RLHF, and RL with verifiable rewards (GRPO and its length bias),
  plus reasoning models and test-time scaling from the builder's side. Reach for it when a CS329A
  question is really about how the underlying model is trained or served. **Complete: all 18
  lectures.**
  Start at [INDEX](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs336/main/INDEX.md) ·
  [test-time scaling](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs336/main/wiki/test-time-scaling.md) ·
  [RLVR](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs336/main/wiki/rlvr.md) ·
  [reasoning models](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs336/main/wiki/reasoning-models.md) ·
  [agentic RL](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs336/main/wiki/agentic-rl.md)

- **CS224N — Natural Language Processing with Deep Learning** (Stanford, Christopher Manning with
  guest lecturers, Spring 2024).
  KB: `https://github.com/chaimantec/cairn-kb-cs224n` — pass this as `kb` to `kb_read` / `kb_list`.
  Good for the foundations CS329A's first lecture summarizes in minutes: prompting and
  chain-of-thought, instruction fine-tuning and RLHF explained step by step, and an earlier,
  more sceptical lecture on reasoning and language-model agents (its lecture 15) that makes a useful
  counterpoint. **Complete: all 23 lectures**, but a 2024 course — it predates o1-style reasoning
  models and today's coding agents.
  Start at [INDEX](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs224n/main/INDEX.md) ·
  [chain of thought](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs224n/main/wiki/chain-of-thought.md) ·
  [RLHF](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs224n/main/wiki/rlhf.md) ·
  [language-model agents](https://raw.githubusercontent.com/chaimantec/cairn-kb-cs224n/main/wiki/language-model-agents.md)
