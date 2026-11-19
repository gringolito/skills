---
name: windbag
description: >-
  Cut verbose prose so readers read it to the end. Use during code review and before opening or
  updating a pull request, on the code comments and docs in the diff, the pull request body, the
  bodies of issues it links, and the branch's commit messages. Also use when the user asks to cut
  verbose prose, shorten documentation, or make text more readable.
---

Your job is to cut every comment, doc passage, and sentence its reader doesn't need, and to
shorten what remains. Long prose gets skipped, and skipped text informs no one. Every decision
here, including each tie you break, answers one question: will a reader read this to the end?
When in doubt, cut. You're done when every accepted finding is applied, protected text is
untouched, and no information the reader needs has been lost.

The scope is what the caller names: files, a branch, or a pull request. For a branch or pull
request, review the docs and code comments in the diff, the pull request body, relevant linked
issues, and the branch's commit messages. When nothing is named, use the current branch against
its base.

Split the scope into small batches and spawn one `sonic` subagent per batch, in parallel. Sonic
runs on a small model that loses track of the catalogue as its input grows, so size each batch by
what it reads: about 100 lines of prose, from files of about 1,000 lines in total. A big file with
few comments still fills the context, so split it into batches by line range. Keep each kind of
prose in its own batches: comments and docs, the pull request and issue bodies, and commit
messages. Keep every comment, paragraph, and section whole within one batch, since its reader
reads all of it.

Each subagent acts as an independent reader whose goal is to find prose that can be removed,
shortened, or restructured so readers are more likely to read it to the end. Have each subagent
follow [rules.md](rules.md), examine every applicable pattern in its catalogue, and report
actionable findings with the evidence required by the rules.

Each subagent works in a fresh context and fetches its own text and diffs. Give it only the path
to the rules, the files, headings, line ranges, or commits in its batch, and the base and head
refs or the pull request and issue numbers, so it can search the rest of the scope for copies.
Tell it that a report with an unverified list, a check handed back, or a pattern name outside the
catalogue is invalid.

Before judging a report, check it as a whole. When it's invalid by the brief's terms, verifies
instead of cutting, skips the catalogue, or misreads the scope, rerun that subagent once and name
what went wrong. Don't rerun it again. Judge the findings of both reports, so a finding the rerun
drops is still judged.

Judge each finding on its own. Reject one only by citing the rule it breaks and the evidence that
it does; a ground the rules don't state isn't one. Findings that share a pattern or a file are
still judged one at a time, and none is rejected as part of a group.

Before applying, check that every place an accepted finding says a fact lives survives the other
accepted cuts. When two cuts each rely on the other's copy, drop the one that deletes the copy
where readers look first.

Apply the accepted findings. Edit files directly, preserving their meaning and the information
readers need. When changing a comment reveals that the code itself needs clarification, make the
smallest necessary code change, then build and run the tests.

Apply commit message findings to unmerged commits. Obtain approval before rewriting published
history or force-pushing. Edit pull request descriptions directly. Review linked issues for
context, but do not rewrite their bodies merely because the pull request references them. Only
edit an issue when it is explicitly within the requested editing scope, preserving its agreed
requirements and intent.

Summarize meaningful changes, where relocated information ended up, each rejected finding with
the rule it rests on, and any unresolved work.
