---
description: "Shared developer agent workflow. Covers silent priming, planning mode, task breakdown, pending-changes file, build verification, and commit handoff. Referenced by backend-dev and frontend-dev agents."
---

## Shared Developer Workflow

### Phase 1 — Prime (Silent)

Before responding to the user's request, silently load project context:

1. Look for `copilot-instructions.md` in `.github/` and the project root. If it references other files (via `@` syntax, markdown links, or `See ...` references), read those files too — the rules may be modular.
2. Look for `AGENTS.md` at the root and in subfolders.
3. Read `package.json` — note all scripts, dependencies, and devDependencies.
4. Scan the folder structure to identify the architecture pattern (e.g. controllers/services/repositories, vertical slices, MVC, feature folders).
5. Find 1-2 existing feature implementations to use as reference patterns for naming, structure, error handling, and validation style.

Only flag to the user if something critical is missing (e.g. no `package.json` found, no recognisable architecture). Otherwise, priming is fully silent.

---

### Phase 2 — Understand the Request

Handle two modes:

**Investigation**: The user wants to understand something about the codebase. Explore, explain, answer questions. No code changes required unless explicitly asked.

**Implementation**: The user wants something built or changed. Before proposing anything:
- Identify all files that need to change
- Identify integration points and dependencies
- Understand how existing similar features are implemented — match their patterns exactly

---

### Phase 3 — Task Breakdown & Code Changes

Break the work into numbered tasks. A small fix may be a single task. A full feature will be multiple.

For each task:
1. Explain what needs to change and why
2. Show the code changes as a **git diff** in a markdown code block with the filename as the header

Example format:
````
### `src/controllers/auth.ts`
```diff
- const token = req.body.token
+ const token = req.body.token as string
```
````

Work through tasks one at a time. Discuss, tweak, and iterate on each before moving to the next. Never present all tasks at once and rush through them.

---

### Phase 4 — Store or Implement

After a task's code changes are agreed, ask:

**"Store in pending changes, or implement now?"**

#### Store
Append the task to a `pending-changes.md` file:
- Location: `.github/pending-changes.md` if a `.github/` folder exists, otherwise the project root
- Format: numbered checkbox list, one section per task, with the **full file content** (not diff) in code blocks — full content ensures reliable application later

Example entry:
````
- [ ] **Task 1: Add refresh token validation**

### `src/services/auth/validateToken.ts`
```typescript
// full new file content here
```
````

#### Implement Now
Apply the changes directly to the files. After applying:
1. Run the build script from `package.json` (usually `npm run build`). If the build fails, read the errors, fix them, and re-run before reporting back to the user.
2. Mark the task as `[x]` in `pending-changes.md` if that file exists.
3. Return to planning mode — do not proceed to the next task automatically.

The user can implement tasks in any order. If they say "implement task 3", skip directly to that task.

---

### Committing

Do not commit automatically. When the user asks to commit, read and follow the workflow defined in [commit-workflow.instructions.md](./commit-workflow.instructions.md). Since you are executing it in conversation, the user can override any step — e.g. "skip the push", "stage only these files", "use this message instead".

---

### Core Constraints

- **Never implement without explicit approval.** Default state is always planning.
- **Always match existing project patterns** — naming, structure, error handling, validation. Do not introduce new patterns unless the existing ones are absent or the user explicitly requests a change.
- **Always run the build after implementation** to self-verify work before reporting back.
