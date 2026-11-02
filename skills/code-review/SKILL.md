---
name: code-review
description: >-
  Review a change against the project's standards and its spec, and check that its tests would
  catch regressions. Use when an agent needs independent feedback on work in progress, such as
  before opening a pull request, or when the user wants a change reviewed locally. To post the
  findings on a pull request, use `review-pr`.
argument-hint: "Optional: the change to review and the issue or spec it implements"
---

Review the change on three axes, each defined in its own reference:

- [Standards](standards-axis.md): does the code follow the project's conventions and best
  practices?
- [Spec](spec-axis.md): does the code do what the originating issue or spec asked for?
- [Tests](tests-axis.md): would the tests fail if the changed behavior were wrong?

The change is whatever the caller names: a branch, a pull request, or the uncommitted work in
progress. When nothing is named, review the current branch against its base.

Run each axis in its own `reviewer` agent, in parallel, so no axis pollutes another's context and
none shares the author's. Pass each reviewer its axis reference as written, with any reference it
links and the reporting rules below, instead of summarizing them, so every reviewer applies the
same definition. You don't need to read the axis references yourself.

Each reviewer opens its report with its exact model ID, copied from its system prompt or runtime
configuration. If neither names a model, it says so rather than guessing. Keep each axis summary
under 200 words. Keep individual findings concise without omitting evidence needed to act on them.

Each finding states the problem, why it matters, and a concrete remedy, with its file and line
when applicable. Distinguish demonstrated problems from uncertain concerns.

Return the findings to the caller grouped by axis, each axis with the model that reviewed it.
Don't merge or rerank findings across the axes, so one never masks another. Publish nothing:
the review leaves no trace on the pull request, the issue, or the commit history.

