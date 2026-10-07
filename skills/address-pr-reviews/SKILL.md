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

Treat a comment as ambiguous when the reviewer weighs options without picking one, says they are
unsure, or writes something with more than one reasonable reading. The choice belongs to the
reviewer, so don't make it for them. Reply in that comment's thread with the readings or options
you see and what each would change, and ask which one they want. Leave the code that comment
concerns untouched until they answer, and treat their answer as new feedback. Work you think the
comment implies beyond what it says is part of the question, not something to start. Carry on with
the clear comments meanwhile. A question the reviewer asks gets an answer in its thread.

Apply a suggested change as written. Fix only obvious typos and style formats in it, and say so in
the reply.

Reply in each comment's own thread as soon as its fix is pushed, naming the commit and what
changed, or why you left the code as it is.

When you report back while questions to a reviewer are still open, name them, since the pull
request waits on those answers.
