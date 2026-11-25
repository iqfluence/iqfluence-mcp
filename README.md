<p align="center">
  <img src="assets/icon.png" alt="IQFluence" width="96" height="96">
</p>

<h1 align="center">IQFluence MCP</h1>

<p align="center">
  Search 455M+ Instagram, TikTok and YouTube creators and analyze influencers from your AI assistant.
</p>

<p align="center">
  <a href="https://iqfluence.io/docs/mcp">Documentation</a> ·
  <a href="https://iqfluence.io">Website</a> ·
  <a href="mailto:support@iqfluence.io">Support</a>
</p>

<p align="center">
  <a href="https://glama.ai/mcp/connectors/io.iqfluence/mcp"><img src="https://glama.ai/mcp/connectors/io.iqfluence/mcp/badges/score.svg" alt="IQFluence MCP connector on Glama"></a>
</p>

IQFluence MCP is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI assistants
access to IQFluence influencer data: 455M+ Instagram, TikTok and YouTube creators. From Claude, ChatGPT, Codex,
Cursor, VS Code, Gemini CLI and other MCP clients you can find creators, open audience reports with fake-follower
checks, compare creators, check audience overlap, build media plans, read posts and comments, and see Campaign
Monitoring results.

The server is hosted by IQFluence, so there is nothing to install or run. This repository holds its public
description and ready-made client configs.

| | |
|---|---|
| Server URL | `https://mcp.iqfluence.io/mcp` |
| Transport | Streamable HTTP (remote) |
| Authentication | OAuth 2.1 in the browser; `Authorization: Bearer <API key>` for scripts and CI |
| Account | An IQFluence account is required. [Create one for free](https://iqfluence.io) |
| Registry | [`io.iqfluence/mcp`](https://registry.modelcontextprotocol.io/v0/servers?search=io.iqfluence/mcp) in the official MCP Registry |

## Connect

Every client uses the same server URL and signs in through the browser (OAuth): the assistant opens an IQFluence
window, you check the account and click **Allow access**.

### Claude (web, Desktop, mobile)

1. Open **Customize → Connectors**, click **Add → Add custom connector**.
2. Name: `IQFluence`, URL: `https://mcp.iqfluence.io/mcp`. Click **Add**, then **Connect**.

In Claude Team and Enterprise the organization owner adds the connector in **Organization settings → Connectors**,
then each member connects their own IQFluence account.

### Claude Code

```bash
claude mcp add --transport http --scope user iqfluence https://mcp.iqfluence.io/mcp
claude mcp login iqfluence
```

### ChatGPT

Custom MCP servers work on the ChatGPT website on Pro, Business, Enterprise and Edu plans with developer mode turned
on (**Settings → Apps → Advanced settings → Developer mode**).

- **Pro**: **Plugins → Add → Add custom MCP server**. Name `IQFluence`, URL `https://mcp.iqfluence.io/mcp`,
  authentication OAuth.
- **Business, Enterprise, Edu**: the workspace admin creates the app in **Workspace settings → Apps → Create** with
  the same URL and OAuth, then publishes it; employees connect their own IQFluence accounts.

### Codex

```bash
codex mcp add iqfluence --url https://mcp.iqfluence.io/mcp
```

### Cursor

Add to `~/.cursor/mcp.json` or `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "iqfluence": {
      "url": "https://mcp.iqfluence.io/mcp"
    }
  }
}
```

### VS Code (Copilot)

Run **MCP: Add Server → HTTP** from the Command Palette, or add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "iqfluence": {
      "type": "http",
      "url": "https://mcp.iqfluence.io/mcp"
    }
  }
}
```

### Gemini CLI

Install this repository as an extension:

```bash
gemini extensions install https://github.com/iqfluence/iqfluence-mcp
```

or add the server directly:

```bash
gemini mcp add --transport http iqfluence https://mcp.iqfluence.io/mcp
```

Then run `/mcp auth iqfluence` inside Gemini CLI.

### Cline

Open **MCP Servers → Configure → Remote Servers**, enter the name `iqfluence` and the URL
`https://mcp.iqfluence.io/mcp`, choose **Streamable HTTP** and click **Add Server**. Or add to
`cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "iqfluence": {
      "type": "streamableHttp",
      "url": "https://mcp.iqfluence.io/mcp",
      "disabled": false
    }
  }
}
```

