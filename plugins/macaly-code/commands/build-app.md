---
name: build-app
description: Build a new web app on Macaly from a one-line idea
---

Build a new web app on **Macaly** for this request: **$ARGUMENTS**

Use the macaly-code MCP server's tools — do not write any files on
the local filesystem for this. Steps:

1. `create_app` with a short `name` derived from the request. Read the returned
   **briefing** in full before writing any code (stack, boundaries, conventions).
2. Implement the app with `write_file` (one commit per file, full contents). Build a
   real, working first version — routes, components, styles — following the briefing's
   TanStack Start + Tailwind conventions. If it needs persistence/auth/payments/media,
   read `skill_info` (e.g. `setup-convex-db`) and execute the guide's commands with
   the available project command tool (`run_project_command` or `bash`) — it runs
   inside the project sandbox where the guide's credentials and scripts live.
3. Typecheck after your last write with the project command tool and command
   `.sandbox/check-errors` (timeoutSeconds: 120), then fix anything it reports (use
   `get_logs` for detail).
4. Show the result with `preview_app` and report its preview URL. The universal MCP
   endpoint may also render it inline in clients that support MCP Apps. Do **not** `publish_app`
   unless the user asks to go live.

Keep it scoped to what was asked — a clean, working first version, not a speculative
platform.
