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

Apply a suggested change as written. Fix only obvious typos in it, and say so in the reply.

Reply in each comment's own thread as soon as its fix is pushed, naming the commit and what
changed, or the decision you made instead. Before replying that something is fixed or removed,
check that the change is on the pull request's head. Thread line numbers refer to the commit the
comment was made on, so confirm you changed the text the comment points at. A finding in a review
body with no line gets one reply comment that quotes it and says how it was handled.

If a reply can't be posted, report that to the user instead of leaving the comment unanswered.
