---
name: conventional-commits
description: >-
  Generate and review Git commit messages that follow the Conventional Commits
  1.0.0 specification and match the target repository's own conventions,
  including a bilingual English-first, German-second shape, which this skill
  detects from history rather than assuming. Reach for this skill whenever a
  commit message is being drafted, rewritten, or reviewed: when the user pastes
  a message and asks for cleanup, when a staged or unstaged diff needs to be
  summarized, or when a pull-request title and squash message need to be
  normalized. Produces subject, optional body and optional footers, flags
  breaking changes, scopes and trailers, and checks the message's factual
  claims against the actual diff before accepting them.
---

# Conventional Commits

A sparring partner for writing, reviewing, and rewriting Git commit messages
against **[Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)**
and, just as importantly, against the conventions of the repository you are
actually standing in.

## Step 0: read the repo before writing anything

This skill is installed globally, so it has no "this repo." Every repository has
its own convention, and the only reliable source is its history. Two
repositories of the same team can disagree: one writes every non-merge commit
in English and German, the other in English only. Guessing gets one of them
wrong every time.

```bash
git log --oneline -20                       # subject style, types, scope vocabulary
git log --no-merges -20 --format=%B         # bodies, footers, trailers, language
```

Decide three things from that output, not from habit:

1. **Language shape.** Count the bilingual halves:

   ```bash
   git log --no-merges -20 --format=%B | grep -cx 'Deutsch:'
   ```

   If most of the last 20 non-merge commits are bilingual, write bilingual.
   Near zero means monolingual English. Genuinely mixed: say so and ask; don't pick
   silently.
2. **Scope vocabulary.** Use scopes the repo already uses. A wrong scope is
   worse than no scope.
3. **Trailers.** If no commit carries `Co-authored-by:` or `Refs:`, don't add
   one: it breaks the uniformity of the history. If they all do, match them.

**Merge commits are left alone.** In bilingual repos they stay monolingual
(`Merge branch '…'`); that is not a violation to fix.

## Step 1: read the diff, and check the user's claims against it

```bash
git status && git diff --cached          # plus `git diff` if something is unstaged
```

Never describe a change you have not read. And when the user *tells* you what
the commit does, above all in a message they ask you to clean up, verify each
claim before carrying it into the rewrite:

- *"also bumped a couple of deps"*: is a manifest actually staged?
  `git diff --cached --name-only`.
- *"BREAKING: `FOO` is now required"*: is that line **added**, or is it context
  that has been there since an earlier commit? `git log -S'FOO' --oneline`
  settles it.

A commit message that claims a breaking change which is not in the diff sends
everyone hunting for a migration that does not exist, and wrongly bumps a major
version if anything parses your history. Say plainly what the diff does and does
not contain, then write the message for the real change. This check is worth
more than every formatting rule below.

## Spec format

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

| Type | SemVer | Use for |
| --- | --- | --- |
| `feat` | MINOR | New user-facing feature or capability. |
| `fix` | PATCH | Bug fix. |
| `docs` | none | Documentation only. |
| `style` | none | Formatting or whitespace; no semantic change. |
| `refactor` | none | Fixes nothing, adds no feature. |
| `perf` | PATCH | Performance improvement. |
| `test` | none | Tests added or fixed. |
| `build` | none | Build system, dependencies, package metadata. |
| `ci` | none | CI configuration. |
| `chore` | none | Tooling, configs, maintenance. |
| `revert` | varies | Reverts earlier work; reference the SHA in `Refs:`. |

The spec mandates `feat` and `fix`; the rest come from the Angular convention.
Don't invent new types. **If a diff fits two types, split the commit**: that is
part of the point.

Where repo history and type purity disagree, history usually wins. If the repo
consistently types by *area* (a genuine bug fix in CI shipped as `ci(deploy)`),
follow it and say why you chose it.

**Subject.** Imperative, no trailing period, at most 72 characters. Describe
the observable effect, not the mechanism. No `WIP`, no `update stuff`. If you
cannot write one line, the commit is doing too much.

**Scope.** A noun in `()` naming the affected area, drawn from the repo's own
vocabulary. Omit it if unclear or mixed.

**Body.** Use it when the *why* is not obvious or a tradeoff was made. Wrap at
about 72.

**Footers.** Git trailer syntax (`Token: value`), one per line, at the very
end. `BREAKING CHANGE:` is reserved and must be uppercase.

