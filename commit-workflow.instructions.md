---
description: "Shared commit and push workflow. Covers build verification, staging, conventional commit message generation, Husky hook handling, and pushing to remote."
---

## Commit & Push Workflow

### Step 1 — Verify Build

Run `npm run build`.

- If it **passes**, continue to Step 2.
- If it **fails**, read the error output carefully. Offer to fix the errors. After fixing, re-run `npm run build`. Only continue once the build passes. Do not proceed with a failing build.

### Step 2 — Understand the Changes

Run the following and read all output before saying anything to the user:

```
git status
git diff HEAD
```

Silently analyse what was added, modified, or deleted and understand the intent of the changes. Then show the user a brief plain-English summary of what changed (2-4 sentences max).

Ask: **"Stage all of these? (yes / no / list specific files)"**

Wait for confirmation before continuing.

### Step 3 — Stage & Propose Commit Message

Stage the confirmed files.

Analyse the diff and propose a short conventional commit message.

Rules:
- Type must be exactly one of: `feat`, `fix`, `chore`, `refactor`
- Format: `<type>: <short description>`
- Description must be lowercase, present tense, no full stop
- Keep it short and specific

Examples of good commit messages:
- `feat: add refresh token rotation`
- `fix: resolve duplicate record on create`
- `chore: update express to v5`
- `refactor: extract password hashing to helper`

Ask: **"Commit with this message? (yes / edit)"**

If the user says edit, accept their revised message and use it exactly.

### Step 4 — Commit & Handle Hooks

Run `git commit -m "<message>"`.

Wait for the **full output** including any Husky or lint-staged hooks.

- If hooks **pass** → continue to Step 5.
- If hooks **fail due to lint errors** → read the errors carefully, fix the affected files, re-stage the fixed files, and retry the commit. Retry up to 2 times. If it still fails after 2 retries, stop and report exactly what went wrong.

### Step 5 — Push

Ask: **"Push to remote? (yes / no)"**

If yes: get the current branch by running `git branch --show-current`, then run `git push origin <current-branch>`.
