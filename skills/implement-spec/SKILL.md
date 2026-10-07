---
name: implement-spec
description: >-
  Implement a specification issue end-to-end by running `implement-issue` on each of its
  sub-issues in dependency order. Use when the user wants a whole spec or parent issue
  implemented, including all of its sub-issues. To implement a single issue, use
  `implement-issue`.
argument-hint: "The specification or parent issue number"
---

Take the specification issue from open to fully implemented. Each sub-issue is one unit of
work, handled by the `implement-issue` skill. That skill defines how a single issue is
implemented, reviewed, and merged. This one decides which sub-issues run and when.

Start by reading the specification issue and all its comments, then every sub-issue and its
comments. Use the `blocked_by` relationships to work out the dependencies. Follow the ordering
those dependencies require, and add no other except the exemplar below.

When several sub-issues make the same kind of change, such as rewrites that share a style or a
contract, run one of them first as the exemplar and hold the rest until the user has reviewed it.
The user's review of the first one sets rules every later one needs, and running them all at once
means each repeats the same mistakes. Unrelated sub-issues still run alongside it.

Run `implement-issue` on every sub-issue that is currently unblocked, passing it the sub-issue
number. Unblocked sub-issues can run in parallel, each in its own sub-agent with its own full
lifecycle. Give each sub-agent the rules the user's reviews have set so far, and pass a new one to
every sub-agent still working as soon as the user states it.

Keep watching the open pull requests. When the user merges one, re-check the sub-issues and run
`implement-issue` on any that are now unblocked. Keep going until every applicable sub-issue's
pull request is merged.

When `implement-issue` reports a blocker on a sub-issue, stop and report it rather than skip
the sub-issue.

When every sub-issue's pull request is merged, run the `scope-completeness-review` skill on the
specification issue. Report each completed sub-issue with its pull request and status, the
review's outcome, and any gaps or proposed follow-up issues without acting on them.
