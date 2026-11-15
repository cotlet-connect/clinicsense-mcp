# ClinicSense MCP server

Connect ClinicSense to ChatGPT, Claude and any AI agent through Cotlet.dev. Manage scheduling, wait lists, payments, reports and client emails in your signed-in ClinicSense account. The hosted integration excludes charting, SOAP notes, intake forms and client files. Write calls ask for approval by default.

## What you can ask

> Find clients on the wait list for tomorrow's cancelled slot.

> Show tomorrow's appointments for the whole team.

> Run the revenue report for last month.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings > Connectors > Add custom connector, URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its ClinicSense tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 clinicsense_agent.py serve
```

Or with Docker:

```bash
docker build -t clinicsense-mcp .
docker run -i --rm -e COTLET_API_KEY clinicsense-mcp
```

Tools: `clinicsense_guide`, `clinicsense_status`, `clinicsense_open`, `clinicsense_read`, `clinicsense_click`, `clinicsense_type`, `clinicsense_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 clinicsense_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 clinicsense_agent.py status    # is ClinicSense ready in your Cotlet.dev account
python3 clinicsense_agent.py guide     # ClinicSense screens and sign-in link
python3 clinicsense_agent.py read      # open ClinicSense, read the page, close the browser
python3 clinicsense_agent.py tools     # list Cotlet.dev tools
python3 clinicsense_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from ClinicSense users

**Can the agent read SOAP notes or intake forms?**

The hosted ClinicSense tools exclude charting, SOAP notes, intake and consent forms, client files and TELUS eClaims. They cover scheduling, payments, reports and client emails.

**Which ClinicSense plan do I need?**

Your ClinicSense plan must include the feature you use. A tool cannot add features outside your plan.

**Is this affiliated with ClinicSense?**

No. Cotlet.dev is unofficial, independent and not affiliated with ClinicSense.

## Learn more

- [ClinicSense integration and hosted tools](https://cotlet.dev/integrations/clinicsense-api-mcp-server)

---

More: https://cotlet.dev/integrations/clinicsense-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=clinicsense

Request access: https://cotlet.dev/signup?app=clinicsense&utm_source=github&utm_medium=readme&utm_campaign=clinicsense

## Workflows on the hosted integration

The live integration page documents these 16 operations:

- `list_appointments`
- `manage_appointments`
- `configure_online_booking`
- `configure_reminders`
- `configure_no_show_guard`
- `manage_wait_list`
- `manage_invoices`
- `manage_packages`
- `manage_gift_certificates`
- `run_financial_report`
- `send_newsletter`
- `configure_availability_summary`
- `configure_wellness_check_in`
- `manage_feedback`
- `configure_referral_email`
- `configure_birthday_email`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
