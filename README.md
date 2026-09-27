# Financy plugin for Grok

Read-only access to your Israeli bank and credit-card data — balances,
transactions and categories — from Grok Bot and Grok Build, through the hosted
[Financy](https://open-finance.ai) MCP server.

The plugin bundles:

| Component | What it does |
|---|---|
| MCP server `financy` | `https://mcp.open-finance.ai/mcp` — the hosted Financy server |
| Skill `financy-data-freshness` | Checks how current the data is before any answer |
| Skill `personal-spending-review` | Spending by category, subscriptions, anomalies, weekly summary |
| Skill `business-cashflow-review` | Inflows vs. outflows, suppliers, fixed expenses, monthly report |

## Requirements

- A [Financy](https://open-finance.ai) account with at least one bank or card
  connected.
- A paid Financy plan (Starter or Pro). The data API is not available on the
  free plan.

## Install

In Grok Bot: **Settings → Plugins → search "Financy" → Add**, then sign in with
your Financy account on the consent screen.

Before it is listed, you can add the server by URL: ask your Bot to "add the
MCP server `https://mcp.open-finance.ai/mcp`", confirm, and click
**Authorize**.

## Bot templates

Two ready-made Bots use this plugin. Their full configuration is in
[`bots/`](bots/):

- [Personal Finance Bot](bots/personal-finance-bot.md) — household spending,
  subscriptions and a weekly summary.
- [Business Finance Bot](bots/business-finance-bot.md) — cash flow, suppliers,
  fixed expenses and a monthly report for small businesses.

## Network endpoints and permissions

| Endpoint | Why |
|---|---|
| `https://mcp.open-finance.ai/mcp` | The MCP server. Every tool is annotated `readOnlyHint: true`. |
| `https://auth.open-finance.ai` | OAuth 2.1 sign-in. The client registers itself via Dynamic Client Registration or a Client ID Metadata Document; no client secret ships in this plugin. |

The plugin requests one OAuth scope, `mcp:read`. It cannot move money, create
or change bank connections, or trigger a paid data refresh. The OAuth token
stays with the Grok connector backend; the model never sees it.

This plugin contains no hooks, scripts, or executable code — only an MCP
server URL and Markdown skills.

## Tools

| Tool | Purpose |
|---|---|
| `get_status` | Per-connection freshness, consent expiry and account count |
| `list_connections` / `get_connection` | Bank and card connections and their fetch state |
| `list_accounts` / `get_account` | Accounts and balances (checking, card, loan, savings, securities) |
| `list_transactions` / `get_transaction` | Transactions, filterable by date, account, connection and type |
| `list_categories` | Transaction category taxonomy (English + Hebrew) |
| `list_providers` / `list_bank_branches` | Reference data |

## Privacy

Your financial data flows from Financy to your own Grok account only when you
ask a question. Financy's handling of your data is covered by the
[Open Finance privacy policy](https://open-finance.ai). The Bot templates tell
the Bot not to store balances or transactions in memory.

## License

[Apache-2.0](LICENSE)
