---
max_turns: 6
allowed_tools: [Skill]
---

Write the commit message for my staged change. You cannot run git here, so
here is what you would see.

`git log --oneline -6`:

```text
9c1d2e4 feat(cli): add a --json flag to the list command
41aa7f0 fix(parser): accept a trailing comma in arrays
a07e112 docs(readme): explain the config file
5b6c903 test(parser): cover empty input
e3f00d1 build(deps): bump yaml to 2.4.1
77d0b2a feat(cli): print the version with --version
```

None of the last 20 commits has a body with `English:` or `Deutsch:`, and none
has a trailer.

`git diff --cached`:

```diff
--- a/src/cli/list.ts
+++ b/src/cli/list.ts
@@ -40,6 +40,9 @@ export function list(opts: Options) {
+  if (opts.limit !== undefined && opts.limit < 1) {
+    throw new UsageError("--limit must be at least 1");
+  }
   const rows = load(opts);
```
