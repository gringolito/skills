# skills

[![skills.sh](https://skills.sh/b/gringolito/skills)](https://skills.sh/gringolito/skills)

AI skills for common tasks — a collection of reusable skills for AI agents that provide procedural knowledge to help them accomplish specific tasks more effectively.

## Installation

Install all skills from this repository using the [skills.sh](https://skills.sh) CLI:

```sh
npx skills add gringolito/skills
```

This will install all skills and make them available to your AI agent.

## Skills

| Skill | Description |
| --- | --- |
| [address-pr-reviews](./skills/address-pr-reviews/) | Address review comments on pull requests |
| [execute-next-item](./skills/execute-next-item/) | Select the next backlog item, discuss the implementation plan, and hand off to another agent |
| [handoff](./skills/handoff/) | Compact the current conversation into a handoff document for another agent to pick up (forked from [mattpocock/skills](https://github.com/mattpocock/skills) so the model can invoke it correctly) |

## License

[MIT](./LICENSE)
