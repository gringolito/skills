---
name: tidy-pr
description: >-
  Tidy the current work into a clean, reviewable pull request that shows its intent, key files,
  and risks, without changing behavior. Use when the user wants to clean up, organize, or prepare
  a PR for review.
---

Turn the current work into a pull request a reviewer can follow on its own. They should see what
the change is for, which files matter, and where the risks are, without reconstructing the author's
intent from the raw diff. Cut the noise while leaving the behavior alone.

Start from the existing pull request, or open one if none exists. Read its commits, full diff,
changed paths, generated and mechanical files, and description. Note everything that slows a
reviewer down: noisy or tangled history, a stale or thin description, unrelated changes, mechanical
edits mixed with behavioral ones, missing test context, or no obvious place to start reading.

Rewrite history only when the user asked for it or approved your plan. If the history needs work,
propose the new commit structure first. Aim for commits that each hold one meaningful change and,
where practical, can be reviewed on their own.

Avoid changing code unless it is necessary to make the pull request accurate and reviewable. When
the code itself does not need to change, improve the description and reviewer guidance instead.
The description must start with a brief summary of the change and, when available, explain why the
change was needed. It should accurately describe what the change does, distinguish core files from
generated or mechanical ones, and cover meaningful risks, migration and rollout steps. Link issues,
design docs, dashboards, or other context when they explain the intent.

Never present a behavior change as cleanup. If the pull request is too big, or mixes unrelated
concerns so badly that tidying cannot make it reviewable, recommend splitting it instead of
polishing around the problem.

Before you finish, confirm that the pull request still represents the intended changes and that
nothing changed behavior by accident.