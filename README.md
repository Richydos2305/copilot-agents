# copilot-agents

A personal collection of AI coding agents, prompts, and shared instructions, built around the [PIV Loop](https://github.com/dynamous-community/agentic-coding-course) (Plan → Implement → Validate) methodology — maintained in two parallel forms for two different tools.

## Layout

| Folder | For | Details |
|---|---|---|
| [`editors/`](editors/) | VS Code Copilot (and similar IDE agents) | `agents/<name>/agent.md`, `prompts/<name>/prompt.md`, `shared/<name>.md` — see [editors/README.md](editors/README.md) |
| [`claude/`](claude/) | Claude Code | `skills/<name>/SKILL.md`, `shared/<name>.md` — see [claude/README.md](claude/README.md) |

`claude/` is a git-tracked reference copy. The skills actually running in this Claude Code install live at `claude-code/` (gitignored, symlinked into `~/.claude/`) — see [claude/README.md](claude/README.md) for how the two relate.

## Agents & Skills

Each of these exists in both forms — same behaviour, ported across the two conventions.

| Name | Purpose | `editors/` | `claude/` |
|---|---|---|---|
| Backend Dev | Senior backend developer persona. Plans before implementing, never touches frontend code or tests. | `agents/backend-dev/agent.md` | `skills/backend-dev/SKILL.md` |
| Frontend Dev | Senior frontend developer persona (React 19+/TypeScript). Plans before implementing, never touches backend code or tests. | `agents/frontend-dev/agent.md` | `skills/frontend-dev/SKILL.md` |
| GitHub Expert | Git and GitHub CLI operator — branching, rebasing, conflicts, `gh pr create`. Never force-pushes without `--force-with-lease`. | `agents/github/agent.md` | `skills/github-expert/SKILL.md` |
| Design | Pencil MCP-powered product designer. Plan → generate → validate → iterate, exports to HTML/CSS or hands off to Frontend Dev. | `agents/design/agent.md` | `skills/design/SKILL.md` |
| LinkedIn | Thought logger and post generator — three tones, no corporate speak. | `agents/linkedin/agent.md` | `skills/linkedin/SKILL.md` |
| Code Review | Beginner-friendly PR reviewer — explains changes section by section, never applies fixes, ends with Approve/Request Changes. | `agents/code-review/agent.md` | `skills/code-review-explain/SKILL.md` |
| Test Writer | Senior test engineer — proposes test cases for approval before writing any test code. | `agents/write-tests/agent.md` | `skills/write-tests-approve/SKILL.md` |
| Commit | Build → diff summary → conventional commit message → push, with hook-retry handling. | `prompts/commit/prompt.md` | `skills/commit/SKILL.md` |
| Bug Reporter / Log Complaint | Logs a structured issue entry (root cause + repeat detection) for a misbehaving agent/skill. Never fixes anything itself. | `agents/bug-reporter/agent.md` | `skills/log-complaint/SKILL.md` |
| Issue Fixer | Works through the issues log one at a time, proposing a minimal fix and waiting for approval before applying it. | `agents/fix-issues/agent.md` | `skills/fix-issues/SKILL.md` |

## Conventional Commits Reference

Commit types used across these agents: `feat`, `fix`, `chore`, `refactor`.

Format: `<type>: <short description>` — no scope brackets.

```
feat: add refresh token rotation
fix: resolve duplicate record on create
chore: update dependencies
refactor: extract validation into middleware
```
