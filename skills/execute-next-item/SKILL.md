---
name: execute-next-item
description: Select the next backlog item, refine the implementation approach with me, then delegate the implementation to another agent. Use when starting work on a new backlog item.
---

Run /github-backlog-management:pick-item to select the next backlog item.

After the item has been selected, run /grilling using the /domain-modeling skill. Use the session to surface implementation details, challenge assumptions, and find edge cases before proposing a plan. Keep going until no significant unknowns remain.

Present the implementation plan for my review and then pause. Do not continue until I explicitly approve the plan.

Once the plan is approved, delegate the implementation to a new sub-agent. Give it:

- The selected item's identifier and a link to its issue.
- The approved implementation plan.
- A "suggested skills" section listing which skills the sub-agent should invoke during implementation, and why.
- Links or paths to relevant PRDs, plans, ADRs, issues, commits, and diffs instead of restating their content.

Before handing off, redact any sensitive information such as API keys, passwords, or personally identifiable information.
