---
name: the-office
description: >-
  Use for sales prospecting and outreach with The Office CLI: LinkedIn and
  HubSpot lead/account search, profile and company enrichment, building and
  running campaigns (custom, AI, signals), sending LinkedIn invites and
  messages, and sales dashboards/reports. Trigger when the user wants to find
  leads, enrich prospects, run outreach, or check sales pipeline data.
---

# The Office CLI

The Office is a sales CLI for LinkedIn/HubSpot prospecting and outreach,
designed to be driven by an AI agent. Every command is self-describing, so you
discover the exact contract at runtime rather than guessing.

## Running the CLI

Always invoke the bundled wrapper by its absolute path — it is **not** on
`$PATH`:

```bash
"${CLAUDE_PLUGIN_ROOT}/bin/the-office" <command> [flags]
```

The wrapper runs the published `the-office-cli` via `uvx` (or `pipx`),
fetching and caching it on first use. If neither is installed it prints install
instructions and exits 127 — surface that to the user.

## Workflow: discover, preview, run

1. **List** what exists:
   `"${CLAUDE_PLUGIN_ROOT}/bin/the-office" tour` — every command + description.
2. **Inspect** the exact JSON input/output before calling anything new:
   `"${CLAUDE_PLUGIN_ROOT}/bin/the-office" <command> --schema`
3. **Preview** a mutating call without side effects:
   `"${CLAUDE_PLUGIN_ROOT}/bin/the-office" <command> --dry-run`
4. **Run** it. Commands read JSON from stdin where applicable and print JSON.

Prefer this loop over assuming flags — treat `--schema` as the source of truth.

## First steps in a session

- `whoami` — current user + LinkedIn connection status.
- `fire-drill` — checks API, auth, and LinkedIn are connected.
- If not authenticated: `login` (browser sign-in) or `login --email ...` (CI).
- If LinkedIn isn't connected: `linkedin connect account`.

## Command surface (run `tour` for the full list)

- **Auth/identity**: `login`, `logout`, `whoami`, `create-account`, `fire-drill`
- **Settings**: `settings get`, `settings update`
- **LinkedIn search**: `linkedin search parameters`,
  `linkedin search classic {leads,accounts,jobs,posts}`,
  `linkedin search sales navigator {leads,accounts}`
- **LinkedIn enrich**: `linkedin retrieve profile`, `linkedin retrieve company`,
  `linkedin me`, `linkedin list relations`
- **LinkedIn outreach**: `linkedin send invite`, `linkedin send message`,
  `linkedin start chat`, `linkedin list chats`, `linkedin list messages`,
  `linkedin list user messages`, `linkedin check connection`,
  `linkedin reconnect account`
- **Campaigns**: `campaign create custom`, `campaign create ai`,
  `campaign create signals`
- **Import**: `lead import`, `account import`
- **Reporting**: `report`, `dashboard funnel`, `dashboard conversations`

## Notes

- Mutating outreach (invites, messages, campaigns) is real activity on the
  user's LinkedIn/account — confirm intent and prefer `--dry-run` first.
- Config (`api_url`, `app_url`) and the auth token live in `~/.the-office/`.
