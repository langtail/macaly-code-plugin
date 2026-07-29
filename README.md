# Macaly agent plugins

Build and host real web apps on [Macaly](https://www.macaly.com) with your favorite
agent harness. With the **macaly-code** plugin installed, "make me a snake game app"
becomes a real, deployable app with a live preview — instead of local scaffolding.

## Claude Code

**Add the custom marketplace**

```sh
/plugin marketplace add langtail/macaly-code-plugin
```

**Install the plugin**

```sh
/plugin install macaly-code@macaly
```

## Codex

**Add the custom marketplace**

```sh
codex plugin marketplace add langtail/macaly-code-plugin
```

**Install the plugin**

```sh
codex plugin install macaly-code@macaly
```

You can also browse and install plugins interactively by running `/plugins` inside
Codex CLI after adding the marketplace.

## Cursor

Once the plugin is listed on the Cursor marketplace:

```sh
/add-plugin macaly-code
```

The `.cursor-plugin/` manifests in this repo are marketplace-ready.

## What the plugin ships

| Piece                              | Role                                                           |
| ---------------------------------- | --------------------------------------------------------------- |
| `mcp.json`                         | The Macaly Code MCP connection (HTTP, API-key auth).            |
| `skills/build-app-on-macaly`       | The full build loop the agent follows.                          |
| `rules/route-app-builds-to-macaly` | Routes new-app prompts to Macaly, keeps local-repo work local.  |
| `commands/build-app`               | `/build-app <idea>` — a friction-free explicit entry point.     |

The server reference lives in the Macaly repo at `docs/code-mcp.md`.
