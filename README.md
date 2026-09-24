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
| [ai-attribution](./skills/ai-attribution/) | Mark issues, pull requests, comments, reviews, and commits written on the user's behalf with a footnote naming the AI model that wrote them |
| [dispatch-issue](./skills/dispatch-issue/) | Autonomously take a ready-for-agent issue from the queue to a reviewed PR |
| [execute-next-item](./skills/execute-next-item/) | Select the next backlog item, discuss the implementation plan, and hand off to another agent |

## License

[MIT](./LICENSE)
