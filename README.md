# ClinicSense API integration and MCP server: connect ClinicSense to ChatGPT, Claude and any AI agent

ClinicSense API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in ClinicSense account: appointments, clients, intake forms, SOAP notes, packages, payments and reports, on the same screens your login opens. No developer and no ClinicSense API key needed. Cotlet is independent and is not affiliated with ClinicSense.

**Does ClinicSense have an API or an MCP server?** ClinicSense has no public API and no Zapier, Make or Pipedream app (checked 23 Nov 2026). Its built-in connections are Square payments, Google Calendar sync, TELUS Health eClaims in Canada and Google review requests. ClinicSense has no MCP server and no ChatGPT or Claude app.

## What massage therapists and wellness clinic owners ask their AI to do in ClinicSense

| Job | Ask your AI | Make it repeat |
|---|---|---|
| SOAP note drafts | "Turn my shorthand for today's 2 pm client into a SOAP note draft. I review it before it is saved." | After each session |
| Clients to rebook | "Find clients I have not seen in 60 days and draft a rebooking email to each. Send nothing yet." | Weekly |
| Open slots this week | "List my open slots this week and draft a short email offering them to clients who like those times." | Weekly |
| Income by service and practitioner | "Build last month's income by service and by practitioner, with tips and package sales." | Monthly |
| Payments for your accountant | "Export last month's payments and tax totals to a spreadsheet for my accountant." | Monthly |
| Where bookings came from | "List this month's online bookings with the page or campaign each one came from." | Monthly |
| No-shows and late cancels | "List this week's no-shows and late cancellations with the fee that applies, and draft a note to each client." | Weekly |
| Appointments in a spreadsheet | "Put each week's appointments and payments in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Import from MassageBook or Vagaro | "Import the clients in this MassageBook export into ClinicSense. Show me before you save." | Once |
| Leaving ClinicSense | "Export all my clients, appointments and treatment notes before I move to another app." | Once |

Your AI asks you before it emails or texts a client, saves a note or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own ClinicSense API, built with Cotlet

ClinicSense gives you no API and no Zapier app. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, QuickBooks, Mailchimp, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `appointments-for-week` | appointments for a week with client, service and practitioner |
| `open-slots` | free slots per practitioner for the days you name |
| `lapsed-clients` | clients with no visit for the days you name, with their last service |
| `income-for-period` | income by service and practitioner, with tips and package sales |
| `no-shows` | no-shows and late cancellations in a period |
| `add-client` | adds a client with phone, email and a note (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/appointments-for-week \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=clinicsense

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to ClinicSense once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

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

## Questions from ClinicSense therapists
**Does ClinicSense have an API or a Zapier app?**

No. ClinicSense has no public API and no Zapier, Make or Pipedream app (checked 23 Nov 2026). With Cotlet you build your own endpoints in your signed-in ClinicSense account and call them from Google Sheets, Zapier, Make or n8n.

**Can ChatGPT or Claude write my ClinicSense SOAP notes?**

Your AI turns your shorthand or voice memo into a SOAP note draft for the client you name. You read it and it is saved in ClinicSense only after your OK.

**Can an AI agent rebook clients I have not seen in a while?**

Yes. It lists clients with no visit for the days you choose and drafts a rebooking email to each. Nothing goes out until you say yes.

**How do I get ClinicSense payments into QuickBooks?**

ClinicSense names no QuickBooks integration. Your AI exports the month's payments and tax totals to a spreadsheet for your accountant or a QuickBooks import.

**How do I see which ad or page brought a ClinicSense booking?**

Your AI lists the month's online bookings with the page or campaign each came from, as far as your ClinicSense screens show it.

**How do I export everything from ClinicSense before I leave?**

Your AI exports clients, appointments and treatment notes from the screens your login opens, so you keep your records when you switch.

## Find it on

- [ClinicSense MCP server on Cotlet.dev](https://cotlet.dev/integrations/clinicsense-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=clinicsense&utm_content=find)
- [ClinicSense API with Cotlet](https://cotlet.dev/integrations/api/clinicsense?utm_source=github&utm_medium=readme&utm_campaign=clinicsense&utm_content=find)
- [Does ClinicSense have an API or MCP server?](https://cotlet.dev/api-mcp-status/clinicsense?utm_source=github&utm_medium=readme&utm_campaign=clinicsense&utm_content=find)
- [PyPI package clinicsense-mcp](https://pypi.org/project/clinicsense-mcp/)

## More massage and wellness apps on Cotlet

- [Jane MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [Zanda MCP](https://github.com/cotlet-connect/zanda-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
