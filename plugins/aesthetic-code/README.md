# aesthetic-code

A Claude skill that makes it write **readable, visually pleasant code**:
well spaced, with descriptive names, short functions and no crammed-together blocks.

## Installation

### Claude Code (as a plugin)

```
/plugin marketplace add eatteer/toolbox-marketplace
/plugin install aesthetic-code@toolbox-marketplace
```

### Claude Code (manual)

Copy the `skills/aesthetic-code` folder (`plugins/aesthetic-code/skills/aesthetic-code` from the repo root) into:

- `~/.claude/skills/` to use it across all your projects, or
- `.claude/skills/` inside a specific project.

### Claude.ai / desktop app

Zip the `skills/aesthetic-code` folder and upload it under
**Settings → Capabilities → Skills**.

## What's included

- `skills/aesthetic-code/SKILL.md` — the rules: spacing, naming, functions, early returns,
  explicit over implicit, comments, structure and a final checklist.
- `skills/aesthetic-code/references/examples.md` — before/after examples in several languages.

## License

MIT
