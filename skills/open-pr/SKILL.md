---
name: open-pr
description: Inspect the current changes, create a branch, commit the work, and open a pull request. Use when the user wants to branch off, commit and open a PR, ship the current changes, or says things like "cut a branch", "push this up", or "make a PR".
---

Inspect the current working tree by verifying:

!`git status`
!`git diff`
!`git diff --staged`

Review every modified file and determine whether it belongs in this commit. Classify each file as either in scope or out of scope, and if the scope is ambiguous, discuss it with me before proceeding. Do not continue until every modified file has been accounted for.

Create a branch named `<type>/<short-description>` (for example `feat/user-auth` or `fix/null-pointer`), using the current branch as the base unless I specify otherwise.

Stage only the in-scope files and create a signed, signed-off commit using `git commit -S -s`. Follow the Conventional Commits specification for the subject line, keeping it imperative and no more than 72 characters. The subject should explain what changed, while the commit body should explain why the change exists and include any non-obvious rationale. When the work relates to a GitHub issue, do not include the issue number in the subject line; instead, add `Refs #<N>` as a footer in the commit body.

Push the branch and open a pull request using the GitHub CLI. Write a concise PR title and a body containing a **Summary** and **Why** sections followed by `Closes #<N>` when applicable. Do not include a Test Plan section.

Finally, return the URL of the newly created pull request after confirming it was created successfully.
