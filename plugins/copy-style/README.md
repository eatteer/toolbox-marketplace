# copy-style

A Claude skill that makes it write **consistent copy**: sentence case instead of Title Case,
one term per concept (no "agregar" here, "añadir" there and "adicionar" elsewhere),
and the same tone, verb form and punctuation everywhere.

## Installation

### Claude Code (as a plugin)

```
/plugin marketplace add eatteer/toolbox-marketplace
/plugin install copy-style@toolbox-marketplace
```

### Claude Code (manual)

Copy the `skills/copy-style` folder (`plugins/copy-style/skills/copy-style` from the repo root) into:

- `~/.claude/skills/` to use it across all your projects, or
- `.claude/skills/` inside a specific project.

### Claude.ai / desktop app

Zip the `skills/copy-style` folder and upload it under
**Settings → Capabilities → Skills**.

## What's included

- `skills/copy-style/SKILL.md` — the rules: sentence case, one term per concept,
  voice and form, punctuation and formatting, copy inside code and a final checklist.
- `skills/copy-style/references/examples.md` — before/after examples in Spanish and English.

## License

MIT