Sign in through the browser when Cline asks to authorize the server, or use an API key header as shown below.

### API key instead of OAuth

For scripts and CI, create a key in **Profile → API keys** on iqfluence.io and send it as a header:

```bash
claude mcp add --transport http --scope user iqfluence https://mcp.iqfluence.io/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

## Tools

The assistant picks the right tool for the task. It states the cost of paid steps and asks first if a task costs
more than 5 credits.

| Tool | What it does | Cost |
|---|---|---|
| `search_influencers` | Finds Instagram, TikTok and YouTube creators by niche, location, size, engagement and audience demographics | First 3 results free; further rows come back hidden |
| `unlock_search_results` | Reveals hidden search rows | 0.02 credits per account |
| `lookup_filter_values` | Finds filter values: locations, interests, brands, languages, hashtags | Free |
| `get_influencer_report` | Full creator report: audience and its quality, engagement, growth, content, contacts | 1 credit; free if bought in the last 30 days |
| `compare_influencers` | Compares 2–5 creators | 1 credit per creator without a recent report |
| `build_media_plan` | Expected views, engagements, CPM and cost per engagement | 1 credit per creator without a recent report |
| `audience_overlap` | Shared audience and unique reach across creators | 1 credit per calculation |
| `get_contacts` | Public contacts: email, phone, other social accounts | 0.04 credits per account with contacts |
| `get_hashtag_posts` | Public posts with a hashtag | 0.02 credits per page |
| `get_creator_posts` | A creator's latest posts | 0.02 credits per page |
| `get_post` | One post's stats and content | 0.02 credits |
| `get_post_comments` | Comments under a post | 0.02 credits per page |
| `list_my_reports` | Reports bought in the last 60 days | Free |
| `list_campaigns` | Campaign Monitoring campaigns | Free |
| `get_campaign_stats` | Campaign results: views, engagement, budget, top posts | Free |
| `get_account_balance` | MCP balance, plan and today's spend for this connection | Free |

Reports, comparisons, media plans, overlaps, campaign results and the balance render as interactive cards in clients
that support MCP Apps (Claude, ChatGPT).

## Example prompts

- "Find 5 US food creators on Instagram: 50–200K followers, ER above 3%"
- "Analyze Instagram creator johnnyscookin: who is the audience?"
- "Compare johnnyscookin, chefsummerstorm and surelynotvegan for a US campaign"
- "Build a media plan for these three creators with fees of £500, £800 and £1,200"
- "How much do the audiences of @a and @b overlap?"
- "Show recent TikTok posts for #veganrecipes"
- "How is my Campaign Monitoring campaign doing?"

## Pricing

MCP requests spend MCP credits, a balance separate from website and API credits.

| | Starter | Pro |
|---|---|---|
| Credits per month | 300 | 500 |
| Monthly billing | $199/month | $299/month |
| Yearly billing (20% off) | $1,910.40 | $2,870.40 |

New accounts get 5 trial credits on the first connection, valid for 30 days. Top-ups cost $0.79 per credit, from 50
credits, with an active plan. Prices exclude VAT. See [iqfluence.io/docs/mcp](https://iqfluence.io/docs/mcp#mcp-credits-and-plans)
for details.

## Data and privacy

The server receives only what the assistant sends in a tool call: the tool name and its arguments. It does not
receive your conversation or files. Tool calls are processed on IQFluence infrastructure in the EU (Germany), are not
sent to AI models and are not used for training. Each connection appears as an "MCP · assistant name" key in
**Profile → API keys**, where you can set a daily credit limit or revoke access at once.

[Privacy Policy](https://iqfluence.io/public/privacy-policy) · [Terms of Service](https://iqfluence.io/public/terms)

## Support

Questions and issues: [support@iqfluence.io](mailto:support@iqfluence.io).

## License

The contents of this repository (documentation and configuration files) are available under the [MIT License](LICENSE).
The IQFluence service and its data are provided under the [IQFluence Terms of Service](https://iqfluence.io/public/terms).
