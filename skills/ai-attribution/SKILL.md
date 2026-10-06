---
name: ai-attribution
description: >-
  Add AI attribution to everything published on the user's behalf. Use whenever you
  create an issue or pull request, comment on or reply to an issue, post a pull request review or
  inline review comment, reply to a review thread, or write a commit message.
---

Add AI attribution to everything you publish on the user's behalf. This includes issues,
pull requests, comments, replies, reviews, inline review comments, and commit messages, whether you
write them directly or through another skill. Never publish without the attribution.

Get the user's name from the git configuration. Take the model from your system prompt or runtime
configuration. Do not guess either one. If you cannot determine them, ask the user before
publishing.

Write the model's human-readable name, not its ID. Runtimes and sub-agents often report a slug
such as `openai-codex/gpt-6.1-sol:high` or `openrouter/glm-5.3-flash:auto`. Drop the provider
prefix and the thinking-level or routing suffix, then write what remains the way its vendor does,
such as `GPT-6.1-Sol`, `GLM-5.3-Flash`, or `Claude Sonnet 4.5`.

When you delegate publishing to a sub-agent, do not pass your own model name in its instructions.
The sub-agent may run on a different model, so tell it to determine its own model the same way and
attribute the work to that model.

End issues, pull requests, comments, and reviews with:

```markdown
---
<sub>This comment was added on behalf of <name> by AI (<model>).</sub>
```

Adapt the wording to what you are publishing, such as "This issue was created", "This pull
request was opened", "This review was posted", or "This reply was added". Keep the rest of the
footnote unchanged.

For pull request reviews, add the attribution to both the review body and every inline review
comment, since GitHub displays each inline comment in its own thread.

Attribute commits with a `Co-Authored-By` trailer instead of a footnote. Name the model and use its
vendor's no-reply address, such as `noreply@anthropic.com` for Claude models or
`noreply@email.openai.com` for OpenAI models.

When editing a post or amending a commit, keep a single attribution naming the current model.
