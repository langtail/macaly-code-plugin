# Macaly agent plugins

Build and host real web apps on [Macaly](https://www.macaly.com) with your favorite
agent harness. With the **macaly-code** plugin installed, "build a customer feedback dashboard"
becomes a real, deployable app with a live preview — instead of local scaffolding.

## Claude Code

**One-click install**

[Install macaly-code in Claude](https://claude.ai/desktop/customize/plugins/new?marketplace=langtail/macaly-code-plugin&plugin=macaly-code)

```
https://claude.ai/desktop/customize/plugins/new?marketplace=langtail/macaly-code-plugin&plugin=macaly-code
```

This opens the Claude desktop app straight on the macaly-code install dialog, with the
marketplace already filled in.

**Or add the custom marketplace by hand**

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
codex plugin add macaly-code@macaly
```

**Authenticate with Macaly**

```sh
codex mcp login macaly-code
```

Complete the OAuth flow in your browser. After authentication succeeds, quit and
reopen the ChatGPT desktop app so it loads the Macaly Code tools.

You can also browse and install plugins interactively by running `/plugins` inside
Codex CLI after adding the marketplace.

## Cursor

**One-click install (no marketplace needed)**

[Add Macaly Cloud to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=macaly-cloud&config=eyJ1cmwiOiJodHRwczovL3d3dy5tYWNhbHkuY29tL2FwaS9jbG91ZC9tY3AifQ==)

```
cursor://anysphere.cursor-deeplink/mcp/install?name=macaly-cloud&config=eyJ1cmwiOiJodHRwczovL3d3dy5tYWNhbHkuY29tL2FwaS9jbG91ZC9tY3AifQ==
```

Cursor opens the browser for the Macaly OAuth sign-in. After that the server shows as
connected under Settings > Tools & MCP.

**From the Cursor Marketplace**

```sh
/add-plugin macaly-code
```

**Or add the server by hand** in `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "macaly-cloud": {
      "url": "https://www.macaly.com/api/cloud/mcp"
    }
  }
}
```

Team admins who use an MCP allowlist must allow `https://www.macaly.com/api/cloud/mcp`.

## What the plugin ships

| Piece                               | Role                                                               |
| ----------------------------------- | ------------------------------------------------------------------ |
| `.mcp.chatgpt.json`                 | ChatGPT/Codex directory profile without embedded preview UI.       |
| `.mcp.claude.json`                  | Claude directory profile with review-scoped descriptors.           |
| `.mcp.universal.json`               | Universal MCP connection for Cursor and direct installations.      |
| `skills-cursor/build-app-on-macaly` | The Cursor workflow with the full hosted-app build loop.           |
| `skills-claude/build-app-on-macaly` | Claude-specific workflow scoped to user-selected Macaly work.      |
| `skills-codex/build-app-on-macaly`  | The Codex workflow with the full hosted-app build loop.            |
| `rules/route-app-builds-to-macaly`  | Scopes explicitly selected Macaly work and keeps local work local. |
| `commands/build-app`                | `/build-app <idea>` — a friction-free explicit entry point.        |

The server reference lives in the Macaly repo at `docs/code-mcp.md`.

For the OpenAI Plugins Directory listing, reviewer tests, tool-annotation
justifications, and remaining portal steps, see
[`docs/openai-submission.md`](docs/openai-submission.md).

For the Anthropic Connectors and Plugins directories, use the dedicated endpoint and
checklist in [`docs/anthropic-submission.md`](docs/anthropic-submission.md).
