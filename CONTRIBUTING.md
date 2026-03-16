# Contributing a Plugin to Colony

## Steps

### 1. Add the subtree

```bash
git subtree add --prefix=plugins/<name> https://github.com/<owner>/<repo>.git main --squash
```

### 2. Add entry to colony.json

```json
{
  "name": "<name>",
  "description": "<description>",
  "repo": "<owner>/<repo>",
  "branch": "main",
  "path": "plugins/<name>",
  "category": "<category>",
  "tags": ["<tag1>", "<tag2>"]
}
```

### 3. Add entry to .claude-plugin/marketplace.json

```json
{
  "name": "<name>",
  "source": "./plugins/<name>",
  "description": "<description>",
  "category": "<category>",
  "tags": ["<tag1>", "<tag2>"]
}
```

### 4. Add notify workflow to the plugin repo

Copy `templates/notify-colony.yml` to the plugin repo at `.github/workflows/notify-colony.yml` and replace `PLUGIN_NAME_HERE` with the plugin name.

The sync workflow reads `colony.json` dynamically — no workflow changes needed in Colony.

## Plugin Requirements

Your plugin repo must have a `.claude-plugin/plugin.json` with at minimum:

```json
{
  "name": "<name>",
  "version": "1.0.0",
  "description": "<description>"
}
```

See the [Claude Code plugin docs](https://code.claude.com/docs/en/plugins) for the full plugin spec.
