---
name: karpathy-guidelines
description: Guidelines to avoid common LLM coding mistakes (silent assumptions, overcomplication, drive-by edits, weak success criteria). Use when writing, reviewing or refactoring code.
---

Source thread: [thread.md](thread.md). Biases toward caution; for trivial tasks use judgment.

1. **Think first.** State assumptions; if unsure, ask. Present multiple interpretations instead of picking silently. Name confusion, surface tradeoffs, push back when a simpler way exists.
2. **Simple.** Minimum code that solves the ask. No extra features, single-use abstractions, unrequested config, or handling of impossible cases. If 200 lines could be 50, rewrite.
3. **Surgical.** Every changed line traces to the request. No adjacent refactors, comment/format edits, or style changes. Remove only orphans your change created; mention unrelated dead code, don't delete it.
4. **Goal-driven.** Turn tasks into verifiable criteria (bug → failing test first, then pass; refactor → tests green before and after). Prefer declarative goals over imperative steps. For multi-step work, give a short plan with a check per step.
