---
description: >-
  Access your Nexafin financial data through our REST API or connect AI
  assistants via MCP.
---

# Introduction

Nexafin offers two ways to access your financial data programmatically:

## Public API

A REST API for building custom integrations, dashboards, and automations with your bank accounts and transactions. Authenticate with API keys and access endpoints for transactions, bank accounts, categories, and balances.

{% hint style="info" %}
We are working to extend the public API to add more options and capabilities. If you need a specific endpoint, [email us](mailto:support@nexafin.com).
{% endhint %}

**Available endpoints:**

* [x] Update, delete and list transactions
* [x] Create transactions
* [x] List categories
* [x] List, edit, create and delete bank accounts
* [x] Accounts balances

[Get started with the Public API →](generate-a-token.md)

## MCP (Model Context Protocol)

Connect AI assistants like Claude and ChatGPT to your Nexafin data. Ask questions about your accounts, transactions, bills, and spending patterns through natural conversation. Authenticates via OAuth 2.1 — no API keys needed.

**Available tools:**

* [x] Account balances and net worth
* [x] Transaction search and filtering
* [x] Recurring bills and subscriptions
* [x] Spending analysis by category

[Get started with MCP →](mcp/README.md)
