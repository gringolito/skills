---
name: dispatch-issue
description: Take a ready-for-agent issue from open to a reviewed pull request. Use when the user wants a ready-for-agent issue picked, developed, shipped, and reviewed, or says things like "dispatch the next one", "ship a ready issue", or "work the queue".
argument-hint: "Optional: the issue number to dispatch, or leave this blank to auto-pick."
---

Take a single ready-for-agent issue from open to a reviewed pull request without pausing for user sign-off.

When no issue number is provided, select the highest-ranked eligible issue that is open, labeled `ready-for-agent`, and unassigned. Do not select an issue with an open `blocked_by` dependency. If an issue number is provided, inspect it. If it has sub-issues, use the sub-issue list as picking candidates and select the topmost unblocked sub-issue that is open, labeled `ready-for-agent`, and unassigned. Otherwise, use that issue, and stop and report why if it is assigned, blocked, or otherwise ineligible. If no eligible issue can be found, stop and report that.

Self-assign the issue.

Read the issue, including all comments, and, if it has a parent, read enough of the parent and its comments to understand the context the issue depends on.

Delegate implementation to a new sub-agent, passing it the issue, relevant parent context, and the `tdd` skill. Have the sub-agent follow TDD, keep the changes scoped to the issue, and always run the tests, linters, and code formatting tools locally before considering the implementation complete. Have it return once the implementation and local verification are complete.

Before opening the pull request, run `unslop` over the changes and apply the `make-pr-easy-to-review` skill so the resulting diff and PR are concise and reviewable. Then use `open-pr` to branch, commit, push, and open the pull request, including `Closes #<N>`.

After every push, monitor the pull request checks until they are green or the CI retry limit is reached. If a failure is caused by this PR, delegate back to the implementation sub-agent to perform the necessary fix, push it, and continue monitoring. Do not modify pre-existing or environmental failures, and never resolve a failure by weakening or deleting a test. Allow at most three consecutive push rounds; if CI still does not converge, stop and report the failing checks and relevant logs.

Once CI is green, create a reviewer sub-agent and have it run the `code-review` skill against the pull request. The reviewer should post its findings directly to the pull request as a single review, with inline comments for findings that can be tied to a diff line and body-only findings for architectural or cross-cutting concerns. Stop after the review has been posted. Do not address or delegate fixes for review findings.
