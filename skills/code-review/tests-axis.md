# Tests axis

Would the tests fail if the changed behavior were wrong?

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
