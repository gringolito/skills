---
name: execute-next-item
description: Select the next item to work, discuss the implementation plan and produce a hand off to another agent to implement
---

Run /github-backlog-management:execute-item to select the next backlog item. Before investigating the problem and proposing a implementation plan ask user confirmation on the selected item.

After we agree on which issue to work on:
- Assign it to me.
- Move it to "In Progress".

Then, run a /grilling session, using the /domain-modeling skill, to identify and resolve any knowledge gaps before proposing an implementation plan. Wait for my approval of the implementation plan.

Once approved, call /handoff to delegate the implementation to another agent.
