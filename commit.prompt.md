---
description: "Build, stage, commit with conventional message, and push to GitHub. Use when: committing changes, pushing code, creating a git commit, staging files"
argument-hint: "Optional context hint e.g. 'auth changes'"
agent: "agent"
tools: [execute, read, agent, edit, search, 'gitkraken/*']
---

Follow the commit and push workflow defined in [commit-workflow.instructions.md](./commit-workflow.instructions.md).

If the user says anything like "log a complaint", "report a bug", "this isn't working", or "something is wrong with you" — call the `Bug Reporter` agent, passing **filename**: `commit.prompt.md` and the user's complaint. Show the Bug Reporter's output.
