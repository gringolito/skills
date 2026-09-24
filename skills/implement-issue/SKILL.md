---
name: implement-issue
description: >-
  Implement a single issue end-to-end, from code changes through pull request, CI, and code
  review, until the pull request is merged. Use when the user wants one ready-for-agent issue
  picked and implemented, or says things like "dispatch the next one", "implement a ready
  issue", or "work the queue". To implement every sub-issue of a specification, use
  `implement-spec`.
argument-hint: "Optional: the issue number to implement, or leave this blank to auto-pick."
---

Take a single issue from open to a merged pull request without pausing for user sign-off.

When no issue number is provided, select the highest-ranked issue that is open, labeled
`ready-for-agent`, unassigned, and has no open `blocked_by` dependency. If an issue number is
provided and the issue has sub-issues, select the topmost of those sub-issues that meets the
same criteria. Otherwise, use the provided issue, and stop and report why if it is closed,
assigned, or blocked. If no eligible issue can be found, stop and report that.

Self-assign the issue.

Read the issue and all its comments. If it has a parent, read enough of the parent and its
comments to understand the context the issue depends on.

Run the implementation through a sub-agent. Give it the issue, the relevant parts of the
parent issue, and the `tdd` skill. The sub-agent must follows TDD, changes only what the issue
requires, and runs the tests, linters, and formatters locally before it reports the work as
done.

When the implementation is done, open a pull request that is clear, concise, and easy to
review, and that includes `Closes #<N>`.

Then see the pull request through until it is merged. After every push, check CI and fix any
failures your changes caused. If three consecutive pushes don't turn CI green, stop and report
it as a blocker. Leave pre-existing and environmental failures alone, and never make CI pass
by weakening or deleting a test.

Once CI is green, have a new reviewer sub-agent run the `code-review` skill on the pull
request and post its findings there, as inline comments on the relevant lines of the diff
wherever a finding points to specific code. Address every review comment and reply to each one
with the fix, the solution, or the decision you made. Then run the relevant tests, linters,
and formatters locally, push the fixes, and get CI green again.

Keep watching the pull request for new comments and further review rounds, and repeat the
same cycle for each one, until the pull request is merged.

Always add AI attribution to every commit, pull request, review comment, and reply, including
those written by sub-agents.

Never merge the pull request without the user's explicit consent.

Stop early and report a blocker when the issue and its context aren't enough to implement it,
when a decision is needed that they don't answer, or when a failure blocks progress and can't
be fixed safely.
