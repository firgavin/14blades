# 14blades

A collection of [Claude Code](https://claude.com/claude-code) skills.

## Skills

| Skill | Description |
| --- | --- |
| [semantic-code-review](semantic-code-review/SKILL.md) | Review PRs or code changes for behavioral correctness beyond mechanical checks — live scope, interface consumers, lifecycle, enforcement, ownership, bounds, real entry paths, observable tests, documentation, and evidence-backed findings. |
| [trim-reasoning-transcripts](trim-reasoning-transcripts/SKILL.md) | Audit or fix durable repository prose that leaks design-session shorthand, review vantage, or change narration, while preserving every load-bearing proposition. |

## Usage

Each skill is a directory containing a `SKILL.md` (plus supporting `agents/` and `references/`). To use one, copy or symlink its directory into your skills location:

- Personal: `~/.claude/skills/<skill-name>`
- Project: `.claude/skills/<skill-name>`

```sh
# Example: install semantic-code-review as a personal skill
ln -s "$PWD/semantic-code-review" ~/.claude/skills/semantic-code-review
```

Then invoke it in Claude Code with `/<skill-name>` or let Claude pick it up from its description.
