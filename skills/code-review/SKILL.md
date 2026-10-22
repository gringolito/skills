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
`docs/adrs/`, `docs/decisions/`, or `adrs/`. Leave test code and testing conventions to the tests
axis.

Every applicable ADR that hasn't been superseded is a project standard. Use it as precedent for
implementation choices, especially when it sets a cross-cutting constraint or a preferred pattern.
Its historical context and rejected alternatives are not requirements. Departing from an ADR is a
violation even when the code is clean. If the departure looks intentional, recommend a new ADR that
supersedes the old one instead of reverting the code. An architectural choice no ADR covers is a
candidate for a new one.

The standards axis also looks for the code smells below, taken from Fowler's _Refactoring_ book,
chapter 3. They apply even when the project documents nothing, but a documented standard or ADR
overrides them.

| Code smell | Description | Fix suggestion |
| --- | --- | --- |
| Mysterious Name | A function, variable, or type whose name doesn't reveal what it does or holds | Rename it. If no honest name comes to mind, the design is unclear |
| Duplicated Code | The same logic shape appears in more than one hunk or file of the change | Extract the shared shape and call it from both |
| Feature Envy | A method uses another object's data more than its own | Move it onto the data it envies |
| Data Clumps | The same few fields or parameters keep travelling together | Bundle them into one type |
| Primitive Obsession | A primitive or string stands in for a domain concept | Give the concept its own small type |
| Repeated Switches | The same `switch` or `if` cascade on the same type recurs across the change | Replace it with polymorphism, or with one map both sites share |
| Shotgun Surgery | One logical change forces scattered edits across many files | Gather what changes together into one module |
| Divergent Change | One file or module changes for several unrelated reasons | Split it so each part changes for one reason |
| Speculative Generality | Abstractions, parameters, or hooks serve needs the spec doesn't have | Delete them until a real need shows up |
| Message Chains | The caller walks a long `a.b().c().d()` chain | Hide the walk behind one method on the first object |
| Middle Man | A class or function mostly delegates onward | Cut it and call the real target directly |
| Refused Bequest | A subclass or implementer ignores or overrides most of what it inherits | Drop the inheritance and use composition |

Only report smells that are relevant to the change and that a reviewer could reasonably act on.
Skip anything a tool already enforces. Smells are judgement calls, so report each one by name as
"possible Feature Envy", apart from violations of documented standards and ADRs.

## Tests

Review whether the tests would catch a meaningful regression in the behavior the change adds or
alters. That covers the tests the change adds, modifies, or deletes, the existing tests that
already exercise the changed code, and the tests the change should have touched but didn't. Judge
the tests, not the production code. An implementation defect belongs to the spec or standards axis
and shows up here only as the missing or weak test that let it through. If the change has no
testable behavior, such as documentation or formatting alone, skip this axis and say so. If the
project has no tests at all, say so once instead of listing every untested behavior.

Start from the project's testing conventions: testing sections in `CONTRIBUTING.md` or
`CODING_STANDARDS.md`, ADRs about test strategy, and the patterns established tests already
follow, such as framework, fixture style, naming, and where each kind of test lives. A new test
that ignores an established pattern is a finding even when it works.

The central question is whether a test would fail if the code it covers were wrong. Find the tests
that pin down each changed behavior, including error paths, boundaries, and the branches the diff
adds, and weigh how much protection each needs by its risk, not by its line count. When the spec
axis has a spec, map its requirements to tests and report any requirement nothing verifies. When
it doesn't, review against the contracts evident in the change and say so. For a bug fix, a test
should fail without the fix. If the fix could be reverted with the test still passing, the test
doesn't guard the bug.

Judge a test by what it establishes, not by its name, its passing run, or the coverage it adds.
Read each assertion and ask whether a plausible mistake in the code, such as an off-by-one, a
swapped argument, or a dropped condition, would make it fail. An assertion that survives every
such mistake is decoration. Watch for expected values computed by the same logic under test, and
for mocks or fixtures that replace the behavior the test claims to verify or drop the part of a
contract the scenario depends on.

Treat changes to existing tests with suspicion. Deleted tests, newly skipped or disabled tests,
loosened assertions, widened tolerances, and regenerated snapshots or golden files each need a
reason the change supplies. Without one, report them as possibly bending the tests to fit the code.

Check that each test runs at a level that can expose the likely failure. Isolated logic belongs in
unit tests rather than reached only through a slow end-to-end path. Failures in wiring,
persistence, serialization, or external contracts need a test that crosses that boundary for real
instead of mocking both sides of it. Mocks are for boundaries the project doesn't own or can't run
cheaply, and a test that mostly asserts on its own mocks verifies nothing. Recommend a broader
test only when you can name the failure the existing tests can't catch.

When running a focused test or trying a small mutation would settle an uncertain finding, do it in
a scratch copy of the change, not the working tree the other reviewers are reading. Say what you
ran, and keep what you observed apart from what you concluded by reading.

The tests axis also looks for the test smells below, mostly from Meszaros's _xUnit Test Patterns_.
A documented project convention overrides them.

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

Keep findings tied to the change. For each gap, describe a concrete wrong behavior the current
tests would let pass and the smallest test or assertion that would catch it. Don't ask for tests to
raise coverage, exercise private helpers, duplicate existing protection, or suit a preferred
style, and don't require a test for every changed line. Report only smells that are relevant to
the change and that a reviewer could reasonably act on, and skip what a tool already enforces,
such as coverage thresholds or lint rules for tests. Report each smell by name as "possible
Erratic Test", apart from violations of documented testing conventions. A changed behavior no test
covers is a demonstrated problem, not a smell, so report it plainly.
