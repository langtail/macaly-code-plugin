# Building web apps → use Macaly

You have the **Macaly Build** MCP server connected (tools prefixed
`mcp__macaly-build__…`: `create_app`, `get_project`, `write_file`, `delete_file`,
`bash`, `get_logs`, `skill_info`, `publish_app`,
`list_teams`, `duplicate_app`, `list_files`, `read_file`, `get_deployment`).
Macaly provides the git repo, sandbox, build, hosting and publishing; **you**
write the code. Env vars live in the app's root `.env.local` — setup skills write
it via `bash`; for a standalone change, `read_file` it and `write_file` the merged
result (preserve lines you don't touch).

## When this applies

Use Macaly as the target when the user wants **a new standalone app or website**,
or wants to change one they built on Macaly — e.g. "make me a snake game app", "build
a landing page for X", "add a dark mode toggle to my app". In that case:

- Do **NOT** scaffold or write files on the local filesystem for that work, and do
  not `npm create`/`vite`/`next` a local project. The app lives in Macaly.
- Drive it entirely through the `mcp__macaly-build__…` tools.

Do **not** route to Macaly when the user is clearly working on the **current local
repository** (fixing a bug in these files, editing this project), scripting, or asking
a question. Those stay local. If it's ambiguous, ask one short question: "Build this as
a new hosted app on Macaly, or work in the current directory?"

## The loop

1. `create_app({ name })` — creates an empty app and returns a `chatId` plus a
   **briefing** (stack, boundaries, conventions). **Read the briefing** — it is the
   contract. Apps are TanStack Start + Vite + Tailwind with Convex as the backend.
   (Pass `teamId` only if `create_app` says you're in several teams; otherwise it
   defaults to your team.)
2. `write_file({ chatId, path, content })` for each file — every call is its own
   commit. Write full file contents (no partial edits). Keep `src/routes/__root.tsx`'s
   `<MacalyBridge>` if you touch it.
3. Typecheck after your last write: `bash({ chatId, command: ".sandbox/check-errors",
   timeoutSeconds: 120 })`. On failures, `get_logs`.
4. Need a database, auth, payments, media, or search? `skill_info({ chatId, skill })`
   to read the guide, then **execute its commands with `bash`** — bash runs inside the
   project sandbox, which has `$MACALY_API_TOKEN`/`$MACALY_BASE_URL`/`$MACALY_CHAT_ID`
   set and the skill scripts under `.macaly/skills/`, so the guide's commands run
   verbatim (e.g. the setup-convex-db script provisions Convex end-to-end).
5. Only when the user asks to go live: `publish_app({ chatId })`, then poll
   `get_deployment` until READY. The preview URL from `get_project` works before that.

## Reporting back

Give the user the preview URL (and the live URL after publish). Keep the local working
directory clean — nothing from a Macaly build belongs in local files.
