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

Read [code smells](code-smells.md) when evaluating code design. They apply even when the project
documents nothing, but a documented standard or ADR overrides them. Apply only relevant smells a
reviewer could reasonably act on and that are not already enforced by tooling.
