# Standards axis

Does the code follow the project's conventions and best practices?

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

Use the code smells below when evaluating code design. They apply even when the project documents
nothing, but a documented standard or ADR overrides them. Apply only relevant smells a reviewer
could reasonably act on and that are not already enforced by tooling.

## Code smells

From Fowler's _Refactoring_, chapter 3. Smells are judgement calls, so report each one by name as
"possible Feature Envy". A violation of a documented standard or ADR is reported plainly.

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
