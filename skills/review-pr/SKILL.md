---
name: review-pr
description: >-
  Review a pull request against the project's standards and its spec, and post the findings on it
  as a pull request review. Use when the user asks to "review this PR" or wants a second pair of
  eyes on a pull request they are reviewing.
argument-hint: "The pull request number"
disable-model-invocation: true
---

Review the pull request with the `code-review` skill, then post its findings as one pull request
review, so each finding gets a thread the author can answer.

Post the review as a comment. Approving or requesting changes is the user's verdict, not yours.

Put each finding tied to a line in an inline comment on that line. In the review body, summarize
the findings under `## Standards` and `## Spec`, and give in full the ones with no line to comment
on. If the spec axis was skipped, say why under its heading. Under each heading, name the model
that reviewed that axis by its human-readable name, such as `GPT-6.1-Sol` rather than
`openai-codex/gpt-6.1-sol:high`. End the body with the finding count for each axis and its worst
finding, when any exist.

Write for the pull request's author: direct, specific, technical and courteous. Each finding says
what is wrong, why it matters, and what would fix it.
