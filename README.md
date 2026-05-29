# Revenoid MCP — Claude Code plugin

Connect [Revenoid](https://revenoid.com) to Claude Code (and any MCP-speaking
client) as a one-command plugin. It exposes Revenoid's sales toolset —
discover accounts, find and enrich prospects, query your CRM, search call
transcripts, and generate outreach in your own voice — without leaving the
editor.

This repo is both the **plugin** and its **marketplace** (single-plugin
marketplace, plugin at the repo root).

> **Using the Claude desktop app?** See [`SETUP.md`](./SETUP.md) for the
> point-and-click "Add custom connector" walkthrough (Customize →
> Connectors → +). The plugin/CLI instructions below are for Claude Code.

## Install

```bash
# 1. add this repo as a marketplace
/plugin marketplace add Revenoid-Inc/revenoid-mcp-plugin

# 2. install the plugin
/plugin install revenoid-mcp@revenoid

# 3. authenticate (OAuth — opens your browser to the Revenoid consent screen)
/mcp
```

`@revenoid` is the marketplace `name` (from `.claude-plugin/marketplace.json`);
`revenoid-mcp` is the plugin `name` (from `.claude-plugin/plugin.json`).

## Authentication

The bundled `.mcp.json` points at the remote server and uses **OAuth 2.1** by
default — no secret is shipped in this repo. On first use, run `/mcp` in Claude
Code; it discovers the auth server (RFC 9728 / RFC 8414), registers itself
(Dynamic Client Registration), opens the Revenoid consent screen, and caches +
refreshes the token automatically.

### Prefer an API key (or testing before OAuth is enabled on your tenant)?

Swap `.mcp.json` for the header form and export your key:

```json
{
  "mcpServers": {
    "revenoid": {
      "type": "http",
      "url": "https://core.revenoid.com/api/v2/mcp",
      "headers": { "Authorization": "Bearer ${REVENOID_MCP_KEY}" }
    }
  }
}
```

```bash
export REVENOID_MCP_KEY="rvk_live_<your-key>"
```

Get a key from the Revenoid dashboard (Settings → API Keys). Don't commit a
real key to a public repo — the `${REVENOID_MCP_KEY}` form keeps it out of git.

## Add it directly (no plugin)

If you just want it on one machine without the plugin/marketplace flow:

```bash
# OAuth
claude mcp add --transport http revenoid https://core.revenoid.com/api/v2/mcp
/mcp

# or with an API key
claude mcp add --transport http revenoid https://core.revenoid.com/api/v2/mcp \
  --header "Authorization: Bearer rvk_live_<key>"
```

## What you get

A toolset mirroring the Revenoid AI Workspace, including: `find_accounts`,
`find_prospects`, `enrich_contacts`, `research_account`, `crm_query`,
`crm_push_contacts`, `search_call_transcripts`, `generate_message`,
`revenoid_workflow`, and more.

## Notes

- The server is remote (`https://core.revenoid.com/api/v2/mcp`, Streamable
  HTTP) — nothing runs locally.
- If a Claude Code version expects `"type": "streamable-http"` instead of
  `"http"` for remote servers, update `.mcp.json` accordingly; both are
  accepted by current versions.
- OAuth requires the Revenoid tenant to have the authorization server enabled.
  Until then, use the API-key form above.

## License

© Revenoid. All rights reserved.
