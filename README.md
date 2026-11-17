# Obat MCP server

Connect Obat to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own Obat account with 14 hosted tools for quotes, invoices, reminders, clients and job sites. The three tools that email a client ask for approval by default.

## What you can ask

> List my overdue invoices.

> Draft a deposit invoice from a signed quote and show me the totals.

> List my clients and their job sites.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Obat tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 obat_agent.py serve
```

Or with Docker:

```bash
docker build -t obat-mcp .
docker run -i --rm -e COTLET_API_KEY obat-mcp
```

Tools: `obat_guide`, `obat_status`, `obat_open`, `obat_read`, `obat_click`, `obat_type`, `obat_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 obat_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 obat_agent.py status    # is Obat ready in your Cotlet.dev account
python3 obat_agent.py guide     # Obat screens and sign-in link
python3 obat_agent.py read      # open Obat, read the page, close the browser
python3 obat_agent.py tools     # list Cotlet.dev tools
python3 obat_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Obat users

**Which Obat account should I connect?**

Connect your own Obat account at https://www.obat.fr/login in the Cotlet Cloud Browser.

**Which actions ask for approval?**

send_quote, send_invoice and send_invoice_reminder ask for approval by default. The other four write tools save drafts, clients or job sites, and the seven read tools run immediately.

**Is Cotlet affiliated with Obat?**

No. Cotlet.dev is independent and not affiliated with Obat.

## Learn more

- [Obat integration and hosted tools](https://cotlet.dev/integrations/obat-api-mcp-server)

---

More: https://cotlet.dev/integrations/obat-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=obat

Request access: https://cotlet.dev/signup?app=obat&utm_source=github&utm_medium=readme&utm_campaign=obat

## Workflows on the hosted integration

The live integration page documents these 14 operations:

- `list_quotes`
- `get_quote`
- `create_quote_draft`
- `send_quote`
- `list_invoices`
- `get_invoice`
- `create_invoice_from_quote`
- `send_invoice`
- `get_receivables_summary`
- `send_invoice_reminder`
- `list_clients`
- `list_sites`
- `create_client`
- `create_site`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
