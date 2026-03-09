---
description: >-
  Connect AI assistants like Claude and ChatGPT to your Nexafin financial data
  using the Model Context Protocol (MCP).
---

# MCP Overview

Nexafin supports the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP), an open standard that lets AI assistants securely read your financial data. Ask questions about your accounts, transactions, bills, and spending patterns — all through natural conversation.

{% hint style="info" %}
MCP access requires an active **PRO subscription**. All tools are **read-only** — no modifications to your data.
{% endhint %}

## What you can do

* **Check balances** — "What's my net worth?" or "How much is in my savings account?"
* **Search transactions** — "What did I spend at Amazon last month?"
* **Track bills** — "What bills are due this week?"
* **Analyze spending** — "Show me my top spending categories this quarter"

## Supported clients

Any MCP-compatible client works with Nexafin, including:

* [Claude Desktop](https://claude.ai/download)
* [ChatGPT](https://chatgpt.com/)
* Custom integrations via the MCP SDK

## Available tools

| Tool | Description |
|------|-------------|
| `get_account_balances` | Bank account balances and net worth |
| `get_transactions` | Recent transactions with filtering and search |
| `get_recurring_bills` | Upcoming bills, subscriptions, and recurring payments |
| `get_spending_by_category` | Spending patterns grouped by category |

## Security

* **OAuth 2.1** authentication via WorkOS — your credentials never touch the AI client
* **Read-only access** — no account numbers, routing numbers, or card details are ever returned
* **Per-user rate limiting** — 60 requests per minute
* **Audit logging** — all requests are logged for your security
