---
name: commit
description: Build, review the diff, propose a conventional commit message, commit (handling pre-commit hook retries), and push to the current branch. Use when committing changes, staging files, creating a git commit, or pushing code.
---

Read `/Users/richard/.claude/shared/commit-workflow.md` and follow it step by step.

If the user provided additional context about the changes (e.g. `/commit auth changes`), use it to inform the proposed commit message.

## Complaint Logging

If the user says anything like "log a complaint", "report a bug", "this isn't working", or "something is wrong with you" — invoke the `log-complaint` skill via the Skill tool, passing:

- **skill**: `commit`
- **complaint**: the user's description of what went wrong

Show the user the `log-complaint` output before continuing.
