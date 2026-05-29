# Connect Revenoid to Claude — setup guide

Add the Revenoid MCP server as a **custom connector** in the Claude desktop
app. Takes ~1 minute. Two ways to authenticate: **OAuth** (sign in, no key to
copy) or an **API key**.

- **Server URL:** `https://core.revenoid.com/api/v2/mcp`

---

## Step 1 — Open Connectors

1. In the Claude app's left sidebar, click **Customize**.
2. In the Customize panel, select **Connectors**.

You'll see your existing connectors (GitHub, Gmail, Google Drive, …).

## Step 2 — Add a custom connector

1. Click the **+** button at the top-right of the Connectors list.
2. Choose **Add custom connector**.

A dialog titled **"Add custom connector"** (BETA) opens.

## Step 3 — Fill in the connector

| Field | Value |
|-------|-------|
| **Name** | `Revenoid` |
| **Remote MCP server URL** | `https://core.revenoid.com/api/v2/mcp` |

Leave **Advanced settings** closed for the OAuth path (see below). Then click
**Add**.

## Step 4 — Authenticate

Pick one.

### Option A — OAuth (recommended)
After clicking **Add**, Claude discovers Revenoid's authorization server and
opens a browser window:

1. You'll land on the **"Connect Revenoid to Claude"** consent screen.
2. Enter your **Revenoid email + password** (use the eye icon to check your
   password), then click **Authorize**.
3. The window closes and the connector shows as **connected**.

No key to copy or store — Claude caches and refreshes the token for you.

> Note: OAuth requires your Revenoid tenant to have the authorization server
> enabled. If you see a CORS or "not enabled" error, use Option B (API key)
> for now and check with your Revenoid admin.

### Option B — API key
If you'd rather use a key (or before OAuth is enabled), put the key in the URL
instead — the Claude connector dialog doesn't require custom headers:

1. In **Step 3**, set the **Remote MCP server URL** to:
   ```
   https://core.revenoid.com/api/v2/mcp?api_key=rvk_live_<your-key>
   ```
2. Click **Add**. It connects immediately — no browser sign-in.

Get a key from the Revenoid dashboard → **Settings → API Keys**. Treat it like
a password; anyone with it can act as you.

## Step 5 — Use it

Once connected, the connector lists Revenoid's tools. Start a chat and ask for
sales work in plain language — Claude routes it through Revenoid:

- "Find me 10 cybersecurity accounts and the VPs of Eng at each."
- "Research Snowflake and draft a cold email to their CRO in my voice."
- "Who at Acme have we talked to? Pull the call summaries."
- "Push these enriched contacts to my CRM."

You can set per-tool permissions (Allow / Ask / Deny) in the connector's
**Tool permissions** panel.

---

## Tools you get

`get_company_info`, `find_accounts`, `find_prospects`, `find_person`,
`enrich_contacts`, `research_account`, `lookup_company` / `lookup_person` /
`lookup_email` / `lookup_domain`, `lookup_linkedin_posts`, `find_job_postings`,
`search_call_transcripts`, `crm_query` / `crm_push_contacts` /
`crm_describe_objects` / `crm_describe_fields`, `get_calendar_events` /
`get_calendar_stakeholders`, `list_messaging_agents`, `generate_message`,
`list_saved_sequences`, `list_icp_settings`, `get_credits`, and the
`revenoid_workflow` orchestrator — among others.

## Troubleshooting

| Symptom | Cause / fix |
|---------|-------------|
| `{"...":"Not allowed by CORS"}` or "not enabled" on the consent page | The tenant's OAuth server isn't live yet — use the **API-key** URL (Option B), or have your admin enable it. |
| Connector connects but lists no tools | Re-open the connector and reconnect; confirm the URL is exactly `https://core.revenoid.com/api/v2/mcp`. |
| "Invalid email or password" on the consent screen | Your Revenoid login. SSO-only accounts can't use the password screen yet — use Option B. |
| Browser sign-in never returns | Make sure pop-ups/redirects from `core.revenoid.com` aren't blocked. |

## Other ways to connect

- **Claude Code (CLI):** `claude mcp add --transport http revenoid https://core.revenoid.com/api/v2/mcp`
- **As a plugin:** see [`README.md`](./README.md) for the `/plugin marketplace add` flow.