**Breaking changes** must be visible to humans *and* parsers: `!` after the
type or scope, a `BREAKING CHANGE:` footer, or both:

```text
feat(api)!: send an email to the customer when a product is shipped

BREAKING CHANGE: customers must now opt in via the new `marketingConsent` flag.
```

`BREAKING:` on its own is **not** the spec token, and nothing will parse it.

## The bilingual shape, when history says the repo uses it

The same content twice in **one** message, English first. Only prose is
translated; `feat`, `fix`, scopes and footer tokens stay English in both
halves.

```text
<type>[scope][!]: <English subject>

English:
<English body, wrapped at about 72>

---

<type>[scope][!]: <German subject: a translation, not a paraphrase>

Deutsch:
<German body, wrapped at about 72>

[footers once, at the very end: BREAKING CHANGE:, Refs:, Co-authored-by:]
```

- The separator is `---` alone on its own line, between the halves only.
- The section labels are spelled exactly `English:` and `Deutsch:`.
- A breaking change gets `!` in **both** subjects and the `BREAKING CHANGE:`
  footer **once** at the end.
- One logical change. Duplication does not turn one commit into two. If the
  diff fits two types, split it, and write both commits bilingually.

> **German capitalization.** German capitalizes every noun, so a German subject
> that begins with one is correctly capitalized. That is orthography, not a
> style violation, and lowercasing it would be wrong. The "lowercase first
> word" rule is about not shouting, not about German grammar. Where you can,
> start the German subject with a verb or an article so the two halves stay
> visually parallel (`den Coolify-Aufruf wiederholen`, `die Unit-Suite
> ausführen`), but never mangle the language to satisfy the rule.

You are not the language authority here. Flag any non-trivial German phrasing
for the user to check.

### Example

```text
feat(faq): add collapsible accordion component

English:
Introduce an accessible accordion for the FAQ page so visitors can scan
questions without scrolling past long answers. Each panel uses a single
button + region pattern (aria-expanded, aria-controls) and supports
keyboard navigation.

---

feat(faq): einklappbare Akkordeon-Komponente hinzufügen

Deutsch:
Ein zugängliches Akkordeon für die FAQ-Seite, damit Besucher:innen Fragen
überfliegen können, ohne an langen Antworten vorbeizuscrollen. Jedes Panel
nutzt das Button+Region-Pattern (aria-expanded, aria-controls) und ist
tastaturbedienbar.

Refs: #142
```

## Reviewing a message someone wrote

Work the list, then rewrite what fails.

**Substance first, because this is where the value is:**

1. Does every claim in the message match the diff? Dependencies actually
   bumped? Breaking change actually breaking?
2. One logical change, or does it need splitting?
3. Does the subject describe the observable effect rather than the mechanism?

**Then the spec:**

1. A recognized type? A scope from the repo's vocabulary?
2. Subject at most 72 characters, imperative, no trailing period?
3. Body separated by a blank line?
4. Breaking change visible through `!`, a `BREAKING CHANGE:` footer, or both,
   and not `BREAKING:`?
5. Footers in trailer format, at the end?
6. No noise (`WIP`, `temp`, `misc`, "while I was in there")?

**Then the repo's own shape**, from Step 0:

1. The right language shape, bilingual or monolingual, matching history?
2. If bilingual: both halves, `---` between them only, labels spelled exactly,
   tokens English in both, `!` in both subjects, footers once at the end?
3. Trailers consistent with what the repo already does?

Verdict: `OK`, `minor` (fixable without asking), or `rewrite needed`. Issues one
per line, each naming the rule it breaks. If a message is correct and merely
informal, leave it alone. Don't rewrite messages that are already correct and
released.

## Output

**Generating:** the message in one code block, ready for `git commit -F-`;
then, only when it is not obvious: the chosen type and scope and why, anything
you found in the diff that the message deliberately does not carry, and which
German sentences want a second pair of eyes.

**Reviewing:** the verdict, the issues one per line citing the rule, then the
proposed rewrite in one code block.

## Where this skill stops

This skill owns the **shape** of a commit message: type, scope, subject line,
breaking-change markers, footers, trailers, and the bilingual layout.

It does not own the **prose inside** it. If the user has a style guide or a
prose skill, the body of a message is subject to it like any other text a
human reads. For anything longer than a subject line, draft the body, then
check it against that guide.
