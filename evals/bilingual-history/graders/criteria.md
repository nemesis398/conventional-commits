---
type: llm
weight: 1
---

The response gives one commit message, in a code block, that:

- has the type `fix` and the scope `faq`, with an English subject about the
  search ignoring upper and lower case, in the imperative, without a period;
- is bilingual like the history: an `English:` body, a line with only `---`,
  the same type and scope with a German subject, and a `Deutsch:` body that
  says the same as the English one;
- adds no trailer and no breaking-change marker.

A monolingual message fails.
