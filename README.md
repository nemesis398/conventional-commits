# conventional-commits

A [Claude Code](https://claude.com/claude-code) skill that writes and reviews
Git commit messages in [Conventional Commits
1.0.0](https://www.conventionalcommits.org/en/v1.0.0/), shaped by the history
of the repository you are standing in.

Most commit helpers know the spec. This one reads `git log` first, because two
repositories rarely agree: which scopes they use, whether they carry trailers,
and whether every commit is written in English and German. Then it reads the
diff and checks what you tell it against it. A message that claims a breaking
change the diff does not contain, or a dependency bump that is not staged, gets
corrected instead of polished.

## What it does

- **Writes a message** for the staged change: type, scope from the repo's own
  vocabulary, an imperative subject of at most 72 characters, a body when the
  reason is not obvious, and footers in trailer format.
- **Reviews a message** someone wrote: a verdict (`OK`, `minor`, or `rewrite
  needed`), one line per broken rule, and a rewrite.
- **Flags breaking changes** the way parsers read them: `!` after the type and
  a `BREAKING CHANGE:` footer, never `BREAKING:`.
- **Follows a bilingual history.** When most recent commits carry an
  `English:` and a `Deutsch:` half, it writes both, with the same type, scope
  and `!` in each, and the footers once at the end.
- **Splits** a change that fits two types into two commits.

It owns the shape of the message. The prose inside it is yours, or your
style guide's.

## Install

In Claude Code, add the repository as a marketplace and install the plugin:

```text
/plugin marketplace add nemesis398/conventional-commits
/plugin install conventional-commits@conventional-commits
```

Without the plugin system, copy `skills/conventional-commits/` into
`~/.claude/skills/`. The skill is one Markdown file and needs nothing else.

## Use

Ask in your own words. The skill fires on requests such as:

- "write the commit message"
- "clean up this commit message: …"
- "review my last commit"
- "squash these into one message for the PR"

## Tests

`evals/` holds cases for `claude plugin eval`: a bilingual history, an
English-only one, a message whose claims the diff contradicts, and a message
full of noise. Each case gives the history and the diff in its prompt, so it
needs no repository. They run against a model, on your own account:

```sh
claude plugin eval . --runs 3
```

CI checks the manifest, the Markdown, and that no em dash slipped in.

## Where it comes from

It is a leaf of [ravnix](https://github.com/nemesis398/ravnix): one of the
projects that grew inside that NixOS flake and now stands on its own, so it can
be used without the rest.

## License

[MIT](LICENSE)
