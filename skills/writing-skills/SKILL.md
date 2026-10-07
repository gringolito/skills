---
name: writing-skills
description: >-
  Write or edit text another agent will follow, such as a skill or agent instructions. Use when
  creating, rewriting, or reviewing agent-facing instructions.
---

Write for a capable agent that has never seen this project's history. Give it the outcome, the
rules it can't infer, and the moments it must stop for the user. Leave the mechanics to it.

Open with the goal and what done looks like in terms someone could verify. Then give each rule
the agent needs, one concern per short paragraph, with its reason when it isn't obvious. Say each
thing once.

Describe requirements in terms of observable outcomes and evidence rather than prescribing the
mechanics used to reach them. Tell the agent what must be true when it finishes and what must be
verified. Prescribe a particular tool, command, sub-agent, or sequence only when using it is part
of the requirement rather than one possible implementation.

Cut what the agent already knows: how to find the repo, which command does a job, what a common
term means, that it should read the issue it was given, or that an API exists. Cut what its context
already provides, such as configuration loaded by the agent instructions. Cut sentences that
describe an older version, a removed script, or a transition state. When rewriting, keep a sentence
because the agent needs it, not because the old text had it.

Keep the skill to its own job. When another skill covers part of the work, hand that part over by
name instead of restating its rules. Don't plan work the user will do, run another skill's review,
or describe what the platform does on its own.

Do not require delegation by default. Let the agent decide whether sub-agents help unless separate
context, independent judgement, parallel execution, or another property of delegation is itself
part of the skill's design.

Ask the user when the agent cannot determine what to do or when the choice belongs to the user,
such as choosing between valid alternatives. Require confirmation before actions that are
immediately visible to others and difficult to undo, such as publishing a release. Describe each
stop where it happens rather than collecting gates up front. Everything else proceeds without one.

Ask for a report only when the user will act on it. Drop statements such as "this skill is
read-only", "say what you couldn't check", and closing next-step lines unless they change what the
user does. When the agent writes something people will read, such as an issue body or review
findings, say what tone or qualities the output should have when that isn't already obvious.

Use structure when the structure carries meaning. A checklist, table, named review axis, or output
shape is useful when downstream work depends on it or when it makes the result easier to verify.
Avoid structure that merely turns prose into ceremony.

Prefer rules that remain true across implementations. Avoid encoding incidental details from the
current repository, toolchain, or workflow unless changing that detail would change the skill's
intended behavior.

The frontmatter description says what the skill does and when to use it, never how, and matches
the body. Put long references such as templates or rubrics in their own file next to the skill,
and link each with one sentence explaining when to read it.

Write plain sentences. Avoid numbered procedures, command blocks, unnecessary output templates,
emphasis words such as MUST or NEVER, persona openers, and harness tool names. Keep lines under
100 characters.

These rules apply to agent-facing text. Documentation written for people, such as a README, keeps
its author's voice.

