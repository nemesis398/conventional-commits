---
max_turns: 6
allowed_tools: [Skill]
---

Write the commit message for my staged change. You cannot run git here, so
here is what you would see.

`git log --no-merges -4 --format=%B`:

```text
fix(auth): keep the session after a password change

English:
Changing the password no longer signs the user out of every device.

---

fix(auth): die Sitzung nach einer Passwortänderung behalten

Deutsch:
Eine Passwortänderung meldet den Nutzer nicht mehr auf jedem Gerät ab.

feat(faq): add a search field

English:
Visitors can filter the questions by a word.

---

feat(faq): ein Suchfeld hinzufügen

Deutsch:
Besucher können die Fragen nach einem Wort filtern.
```

`git diff --cached`:

```diff
--- a/src/faq/list.ts
+++ b/src/faq/list.ts
@@ -12,7 +12,7 @@ export function filter(items: Item[], word: string): Item[] {
-  return items.filter((i) => i.question.includes(word));
+  return items.filter((i) => i.question.toLowerCase().includes(word.toLowerCase()));
 }
```
