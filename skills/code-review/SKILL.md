---
name: code-review
description: >-
  Review a change against the project's standards and its spec. Use when an agent needs
  independent feedback on work in progress, such as before opening a pull request, or when the
  user wants a change reviewed locally. To post the findings on a pull request, use `review-pr`.
argument-hint: "Optional: the change to review and the issue or spec it implements"
---

Review the change on two axes:

- Standards: does the code follow the project's conventions and best practices?
- Spec: does the code do what the originating issue or spec asked for?

The change is whatever the caller names: a branch, a pull request, or the uncommitted work in
progress. When nothing is named, review the current branch against its base.

Run each axis in its own `reviewer` agent, in parallel, so neither pollutes the other's context and
neither shares the author's. Each reviewer should keep its report under 200 words and tie every
finding to the relevant file and line when possible. It opens the report with its exact model ID,
copied from its system prompt or runtime configuration. If neither names a model, it says so
rather than guessing.

Return the findings to the caller grouped by axis, each axis with the model that reviewed it.
Don't merge or rerank findings across the axes, so one never masks the other. Publish nothing:
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
`docs/adrs/`, `docs/decisions/`, or `adrs/`.

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
