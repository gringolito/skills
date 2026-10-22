# Test smells

Mostly from Meszaros's _xUnit Test Patterns_. Smells are judgement calls, so report each one by
name as "possible Erratic Test". A violation of a documented testing convention is reported
plainly, and so is a changed behavior no test covers.

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
