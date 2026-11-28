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

## What you can ask your AI to do in ClinicSense

These are real jobs ClinicSense users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in ClinicSense account.

### Clients and open slots

#### Can an AI agent find ClinicSense clients I have not seen in 60 days and send them a rebooking email?

Yes. With Cotlet, your agent reads the Clients and Calendar screens in your signed-in ClinicSense account, lists clients with no visit in 60 days and no next appointment, and drafts a rebooking email for each. Nothing is sent until you say yes.

Ask your agent: "List ClinicSense clients with no visit in 60 days and no next appointment. Draft a short rebooking email for each one."

As an API endpoint: Yes. Save the lapsed-client list as an endpoint in Cotlet and call it every month from n8n, Make or Zapier.

#### Can an AI agent watch my ClinicSense calendar, spot open slots this week and offer them to the right clients?

Partly. ClinicSense can share your upcoming availability by email on its own, under Communication. With Cotlet, your agent reads this week's open slots and drafts a note to the clients who usually book that time. Nothing is sent until you say yes.

Ask your agent: "Find my open ClinicSense slots this week and draft a short email offering them to clients who usually book at those times."

As an API endpoint: Yes. Save the open-slot check as an endpoint in Cotlet and call it every Sunday from n8n, Make or Zapier.

#### Can an AI agent block a problem client in ClinicSense and catch the same person booking again under a new name?

Partly. ClinicSense has No-Show Guard on each client, under More Options, to stop online booking or require a card. It does not match a new name to an old client. With Cotlet, your agent checks new bookings against the clients you blocked by phone, email and address. Nothing changes until you say yes.

Ask your agent: "Block this client from online booking in ClinicSense, then check this week's new bookings for the same phone number, email or address."

### Notes and intake forms

#### Can ChatGPT or Claude write my SOAP notes for a ClinicSense client from my voice memo or shorthand?

Yes. ClinicSense's own AI features are optional and can draft notes. With Cotlet, ChatGPT or Claude turns your shorthand or memo text into a SOAP note draft and enters it in the client's Treatment notes. Nothing is saved until you say yes.

Ask your agent: "Turn these shorthand session notes into a SOAP note draft for Zoe R. in ClinicSense. Show me the draft before you save it."

#### How do I make ClinicSense send the right intake form automatically for each service, for example lymphatic drainage?

Yes. ClinicSense sends forms automatically per service from Setup, then Form Builder, then Options. With Cotlet, your agent goes through your services and sets the right form on each one, for example a lymphatic drainage form. Nothing changes until you say yes.

Ask your agent: "List my ClinicSense services and which form each sends automatically. Propose the lymphatic drainage form for that service and show me before you change it."

### Money and reports

#### Can an AI agent build a monthly income report by service and by practitioner from ClinicSense and tell me why it does not match my bank?

Partly. ClinicSense's Reports include Revenue, Payments and Receivables. With Cotlet, your agent builds one table by service and practitioner and compares the payments with the bank list you give it, then lists the amounts that differ. You confirm the reason for each.

Ask your agent: "Build last month's ClinicSense income by service and by practitioner, then compare the payments with this bank list and list every amount that differs."

As an API endpoint: Yes. Save the income table as an endpoint in Cotlet and call it on the first of each month from n8n, Make or a script.

#### How do I export a monthly payments and tax report from ClinicSense for my accountant?

Yes. ClinicSense has Payments and Sales Tax reports under Reports. With Cotlet, your agent runs both for the month you name and gives you the figures as one file or sheet your accountant can open.

Ask your agent: "Run the ClinicSense Payments and Sales Tax reports for last month and give me one CSV my accountant can open."

As an API endpoint: Yes. Save the monthly run as an endpoint in Cotlet and call it from n8n, Make or Zapier on the first of each month.

#### How do I connect ClinicSense to QuickBooks so my payments and income land in my books without retyping?

Yes. ClinicSense's help names no QuickBooks connection. With Cotlet, your agent reads the month's payments in ClinicSense and enters the matching income in QuickBooks, both signed in in your Cloud Browser. Nothing is entered until you say yes.

Ask your agent: "Take last month's ClinicSense payments and enter them as income in QuickBooks. Show me the list before you enter anything."

As an API endpoint: Yes. Save the payment read as an endpoint in Cotlet that returns JSON, and pass it to QuickBooks from n8n or Make each week.

#### Can an AI agent chase my no-shows and late cancellations and add the fee in ClinicSense?

Yes. With Cotlet, your agent lists the no-shows and late cancellations in your ClinicSense calendar, drafts a message to each client and adds the fee on the appointment's invoice. Cotlet does not charge cards, and nothing is sent or added until you say yes.

Ask your agent: "List last week's ClinicSense no-shows and late cancellations, draft a message to each client and propose the fee for each invoice."

