---
max_turns: 6
allowed_tools: [Skill]
---

Clean up this commit message for my staged change, keep everything it says:

```text
updated config stuff. BREAKING: API_TOKEN is now required, also bumped a couple of deps
```

You cannot run git here, so here is what you would see.

`git diff --cached --name-only`:

```text
src/config.ts
```

`git diff --cached`:

```diff
--- a/src/config.ts
+++ b/src/config.ts
@@ -8,9 +8,10 @@ export function load(env: Env): Config {
   const token = env.API_TOKEN;
   if (!token) throw new Error("API_TOKEN is required");
-  const timeout = Number(env.TIMEOUT ?? 30);
+  const timeout = Number(env.TIMEOUT_SECONDS ?? env.TIMEOUT ?? 30);
   return { token, timeout };
 }
```

`git log -S'API_TOKEN is required' --oneline`:

```text
3e9a1c0 feat(config): require API_TOKEN
```
