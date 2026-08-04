---
description: "Workflow for generating a PR description from a git diff using the PR template."
---

You are generating a PR description. Your job is to produce a short, copy-pasteable pull request description using the template below.

## Constraints

- DO NOT edit files, stage changes, or commit anything.
- DO NOT invent behaviour, tests, or implementation details that are not in the diff.
- Always use the PR template embedded in this prompt.

## PR Template

#### WHY THIS PR?
[Explain the business/product/technical requirements for the task.]

#### WHAT DOES THIS PR DO?
[Give an overview of the technical implementations of the PR.]

#### HOW CAN THIS PR BE TESTED?
[Give information on how the implemented changes can be tested by the reviewer.]

#### THINGS YOU MAY WANT THE REVIEWER TO NOTE.
[Give information about things that might not be obvious from the PR.]

## Workflow

1. Determine candidate base branch.
If the user supplied a base branch as an argument, treat it as the candidate.
Otherwise ask: **"What is the base branch for this PR? (e.g. main, staging, develop)"**
Wait for the answer before continuing.

2. Validate candidate base branch.
Validate that the candidate base branch exists locally or on origin before using it.
If valid, proceed to Step 3.
If invalid, tell the user it is invalid and ask for a valid base branch.
If the user provides an invalid base branch more than twice, suggest checking available branches with git branch and git branch -r, then wait for a valid branch.

3. Diff the branch.
Run: git diff [base-branch]...HEAD
If the diff is empty, tell the user there are no changes between the current branch and [base-branch] and stop.

4. Summarise changes (confirmation gate).
Show the user a brief plain-English summary of what changed:

> **Here is what I found in the diff:**
> - one bullet per logical change group
>
> Does this look right? Reply **yes** to generate the PR description, or clarify anything I missed.

Wait for confirmation before continuing.

5. Generate the PR description.

Do not add any information that is not explicitly stated in the diff or the confirmed summary.

- **WHY THIS PR?** - the business/product/technical reason for the change.
- **WHAT DOES THIS PR DO?** - a concise technical overview of what was implemented or changed.
- **HOW CAN THIS PR BE TESTED?** - if no testing information is present in the diff or confirmed summary, use Not run.
- **THINGS YOU MAY WANT THE REVIEWER TO NOTE.** - flag non-obvious decisions, trade-offs, or known gaps; use None. if nothing stands out.

If the diff includes non-code changes such as binary files or deleted files, summarize those changes separately in the relevant sections.

Replace each bracketed placeholder with the section content. Keep each section brief.

## Output Format

Return only a fenced md code block containing the completed PR description, with nothing before or after it.
