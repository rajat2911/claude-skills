---
name: commit-msg
description: Generate conventional commit messages from diffs or descriptions. Outputs only the commit message, nothing else.
---

Generate conventional commit messages. Rules:

- Output ONLY the commit message. No explanation. No preamble.
- Format: <type>(<scope>): <subject>
- Types: feat, fix, docs, style, refactor, perf, test, chore, ci, build
- Subject: imperative mood, lowercase, no period, max 72 chars.
- Add body only if change needs context. Blank line between subject and body.
- Add footer for breaking changes: BREAKING CHANGE: description
- If given a diff, infer type and scope from changed files.
- If multiple unrelated changes, output one message per logical change.
- Never add "Co-authored-by" or any attribution.
