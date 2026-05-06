# copilot-agents

A personal collection of VS Code Copilot agents, prompts, and shared instructions built around the [PIV Loop](https://github.com/dynamous-community/agentic-coding-course) (Plan → Implement → Validate) methodology.

These files live in your VS Code user-level prompts folder and are available across all projects.

---

## Setup

Copy all files from this repo into your VS Code user prompts folder:

```bash
cp -r ./*.md "/Users/$(whoami)/Library/Application Support/Code/User/prompts/"
```

> **macOS only.** On Windows the path is `%APPDATA%\Code\User\prompts\`. On Linux: `~/.config/Code/User/prompts/`.

After copying, restart VS Code or open the Copilot Chat panel — agents will appear in the agent picker and prompts will be available as slash commands.

---

## VS Code Copilot Primitives

| Type | Extension | How it works |
|------|-----------|-------------|
| **Agent** | `.agent.md` | Persistent persona with a name, tool restrictions, and custom instructions. Selected from the agent picker in Copilot Chat. |
| **Prompt** | `.prompt.md` | Single-purpose slash command (e.g. `/commit`). Runs once per invocation. |
| **Instructions** | `.instructions.md` | Shared reference content. No `applyTo` = loaded on demand via markdown links from agents/prompts. Single source of truth. |

---

## Agents

### Backend Dev (`backend-dev.agent.md`)
Senior backend developer agent. **Planning mode by default** — never implements without explicit approval. Primes itself silently on the codebase before responding. Covers controllers, services, repositories, models, middleware, routes, validation, and error handling. Never writes frontend code or tests.

### Frontend Dev (`frontend-dev.agent.md`)
Senior frontend developer agent. Same planning-first workflow as Backend Dev. React expertise including hooks, React 19+ features (`use()`, `useOptimistic`, `useActionState`, Actions API), TypeScript, state management, data fetching, and accessibility. Never writes backend code or tests.

### GitHub Expert (`github.agent.md`)
Git operations and GitHub CLI agent. Covers branching, merging, rebasing, cherry-picking, stash, conflict resolution, `git log`/`blame`/`bisect`/`reflog`, and `gh` CLI. Safety rules: always explains destructive commands before running, never uses `--force` (always `--force-with-lease`).

### Design (`design.agent.md`)
UI design agent powered by [Pencil MCP](https://www.pencil.new). Can sample an existing React project (up to 2 screens) to infer your design system before starting. Works through a structured brief → plan → generate → validate → export flow. Exports to self-contained HTML/CSS.

### LinkedIn (`linkedin.agent.md`)
Thought logger and LinkedIn post generator. Maintains a persistent log of developer thoughts and drafts posts in three tones: Storytelling, Insight/Lesson, and Hot Take. No corporate speak, no excessive emojis.

### Bug Reporter (`bug-reporter.agent.md`)
Called automatically by other agents when you log a complaint. Reads the agent's own file, diagnoses the root cause, checks for repeat issues, and appends a structured entry to `agent-issues.md`. Returns a brief report to the calling agent.

### Issue Fixer (`fix-issues.agent.md`)
Works through open entries in `agent-issues.md` one at a time. Proposes a precise fix for each issue, waits for your approval, applies it, and moves the issue to Resolved with repeat-issue detection.

---

## Prompts (Slash Commands)

### `/commit` (`commit.prompt.md`)
Full commit and push workflow: builds the project, shows a diff summary, proposes a conventional commit message, handles Husky hooks, and pushes to the current branch.

### `/write-tests` (`write-tests.prompt.md`)
Writes unit tests for a given file or function. Primes on the existing test framework and style, lists test cases for your approval before writing any code, then produces a complete test file.

---

## Shared Instructions

### `dev-workflow.instructions.md`
The shared developer agent workflow used by Backend Dev and Frontend Dev. Covers: silent priming, planning vs implementation modes, numbered task breakdown, pending-changes file format, build verification, and commit handoff.

### `commit-workflow.instructions.md`
The shared commit and push workflow used by the `/commit` prompt. Covers: build verification, git diff summary, staging, conventional commit message generation, Husky hook handling, and pushing to remote.

---

## Complaint Logging

Every agent and prompt includes a **Complaint Logging** section. If something isn't working right, just say:

> "Log a complaint — it implemented without asking."

The current agent will call the **Bug Reporter**, which reads its own file, identifies the root cause, and logs it to `agent-issues.md`. When you're ready to fix things, open the **Issue Fixer** agent.

---

## Runtime Data Files

These files are created automatically and live locally only (gitignored):

| File | Created by | Purpose |
|------|-----------|---------|
| `agent-issues.md` | Bug Reporter | Tracks open and resolved agent issues |
| `linkedin-log.md` | LinkedIn agent | Stores thought queue and post drafts |

---

## Conventional Commits Reference

Commit types used across these agents: `feat`, `fix`, `chore`, `refactor`.

Format: `<type>: <short description>` — no scope brackets.

```
feat: add refresh token rotation
fix: resolve duplicate record on create
chore: update dependencies
refactor: extract validation into middleware
```
