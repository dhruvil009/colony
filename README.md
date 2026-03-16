# Colony

A plugin marketplace repository. Clone Colony, get everything.

All plugin development happens in individual repos. Colony syncs from them automatically using git subtree.

## Quick Start

```bash
git clone https://github.com/dhruvil009/Colony.git
cd Colony
```

All plugins are available under `plugins/`.

## Available Plugins

| Plugin | Description | Source |
|--------|-------------|--------|
| [hivescanner](plugins/hivescanner/) | Unified notification scanner — GitHub, Slack, Discord, and more | [dhruvil009/hivescanner](https://github.com/dhruvil009/hivescanner) |

See `colony.json` for the full machine-readable registry.

## Adding a New Plugin

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

### 3. Add notify workflow to the plugin repo

Copy `templates/notify-colony.yml` to the plugin repo at `.github/workflows/notify-colony.yml` and replace `PLUGIN_NAME_HERE` with the plugin name.

The sync workflow reads `colony.json` dynamically — no workflow changes needed in Colony.

## How Sync Works

1. A plugin repo pushes to `main`
2. The plugin's `notify-colony.yml` workflow sends a `repository_dispatch` event to Colony
3. Colony's `sync-plugins.yml` workflow reads `colony.json`, runs `git subtree pull` for each plugin, and pushes any updates
4. As a fallback, the sync workflow also runs on a schedule (every 6 hours)
