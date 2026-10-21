# toolbox-marketplace

Small, independent utilities for Claude Code. Each one is its own plugin,
so you install only the ones you need.

## Installation

Add the marketplace once:

```
/plugin marketplace add eatteer/toolbox-marketplace
```

Then install the plugins you want:

```
/plugin install <plugin>@toolbox-marketplace
```

## Plugins

| Plugin | What it does |
| --- | --- |
| [aesthetic-code](plugins/aesthetic-code) | Makes Claude write readable, visually pleasant code: well spaced, with good names and best practices. |

## Adding a plugin

1. Create `plugins/<plugin>/` with a `.claude-plugin/plugin.json`, a `README.md`, a `LICENSE`
   and its components (`skills/`, `commands/`, `agents/` or `hooks/`, whichever it needs).
2. Register it in `.claude-plugin/marketplace.json` with `"source": "./plugins/<plugin>"`.
3. Add a row to the table above.

Each plugin keeps its own `version`, so updating one never forces an update of the others.
Bump the `version` in the plugin's `plugin.json` on every change, otherwise users won't receive the update.

## License

MIT
