---
name: build-app-on-macaly
description: Build or modify a hosted web app when the user explicitly chooses Macaly, invokes the Macaly build command, or refers to an existing Macaly app.
---

Use this workflow after the user has chosen Macaly as the target. If a request could
refer either to a new hosted Macaly app or to the current local repository, ask which
target they intend before making changes.

Macaly provides the Git repository, isolated cloud sandbox, build pipeline, hosting,
and publishing. Application code is managed through the `macaly-code` MCP tools.

## Build workflow

1. `create_app({ name })` creates an empty app and returns a `chatId` plus a project
   briefing. Read the briefing because it defines the stack and project boundaries.
   Pass `teamId` only when the account has multiple teams; `list_teams` returns the
   available IDs.
2. Use `write_file({ chatId, path, content, reasoning })` for complete file contents.
   Each write creates a Git commit. Preserve `<MacalyBridge>` when changing
   `src/routes/__root.tsx`.
3. Use `run_project_command` for development operations that file tools cannot
   perform, including package installation, framework CLIs, migrations, capability
   scripts, builds, tests, and `.sandbox/check-errors` validation.
4. For Macaly platform capabilities such as database, authentication, payments,
   media, search, or integrations, `skill_info` returns the relevant setup guide and
   code patterns. The referenced commands run inside the selected project's sandbox.
5. `preview_app({ chatId })` returns the current live preview URL and build status.
6. `publish_app({ chatId })` creates a publicly reachable production deployment and
   is used only after the user explicitly requests publication. `get_deployment`
   returns its status and live URL.

## Reporting

Return the preview URL, summarize the implemented changes and validation result, and
include the production URL only after an explicit publish request.
