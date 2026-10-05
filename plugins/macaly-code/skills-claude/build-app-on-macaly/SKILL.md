---
name: build-app-on-macaly
description: Build or modify a hosted web app when the user explicitly chooses Macaly, invokes the Macaly build command, or refers to an existing Macaly app.
---

Use this workflow after the user has chosen Macaly as the target. If a request could
refer either to a new hosted Macaly app or to the current local repository, ask which
target they intend before making changes.

Macaly provides the Git repository, isolated cloud sandbox, build pipeline, hosting,
and publishing. Application code is managed through the `macaly-cloud` MCP tools.

## Build workflow

1. `create_app({ name })` creates an empty app and returns a `chatId` plus a project
   briefing. Read the briefing because it defines the stack and project boundaries.
   Pass `teamId` only when the account has multiple teams; `list_teams` returns the
   available IDs. For an existing app, `list_projects` returns its `chatId`, and
   `get_project({ chatId, includeBriefing: true })` returns its working rules.
2. Use `edit_file({ chatId, path, edits, reasoning })` for exact text replacements in
   an existing file, and `write_file({ chatId, path, content, reasoning })` for new
   files or complete rewrites. Each write or edit creates a Git commit. `read_file`
   accepts `startLine` and `endLine` to read part of a large file. Preserve
   `<MacalyBridge>` when changing `src/routes/__root.tsx`.
3. Use `run_project_command` for development operations that file tools cannot
   perform, including package installation, framework CLIs, migrations, capability
   scripts, builds, tests, and validation. After the last change, run
   `.sandbox/check-errors --strict`; `get_logs` returns build, development-server,
   and deployment logs.
4. For Macaly platform capabilities such as database, authentication, payments,
   media, search, or integrations, `skill_info` returns the relevant setup guide and
   code patterns. The referenced commands run inside the selected project's sandbox.
5. Images, video, and other media go to the app's asset storage, not through
   `write_file` and not into `public/`. `upload_file` imports a public URL or returns
   an upload command for a local file, and `list_media` returns stored media and a
   link the user can upload files through.
6. At the end of every turn that changed the app, after the preview build succeeds,
   call `preview_app({ chatId })` and share the link it returns, even when an earlier
   link is still valid. The call marks the turn as finished. An optional `path` opens
   a subpage.
7. `publish_app({ chatId })` creates a publicly reachable production deployment and
   is used only after the user explicitly requests publication. `get_deployment`
   returns its status and `liveUrl`. `connect_domain` attaches a custom domain only
   when the user asks for it.

## Reporting

Return the preview URL, summarize the implemented changes and validation result, and
include `liveUrl` only after an explicit publish request.
