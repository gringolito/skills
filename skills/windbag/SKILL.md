---
name: windbag
description: >-
  Cut verbose prose so readers read it to the end. Use during code review and before opening or
  updating a pull request, on the code comments, docs, and skill files in the diff, the pull
  request body, the bodies of issues it links, and the branch's commit messages. Also use when the
  user asks to cut verbose prose, shorten documentation, or make text more readable.
---

Your job is to cut every comment, doc passage, and sentence its reader doesn't need, and to
shorten what remains. Long prose gets skipped, and skipped text informs no one. Every decision
here, including each tie you break, answers one question: will a reader read this to the end?
You're done when every accepted finding is applied, protected text is untouched, and every fact a
cut removed is still where its reader will find it.

The scope is what the caller names: files, a branch, or a pull request. For a branch or pull
request, it's the docs, skill files, and code comments in the diff, the pull request body, the
bodies of issues it links, and the commit messages. When nothing is named, use the current branch
against its base. Agent replies are out of scope.

Sonic agents find cuts with the rules in [rules.md](rules.md), and you judge findings and write
fixes with them, so read them before you start.

Spawn one `sonic` subagent for each part of the scope that has prose: comments, docs, and skill
files; the pull request and issue bodies; and the commit messages. Each fresh context reads the
prose the way someone outside this work will. Each one fetches its own text and diffs with git and
gh, so give it only the path to the rules and where its prose lives, such as the base and head
refs, the pull request and issue numbers, or the named files.

Finding cuts is the sonic agents' job, so don't search the prose for ones they missed. Judge every
finding against the rules, reject it on the grounds they list, and decide each unsure finding by
checking its evidence yourself. When a report went wrong as a whole, such as skipping the
catalogue or misreading the scope, rerun that sonic agent once and name what went wrong. Judge
the second report on its own without rerunning again.

Write the fixes for what you accept. Edit files directly. For a surprise comment, make the code
change, then build and run the tests that cover it. Rewrite the branch's commits to apply commit
message findings, and ask the user before force-pushing. Edit pull request and issue bodies in
place.

Tell the user what changed, where moved facts landed, which findings you rejected and why, and
what open work remains.
