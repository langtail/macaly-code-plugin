# Anthropic directory submission

Submit the remote connector using the Claude-specific production endpoint:

```text
https://www.macaly.com/api/cloud/claude/mcp
```

The related Claude plugin uses the same endpoint through `.mcp.claude.json` and
loads the Claude-specific `skills-claude/build-app-on-macaly` workflow.
The package does not include a default `.mcp.json`, so Claude cannot also
auto-discover the universal endpoint.

## Why this endpoint is separate

- It exposes all 20 Macaly Cloud capabilities.
- Full project-shell functionality remains available as `run_project_command`.
- Read operations advertise `readOnlyHint: true`.
- Operations that can overwrite, delete, or publish data advertise
  `destructiveHint: true`, which makes Claude request confirmation. `preview_app` and
  `upload_file` only add data, so they advertise `destructiveHint: false`.
- Tool descriptions cover one action each and do not direct Claude through other
  tools or make Macaly the default for unrelated requests.
- `preview_app` returns a URL without registering an MCP App iframe resource, so the
  connector does not require MCP App carousel screenshots.
- The universal endpoint remains unchanged for direct installations and other
  clients.

## Connector listing

- Name: `Macaly Cloud`
- Tagline: `Build and host apps on Macaly`
- Server type: Remote MCP, Streamable HTTP
- Authentication: OAuth 2.0 with Dynamic Client Registration
- Website: `https://www.macaly.com`
- Documentation: `https://www.macaly.com/docs/en/welcome/overview`
- Privacy policy: `https://www.macaly.com/privacy-policy`
- Terms: `https://www.macaly.com/terms-of-service`
- Public source: `https://github.com/langtail/macaly-code-plugin`
- Allowed link URIs: none required for the Claude profile; it does not register the
  preview widget or call `ui/open-link`.

## Tool permission summary

| Tools                                                                                                                         | Permission hint                               | Behavior                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `list_teams`, `list_projects`, `get_project`, `get_messages`, `list_files`, `read_file`, `skill_info`, `list_media`, `get_deployment` | `readOnlyHint: true`                          | Retrieve account, project, conversation, source, guide, media, or deployment information without changing project data.                         |
| `preview_app`                                                                                                                 | `destructiveHint: false`                      | Creates a three-hour preview access link and marks the turn as finished. It does not build, change project data, or publish.                    |
| `upload_file`                                                                                                                 | `destructiveHint: false`, `openWorldHint: true` | Adds a new asset from a public URL or a local file. It does not overwrite or delete existing data.                                              |
| `create_app`, `duplicate_app`, `write_file`, `edit_file`, `delete_file`, `get_logs`                                           | `destructiveHint: true`                       | Create or change private Macaly infrastructure or project state and therefore require confirmation in Claude.                                  |
| `run_project_command`                                                                                                         | `destructiveHint: true`, `openWorldHint: true`  | Runs a full shell in the selected project's isolated cloud sandbox; it can change project data and reach external hosts.                       |
| `publish_app`                                                                                                                 | `destructiveHint: true`, `openWorldHint: true`  | Creates a publicly reachable production deployment after an explicit user request.                                                             |
| `connect_domain`                                                                                                              | `destructiveHint: true`, `openWorldHint: true`  | Connects a custom domain. A confirmed move removes the domain from another app first.                                                          |

`get_logs` may initialize the selected project's ephemeral sandbox for development
logs. It does not change project files, but the Claude profile conservatively treats
that infrastructure side effect as a state-changing operation.

## Review preparation

1. Deploy the Claude endpoint and verify its OAuth metadata at
   `/.well-known/oauth-protected-resource/api/cloud/claude/mcp`.
2. Connect the endpoint as a custom connector and complete OAuth.
3. Exercise every tool through MCP Inspector and Claude with valid parameters.
4. Use a populated reviewer account with sample apps, build logs, a completed
   deployment, and permission to create and publish apps.
5. Verify that invalid chat IDs, unauthorized projects, protected file paths, command
   timeouts, and failed deployments return actionable errors.
6. Run `claude plugin validate` against `plugins/macaly-code` before submitting the
   plugin repository.

## Remaining review risk

`run_project_command` intentionally remains a full project-scoped shell because
package installation, migrations, framework CLIs, capability setup, builds, and
tests are required for real applications. Its description and annotations disclose
that capability directly. If Anthropic rejects free-form project commands, retain
the universal endpoint and replace only the Claude directory profile with narrower
purpose-built command tools based on the reviewer feedback.
