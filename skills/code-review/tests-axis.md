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

Use the test smells below when evaluating test quality. Apply only relevant smells that are not
already enforced by tooling, and distinguish subjective smells from violations of documented
conventions.

For each finding, describe a plausible regression the current tests would miss and the
smallest test or assertion that would catch it. Report a changed behavior no test covers as a
coverage gap, not a smell. Don't ask for tests that duplicate existing coverage, exercise private
implementation details, or cover every changed line.

When executing tests or trying mutations would resolve uncertainty, use an isolated copy so
the other reviewers' working tree remains unchanged. Distinguish observed results from
conclusions based on code inspection.

## Test smells

Mostly from Meszaros's _xUnit Test Patterns_. Smells are judgement calls, so report each one by
name as "possible Erratic Test". A violation of a documented testing convention is reported
plainly.

| Test smell | Description | Fix suggestion |
| --- | --- | --- |
| Assertion-Free Test | The test runs code but asserts nothing meaningful, or asserts only that no exception was thrown | Assert on the observable outcome the behavior promises |
| Overspecified Test | The test asserts on implementation details such as call order, private state, or exact mock interactions | Assert on outputs and observable effects so refactoring doesn't break it |
| Eager Test | One test verifies several unrelated behaviors | Split it so each test has one reason to fail |
| Assertion Roulette | Many assertions whose failure output doesn't say which expectation broke | Split the test or add messages that name the expectation |
| Obscure Test | The setup buries what is being tested and why the expected value is right | Name the scenario, keep only relevant setup, and derive expected values visibly |
| Mystery Guest | The test depends on an external file, fixture, or environment the reader can't see | Inline the data or name the fixture so its relevance is obvious |
| General Fixture | A shared setup builds far more than any single test needs | Give each test the minimal fixture it uses |
| Conditional Test Logic | `if`, loops, or `try` inside a test decide what gets asserted | Make each path its own test, or use a parameterized test |
| Erratic Test | The outcome depends on time, randomness, ordering, sleeps, concurrency, or the network | Inject clocks and seeds, isolate state, and wait on conditions instead of durations |
| Interacting Tests | Tests share mutable state, so one passes or fails depending on another | Reset or isolate state per test |
| Test Code Duplication | The same setup or assertion shape is copied across tests, hiding what differs between them | Extract a helper, builder, or parameterized case that keeps the differences visible |
| Test Logic in Production | Production code gains flags, hooks, or branches that exist only for tests | Move the seam to a proper dependency injection point |
