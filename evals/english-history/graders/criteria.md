---
type: llm
weight: 1
---

The response gives one commit message, in a code block, that:

- is in English only, with no `English:` or `Deutsch:` labels and no German
  half;
- has the type `fix` or `feat` and the scope `cli`, and a subject that says
  the list command now refuses a limit below 1, in the imperative, at most 72
  characters, without a period;
- carries no trailer such as `Co-authored-by:` or `Refs:`.
