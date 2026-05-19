---
description: "Raise a GitHub pull request via the CLI."
argument-hint: "Optional arguments: base branch, PR description, and manual PR title."
agent: "agent"
tools: [vscode/askQuestions, execute, read, agent]
---

You are raising a GitHub pull request. You will derive the PR title from the branch name, confirm the base branch, and use `gh pr create` to open the PR.

## Constraints

- Do not change repository files or git history while preparing the PR. You may still ask for and use PR metadata such as the title, body, and base branch.
- Running gh pr create is allowed.
- Always explicitly ask the user to confirm the base branch, regardless of whether it was inferred or provided.
- Always check `gh` is available before proceeding. If not installed, tell the user: `brew install gh && gh auth login`.
- If the user does not confirm the base branch or the final create command, stop and provide a short summary of what was completed.
- If the user does not respond to a confirmation prompt, stop and wait for a reply. Do not assume confirmation.

## Workflow

Complete the steps in order. Do not continue to the next step until the current step succeeds or the user confirms it.

### Step 1 - Get context (silent)

Run these two commands:

- `git branch --show-current` — gives the current branch name.
- `gh api user --jq .login` — gives the authenticated GitHub username, used as the PR assignee.
If either command fails, inform the user and ask them to verify their Git and GitHub setup before continuing.

### Step 2 - Derive the PR title

Convert the branch name to a title using this rule:
- Split on `-`
- The first segment is the ticket ID - keep it uppercase followed by a colon: e.g. `PCM-820`
- Remaining segments are title-cased and joined with spaces
- Example: `PCM-820-fix-fawry-url-pattern` -> `PCM-820: Fix Fawry Url Pattern`
- If the branch name does not match this pattern, ask the user to provide the PR title as text.

### Step 3 - Confirm the base branch

If the user supplied a base branch, show it and ask for confirmation:
> **Base branch will be set to `<branch>`. Is that correct?**

If not supplied, determine the base branch by checking the default branch of the remote repository using `git remote show origin`, then ask:
> **I will target `<inferred-branch>` as the base. Confirm, or tell me the correct branch.**

Wait for confirmation before continuing.

### Step 4 - Get or generate the PR body

If the user provided a PR description (passed as an argument or pasted in), use it directly.

If no description was provided, call the `PR Description` agent to generate one by gathering any needed user input and repository context. Include the confirmed base branch as an explicit argument in that agent request. Use the output as the PR body.

### Step 5 - Raise the PR

Run `gh pr create` with:
- `--title` - the derived title from Step 2
- `--body` - the PR description from Step 4
- `--base` - the confirmed base branch from Step 3
- `--assignee` - the GitHub username from Step 1

Show the user the exact command before running it and wait for a final **yes** confirmation.

If `gh pr create` fails, inform the user of the error and suggest retrying or checking their network connection and GitHub authentication.

After the PR is created, display the PR URL returned by `gh`.
