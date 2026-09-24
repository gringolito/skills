---
name: ai-attribution
description: >-
  Add an AI attribution footnote to everything published on the user's behalf. Use whenever you
  create an issue or pull request, comment on or reply to an issue, post a pull request review or
  inline review comment, reply to a review thread, or write a commit message.
---

Add an AI attribution footnote to everything you publish on the user's behalf. This includes issues,
pull requests, comments, replies, reviews, inline review comments, and commit messages, whether you
write them directly or through another skill. Never publish without the attribution.

Get the user's name from the git configuration. Take the model from your system prompt or runtime
configuration, using its exact name and version, such as `Claude Sonnet 4.5`. Do not guess either
one. If you cannot determine them, ask the user before publishing.

End issues, pull requests, comments, and reviews with:

```markdown
---
<sub>This comment was added on behalf of <name> by AI (<model>).</sub>
```

For pull request reviews, add the attribution to both the review body and every inline review
comment, since GitHub displays each inline comment in its own thread.

End commit messages with the plain-text version as its own paragraph, placed before footers such as
`Refs #<N>` and `Signed-off-by`:

```text
This commit was added on behalf of <name> by AI (<model>).
```

When editing a post or amending a commit, keep a single footnote naming the current model.
