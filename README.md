# Macaly Build — Claude Code plugin

Makes **Macaly** the default target for app-building prompts in Claude Code. With this
installed, "make me a snake game app" builds and hosts a real app on Macaly instead of
scaffolding local files.

## Install

Add this repo as a plugin marketplace, then install the plugin:

```
/plugin marketplace add langtail/macaly-build-plugin
/plugin install macaly-build@macaly
```

(Or in the desktop app: Settings → Plugins → Add marketplace → paste
`https://github.com/langtail/macaly-build-plugin`.)

The MCP connection authenticates with a Macaly API key — export it before starting
Claude Code:

```bash
export MACALY_API_KEY=macaly_...
```

## What's in here

| File                                              | Role                                                                                                                                                   |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `.claude-plugin/marketplace.json`                 | Marketplace manifest — lets Claude Code install this repo via "Add marketplace".                                                                       |
| `plugins/macaly-build/.claude-plugin/plugin.json` | Plugin manifest.                                                                                                                                       |
| `plugins/macaly-build/.mcp.json`                  | Registers the Macaly Build MCP server (HTTP, API-key auth).                                                                                            |
| `plugins/macaly-build/CLAUDE.md`                  | **The routing policy** — tells the agent to build in Macaly for new-app requests, and when _not_ to (local repo work). This is the load-bearing piece. |
| `plugins/macaly-build/commands/build-app.md`      | `/build-app <idea>` — a friction-free explicit entry point.                                                                                            |

## The loop the agent follows

```
create_app  (read the briefing)
  → write_file × N  (each commits; the preview rebuilds after your last write)
  → bash ".sandbox/check-errors"  → get_logs on failure
  → skill_info, then bash to run the guide  (database/auth/payments/media, e.g. setup-convex-db)
  → preview_app  (preview URL; renders the app inline in clients that support MCP Apps)
  → publish_app  (only when the user asks) → get_deployment (until READY)
```

The full server reference lives in the Macaly repo at `docs/build-mcp.md`.
