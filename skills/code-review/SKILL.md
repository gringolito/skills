---
name: code-review
description: >-
  Review a change against the project's standards and its spec, and check that its tests would
  catch regressions. Use when an agent needs independent feedback on work in progress, such as
  before opening a pull request, or when the user wants a change reviewed locally. To post the
  findings on a pull request, use `review-pr`.
argument-hint: "Optional: the change to review and the issue or spec it implements"
---

Review the change on three axes:

- Standards: does the code follow the project's conventions and best practices?
- Spec: does the code do what the originating issue or spec asked for?
- Tests: would the tests fail if the changed behavior were wrong?

The change is whatever the caller names: a branch, a pull request, or the uncommitted work in
progress. When nothing is named, review the current branch against its base.

Run each axis in its own `reviewer` agent, in parallel, so no axis pollutes another's context and
none shares the author's. Each reviewer opens its report with its exact model ID, copied from
its system prompt or runtime configuration. If neither names a model, it says so rather than
guessing. Keep each axis summary under 200 words. Keep individual findings concise without
omitting evidence needed to act on them.

Each finding states the problem, why it matters, and a concrete remedy, with its file and line
when applicable. Distinguish demonstrated problems from uncertain concerns.

Return the findings to the caller grouped by axis, each axis with the model that reviewed it.
Don't merge or rerank findings across the axes, so one never masks another. Publish nothing:
the review leaves no trace on the pull request, the issue, or the commit history.

## Spec

Review the change against the issue or spec it implements: the one the caller names, or the
issue a pull request closes. Failing both, use the pull request description when it defines the
expected behavior. If nothing provides a useful specification, skip this axis and say so.

Look for requirements that are missing or only partially implemented, behavior that was not
requested (scope creep), and requirements that appear implemented but are implemented incorrectly.
Cite the requirement behind each finding. For scope creep, point to the code nobody asked for.

## Standards

Identify the project's documented coding conventions and architectural decisions, including files
such as `CODING_STANDARDS.md`, `CONTRIBUTING.md`, and ADRs under common locations such as
`docs/adrs/`, `docs/decisions/`, or `adrs/`. Leave test quality and testing conventions to the
tests axis.

Every applicable ADR that hasn't been superseded is a project standard. Use it as precedent for
implementation choices, especially when it sets a cross-cutting constraint or a preferred pattern.
Its historical context and rejected alternatives are not requirements. Departing from an ADR is a
violation even when the code is clean. If the departure looks intentional, recommend a new ADR that
supersedes the old one instead of reverting the code. An architectural choice no ADR covers is a
candidate for a new one.

Read [code smells](code-smells.md) when evaluating code design. They apply even when the project
documents nothing, but a documented standard or ADR overrides them. Apply only relevant smells a
reviewer could reasonably act on and that are not already enforced by tooling.

## Tests

Review whether the tests would catch meaningful regressions in behavior the change adds or
alters. Consider added, modified, deleted, and existing tests, as well as tests that should
have been added but weren't. Judge the tests, not the implementation. A production defect shows
up here only as the missing or weak test that let it through.

Follow the project's documented testing conventions and established patterns, including
test strategy ADRs. They take precedence over the guidance below on test level, structure, and
smells. A test that ignores an established pattern is a finding even when it works. If the change
has no testable behavior, skip this axis and say so. If the project has no tests, report that once
rather than listing every untested behavior.

For each behavior the change adds or alters, find the test that protects it and ask whether a
plausible implementation mistake would make it fail. Weigh how much protection each behavior needs
by the change's risk. Check meaningful boundary values, error paths, and requirements from the
spec. When there is no spec, review against the contracts evident in the change and say so. For
bug fixes, verify that the tests would fail without the fix. Judge assertions by what they prove,
not test names, passing results, or coverage numbers.

Look for missing or weak assertions, expected values derived from the implementation under
test, mocks or fixtures that hide the behavior being verified, and tests that mostly assert on
their own mocks. Deleted or disabled tests, weakened assertions, and updated snapshots or golden
files each need a reason the change supplies. Without one, report them as possibly bent to fit
the implementation.

Check that each test runs at a level that can expose the likely failure. Prefer focused tests for
isolated logic and tests that cross real integration points for integration behavior. Recommend
broader tests only when existing tests cannot catch a concrete failure.

Read [test smells](test-smells.md) when evaluating test quality. Apply only relevant smells
that are not already enforced by tooling, and distinguish subjective smells from violations
of documented conventions.

For each finding, describe a plausible regression the current tests would miss and the
smallest test or assertion that would catch it. Report a changed behavior no test covers as a
coverage gap, not a smell. Don't ask for tests that duplicate existing coverage, exercise private
implementation details, or cover every changed line.

When executing tests or trying mutations would resolve uncertainty, use an isolated copy so
the other reviewers' working tree remains unchanged. Distinguish observed results from
conclusions based on code inspection.
