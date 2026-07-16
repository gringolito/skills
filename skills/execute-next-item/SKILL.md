---
name: execute-next-item
description: Select the next backlog item, refine the implementation approach with me, then delegate the implementation to another agent. Use when starting work on a new backlog item.
---

Run /github-backlog-management:pick-item to select the next backlog item.

After the item has been selected, conduct a /grilling session using the /domain-modeling skill. Use the session to uncover implementation details, challenge assumptions, identify edge cases, and resolve any knowledge gaps before proposing an implementation plan. Continue the discussion until you're confident there are no significant unknowns remaining.

Present the implementation plan for my review and then pause. Do not continue until I explicitly approve the plan.

Once the plan is approved, run /handoff to produce a complete implementation handoff based on the approved plan.

Finally, create a new implementation sub-agent. Pass it the generated handoff document together with an explicit instruction that it must not create or use Git worktrees under any circumstances.
