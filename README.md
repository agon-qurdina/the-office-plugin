# The Office — Claude Code Plugin

The [Claude Code](https://code.claude.com/docs) plugin for **The Office CLI** — the
World's Best Sales CLI. Installing this plugin teaches your Claude Code agent
*when* to reach for the CLI (LinkedIn/HubSpot prospecting, lead and account
search, campaigns, outreach, dashboards) and lets the agent run the CLI on
demand — no separate install step.

> The CLI itself is published to PyPI as
> [`the-office-cli`](https://pypi.org/project/the-office-cli/). This repo only
> contains the Claude Code plugin.

## Install

In a terminal, start Claude Code:

```bash
claude
```

Then add this marketplace and install the plugin:

```text
/plugin marketplace add agon-qurdina/the-office-plugin
/plugin install the-office@the-office-plugins
```

Activate it:

```text
/reload-plugins
```

Restart Claude Code so the skill loads (type `exit`, then `claude` again).

That's it — there is **no separate `pip`/`pipx` step**. The plugin ships a
launcher (`bin/the-office`) that runs the published CLI on demand via
[`uv`](https://docs.astral.sh/uv/)'s `uvx` (falling back to `pipx`), fetching and
caching it on first use.

### Requirements

The launcher needs one of these on your machine to fetch and run the CLI:

- **uv** (recommended) — https://docs.astral.sh/uv/getting-started/installation/
- **pipx** — https://pipx.pypa.io/stable/installation/

## How it works

Once installed and enabled, the plugin does two things:

1. **Adds a skill** that tells the agent what The Office CLI is and when to use
   it. The skill is what triggers the agent to reach for the tool.
2. **Bundles a launcher** (`bin/the-office`) that the skill invokes by its
   plugin-relative path (`${CLAUDE_PLUGIN_ROOT}/bin/the-office`), so the agent
   can run the CLI without a separate install. Claude Code does not add the
   plugin's `bin/` to your shell `PATH`; the skill addresses the launcher
   directly.

Nothing runs automatically on install. When you ask the agent to do sales work,
the skill triggers and the agent runs the bundled launcher itself. The first
invocation pays a one-time fetch/cache cost via `uvx`; subsequent calls are fast.

> Note: this launcher is only used by the agent inside Claude Code. To use the
> CLI in your normal terminal, install it directly: `pipx install the-office-cli`
> (or `uvx the-office-cli`).

## Run it

Just ask the agent in natural language, e.g.:

> Find print shops in Scranton, PA on LinkedIn and draft invites to their owners.

The agent will discover commands and contracts on its own using the CLI's
introspection:

```text
the-office tour                 # list every command + description
the-office <command> --schema   # exact JSON input/output contract
the-office <command> --dry-run  # preview without executing
```

## Develop / test locally

Load the plugin straight from a local checkout without installing it from the
marketplace:

```bash
claude --plugin-dir /path/to/the-office-plugin
```

After editing plugin files, run `/reload-plugins` to pick up changes. Validate
the manifests with:

```bash
claude plugin validate .
```

## Layout

```text
the-office-plugin/
├── .claude-plugin/
│   ├── marketplace.json     # marketplace catalog (name: the-office-plugins)
│   └── plugin.json          # plugin manifest (name: the-office)
├── bin/
│   └── the-office           # install-free launcher (uvx → pipx fallback)
└── skills/
    └── the-office/
        └── SKILL.md          # tells the agent when/how to use the CLI
```

## Links

- The Office CLI: https://pypi.org/project/the-office-cli/
- Website: https://www.the-office.ai
- Claude Code plugins: https://code.claude.com/docs/en/plugins
