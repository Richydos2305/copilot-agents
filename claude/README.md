# claude

Claude Code [Skill](https://docs.claude.com/en/docs/claude-code/skills) ports of the GitHub Copilot agents/prompts/instructions in [`editors/`](../editors/).

> **This folder is a git-tracked reference copy, not the live source.** The skills actually running in this Claude Code install live at `../claude-code/` (gitignored, symlinked into `~/.claude/` by `claude-code/sync.sh`). This folder mirrors that content reorganized into the `skills/` + `shared/` structure used across this repo, so it's easier to review and diff in git. If you want this structure to become the live one, point `sync.sh` at `claude/` instead and update its `shared-instructions` references to `shared`.

---

## Structure

```
claude/
  skills/<name>/SKILL.md   — one skill per folder, discovered by Claude Code as /<name>
  shared/<name>.md         — reference content loaded on demand via absolute path
```

---

## Skills

| Slash command | Ported from | Notes |
|---|---|---|
| `/backend-dev` | `editors/agents/backend-dev/agent.md` | Planning-first senior backend dev. Never implements without approval. |
| `/frontend-dev` | `editors/agents/frontend-dev/agent.md` | Planning-first senior frontend dev (React 19+, TypeScript). |
| `/github-expert` | `editors/agents/github/agent.md` | Git/GitHub CLI specialist — branching, rebasing, conflicts, PR creation. |
| `/design` | `editors/agents/design/agent.md` | Pencil MCP-powered design persona. **Requires Pencil MCP configured for Claude Code** (see https://www.pencil.new) — will not function without it. |
| `/linkedin` | `editors/agents/linkedin/agent.md` | Thought logger and LinkedIn post generator. |
| `/write-tests-approve` | `editors/agents/write-tests/agent.md` | Test writer with an approval gate before any test code is written. |
| `/code-review-explain` | `editors/agents/code-review/agent.md` | Junior-friendly, explanation-heavy PR reviewer. Output-only, never applies fixes. Distinct from the built-in `/code-review`. |
| `/commit` | `editors/prompts/commit/prompt.md` | Build → diff → conventional commit → push, with hook-retry handling. |
| `/log-complaint` | `editors/agents/bug-reporter/agent.md` | Logs a structured issue about a misbehaving skill to `agent-issues.md`. |
| `/fix-issues` | `editors/agents/fix-issues/agent.md` | Works through `agent-issues.md` one issue at a time, with approval before each fix. |

`backend-dev`, `frontend-dev`, `github-expert`, `design`, `linkedin`, `write-tests-approve`, and `code-review-explain` all share a **Complaint Logging** section — say "log a complaint" (or similar) while using any of them and they'll invoke `/log-complaint` for you.

---

## Shared instructions (`shared/`)

Referenced by absolute path (`/Users/richard/.claude/shared/<name>.md`) from the skills above, mirroring `editors/shared/`:

- `dev-workflow.md` — the 4-phase planning/implementation workflow used by `backend-dev` and `frontend-dev`.
- `commit-workflow.md` — build → diff → stage → commit → push, used by `commit`, `backend-dev`, and `frontend-dev`.
- `pr-description.md` — generates a PR description from a diff using the WHY/WHAT/HOW/THINGS-TO-NOTE template.
- `raise-pr.md` — title derivation, base branch confirmation, and `gh pr create`, used by `github-expert`.

---

## Runtime files

Created automatically on first use, global (not per-project), not part of this repo:

| File | Created by | Purpose |
|---|---|---|
| `~/.claude/agent-issues.md` | `log-complaint` | Open/Resolved issue log for all skills. Worked through by `fix-issues`. |
| `~/.claude/linkedin-log.md` | `linkedin` | Thoughts queue and post drafts. |

`pending-changes.md` (used by `shared/dev-workflow.md` Phase 4) stays **per-project** — created in `.github/pending-changes.md` or the project root, same as the original Copilot setup.

---

## Notes

- Unlike Copilot's agent picker, Claude Code has one persistent assistant — invoking a skill loads its instructions into the current conversation rather than switching to a separate persona. Cross-skill handoffs (e.g. `backend-dev` → `github-expert`) happen via the Skill tool, in the same conversation.
- `backend-dev`/`frontend-dev` replicate their approval gates conversationally rather than relying on Claude Code's native Plan Mode — the per-task approval and `pending-changes.md` model is more granular than Plan Mode's all-or-nothing approval. The two are complementary if Plan Mode happens to be active.
