---
name: scope-completeness-review
description: >-
  Check that a parent or spec issue's original scope is fully built, across its closed sub-issues
  and the code, and close the issue when it is. Use when a parent issue is ready for a final
  completeness check, or the user asks whether an issue is really done.
argument-hint: "The issue number"
---

Confirm that everything the issue asked for exists in the code before closing it. A closed
sub-issue claims coverage but doesn't prove it, so check each claim against the current code.

Read the original issue, including its acceptance criteria, requirements, and comments, then read
every closed sub-issue created to address it. Treat each acceptance criterion or requirement as a
separate requirement. For each, find the sub-issue that addresses it, if any, and read the code to
confirm it does what the requirement asks.

Build a coverage checklist with one row per requirement, listing the requirement, the sub-issue
that addresses it, the relevant code, and its status. The status is covered, partial, missing, or
divergent when the code does something other than what the issue asked for.

If every requirement is covered, post the checklist as a comment on the issue and close it as
completed. Otherwise, leave the issue open, report the gaps, and propose a follow-on issue for each
one that describes the remaining work.
