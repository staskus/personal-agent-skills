# Personal agent skills

Skills I use with coding agents. Copy, use, or adapt them.

## Skills

| Skill | Purpose |
| --- | --- |
| [explain-in-code-snippets](skills/explain-in-code-snippets/SKILL.md) | Explain code changes with annotated snippets, runtime behavior, evidence, and limits. |
| [snapshot-driven-ui-iteration](skills/snapshot-driven-ui-iteration/SKILL.md) | Render UI snapshots, compare them with the design goal, and correct visible differences. |

## Install

With Node.js 22.20 or later installed, run:

```sh
npx skills add staskus/personal-agent-skills --skill explain-in-code-snippets
```

To install the snapshot skill:

```sh
npx skills add staskus/personal-agent-skills --skill snapshot-driven-ui-iteration
```

Choose your agent and installation scope when prompted. Add `--global` to install across projects.

## Use

Ask your agent:

```text
Explain this PR in code snippets.
```

Or:

```text
Explain your changes in code snippets.
```

For UI changes:

```text
Use snapshot-driven-ui-iteration to implement this UI change and compare it with the design reference.
```

## Contribute

Open an issue or PR with a specific change and a use case.

## License

[MIT](LICENSE).
