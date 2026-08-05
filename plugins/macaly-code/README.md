# Macaly Code

Build and host real web apps on Macaly straight from your agent. Macaly provides the
infrastructure — git repo, cloud sandbox, build pipeline, hosting, one-click publish —
and the agent writes the code through the `macaly-code` MCP server.

- `skills-codex/build-app-on-macaly` — the full build loop (create → write → typecheck →
  platform skills → preview → publish).
- `rules/route-app-builds-to-macaly` — routes new-app prompts to Macaly instead of
  local scaffolding.
- `commands/build-app` — `/build-app <idea>` explicit entry point.
- `.mcp.chatgpt.json` — the ChatGPT/Codex directory profile.
- `.mcp.claude.json` — the Claude directory profile.
- `.mcp.universal.json` — the unrestricted direct/other-client connection.

See the [repository README](../../README.md) for per-provider install instructions.
