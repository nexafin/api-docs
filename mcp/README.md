---
description: >-
  Connect AI assistants like Claude and ChatGPT to your Nexafin financial data
  using the Model Context Protocol (MCP).
---

# MCP Overview

Nexafin supports the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP), an open standard that lets AI assistants securely use your financial data. Ask questions about your accounts, transactions, bills, spending patterns, and paydays — all through natural conversation.

{% hint style="info" %}
MCP access requires an active **PRO subscription**. Reading does not grant write access. Only `set_pay_schedule` can modify data, and it requires the separate `pay-schedule:write` OAuth scope.
{% endhint %}

## What you can do

* **Check balances** — "What's my net worth?" or "How much is in my savings account?"
* **Search transactions** — "What did I spend at Amazon last month?"
* **Track bills** — "What bills are due this week?"
* **Analyze spending** — "Show me my top spending categories this quarter"
* **Check or change payday** — "When is my next payday?" or "Move my payday back to automatic detection"

## Supported clients

Any MCP-compatible client works with Nexafin, including:

* [Claude Desktop](https://claude.ai/download)
* [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI)
* [ChatGPT](https://chatgpt.com/)
* Custom integrations via the MCP SDK

## Available tools

| Tool | Description |
|------|-------------|
| `get_account_balances` | Bank account balances and net worth |
| `get_transactions` | Recent transactions with filtering and search |
| `get_recurring_bills` | Upcoming bills, subscriptions, and recurring payments |
| `get_spending_by_category` | Spending patterns grouped by category |
| `get_pay_schedule` | Pay frequency and upcoming holiday-aware paydays |
| `set_pay_schedule` | Set, change, or reset payday; requires `pay-schedule:write` |

## Security

* **OAuth 2.1** authentication via WorkOS — your credentials never touch the AI client
* **Separate write scope** — reading never permits payday changes; `set_pay_schedule` requires `pay-schedule:write`
* **Sensitive fields excluded** — no account numbers, routing numbers, or card details are returned
* **Per-user rate limiting** — 60 requests per minute
* **Audit logging** — all requests are logged for your security
