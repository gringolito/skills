---
name: address-pr-reviews
description: >-
  Address all review comments on a pull request. Use when a pull request has received review
  feedback that needs to be resolved.
argument-hint: The pull request number
---

Resolve every review comment on the pull request. When you finish, each comment has a reply that
matches what the pull request's head contains.

Read all reviews, review threads and pull request comments, including replies inside threads you
answered before. Account for each one so none is overlooked.

If any feedback is ambiguous or reveals knowledge gaps, conduct a /grilling session using the
/domain-modeling skill to clarify the intent before making changes. A question the reviewer asks
gets an answer in its thread.

Apply a suggested change as written. Fix only obvious typos and style formats in it, and say so in
the reply.

Reply in each comment's own thread as soon as its fix is pushed, naming the commit and what
changed, or the decision you made instead. When the fix changes the UI, show the result in the
reply with the `capture-ui-evidence` skill.