#### How do I sell a monthly membership in ClinicSense that charges the client automatically?

Not on its own. ClinicSense's help centre has no membership article. It has Treatment Packages and Gift Certificates under Sell. Cotlet does not take payments, so an automatic monthly charge has to come from elsewhere. Nothing is created until you say yes.

Ask your agent: "Create a ClinicSense treatment package called Monthly Massage with 1 session of 60 minutes. Show me the settings before you save it."

#### How do I fix gift card and online booking checkouts that spin for my clients in ClinicSense, and automate gift card sales?

Not on its own. Cotlet cannot repair a checkout that spins, and the help centre sends you to ClinicSense support for that. With Cotlet, your agent reads the Gift Certificates screen and lists what was sold, so you see which sales went through.

Ask your agent: "List the gift certificates sold in ClinicSense this month with buyer, amount and whether each has been used."

As an API endpoint: Yes. Save the gift certificate list as an endpoint in Cotlet and call it every week from n8n, Make or Zapier.

### Marketing and leads

#### How do I send my ClinicSense clients to a Mailchimp list and keep it updated?

Yes. ClinicSense sends its own email newsletters under Communication and has no Mailchimp connection. With Cotlet, your agent reads your clients in ClinicSense and adds new ones to your Mailchimp audience, both signed in in your Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Add every ClinicSense client created this month to my Mailchimp audience with name and email. Show me the list first."

As an API endpoint: Yes. Save the new-client read as an endpoint in Cotlet and call it every week from n8n or Make to update Mailchimp.

#### How do I get a Google Book now button on my Google Business listing that books into my ClinicSense calendar?

Partly. ClinicSense gives you an online booking page and an embed for your website. With Cotlet, your agent can add that booking link to your Google Business Profile, signed in in the same Cloud Browser. Whether Google shows a Book now button is up to Google. Nothing changes until you say yes.

Ask your agent: "Add my ClinicSense online booking link to my Google Business Profile as the booking link. Show me the change before you save it."

#### My Facebook ad sends people to my ClinicSense booking page. How do I see which ad produced a booking?

Partly. ClinicSense's Reports have no ad or booking source page. With Cotlet, your agent reads the month's new online bookings in ClinicSense and your results in Facebook Ads Manager, signed in in the same Cloud Browser, and lines them up by date, as far as the screens show.

Ask your agent: "List this month's new online bookings in ClinicSense and line them up by date with my Facebook ad results, so I can see which ad likely brought each one."

#### How do I connect an AI voice receptionist or an AI note scribe to ClinicSense?

Not on its own. ClinicSense's help centre shows no API, and third parties sell their own scribe and voice receptionist for it. Cotlet is not a voice receptionist or a scribe. With Cotlet, your agent can book what a call summary asks for and add the note. Nothing changes until you say yes.

Ask your agent: "Read this call summary, book the appointment it asks for in ClinicSense and add the note to the client's file. Show me before you save."

#### Can I get two-way texting with my clients on top of ClinicSense reminders, for example through a separate business number?

Not on its own. ClinicSense sends reminders, and its help centre shows no two-way text inbox. Cotlet does not send or receive texts itself. With Cotlet, your agent can read replies in the web inbox of your separate number and update the ClinicSense calendar. Nothing changes until you say yes.

Ask your agent: "Read today's replies in my business number's web inbox and list which clients want to move or cancel a ClinicSense appointment."

### Sheets, exports and imports

#### How do I get my ClinicSense appointments and payments into Google Sheets every week?

Yes. ClinicSense has no Zapier app. With Cotlet, your agent reads the week's appointments and payments from Reports in your signed-in account and adds them as rows to your Google Sheet. Nothing is added until you say yes.

Ask your agent: "Add this week's ClinicSense appointments and payments to my Google Sheet, one row each with client, service, practitioner and amount."

As an API endpoint: Yes. Save the weekly read as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier to fill the sheet or send a phone message.

#### How do I export all my clients, appointments and treatment notes from ClinicSense when I want to leave?

Partly. ClinicSense's Export Client List, on the Clients screen, gives the client list only. With Cotlet, your agent goes through appointments, invoices and treatment notes in your signed-in account and saves each as a file you keep.

Ask your agent: "Save every ClinicSense client, past appointment, invoice and treatment note to separate files before I cancel my plan."

#### How do I import my client list from MassageBook or Vagaro into ClinicSense without retyping each person?

Partly. ClinicSense has import guides for MassageBook, Vagaro and others, but it is not self-serve: you email your client list file to support. With Cotlet, your agent can add the clients one by one in ClinicSense from your file. Nothing is created until you say yes.

Ask your agent: "Take the clients in this file and add each one to ClinicSense with name, phone and email. Show me the first five before you start."

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

## More massage and wellness apps on Cotlet

- [Jane MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [Zanda MCP](https://github.com/cotlet-connect/zanda-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
