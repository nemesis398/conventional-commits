---
type: llm
weight: 1
---

The response:

- gives the verdict `rewrite needed`;
- names the missing type, the past tense, the trailing period, the noise
  word `WIP` and the vague "stuff", each as a broken rule;
- says the message covers two changes (login and signup) and should be split
  into two commits;
- proposes rewritten messages in the form `<type>(<scope>): <subject>`, with
  the scopes `auth` and `signup`, or asks what the login fix does before it
  writes the subject.
