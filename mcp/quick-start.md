---
description: Get started with Nexafin MCP in Claude Desktop or ChatGPT.
---

# Quick Start

## Claude Desktop

Add the following to your Claude Desktop MCP configuration file:

{% tabs %}
{% tab title="macOS" %}
Edit `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "nexafin": {
      "url": "https://app.nexafin.com/mcp"
    }
  }
}
```
{% endtab %}

{% tab title="Windows" %}
Edit `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "nexafin": {
      "url": "https://app.nexafin.com/mcp"
    }
  }
}
```
{% endtab %}
{% endtabs %}

Restart Claude Desktop. You will be prompted to authorize Nexafin via OAuth the first time you use an MCP tool.

## ChatGPT

ChatGPT supports MCP servers natively. Add Nexafin as an MCP connection:

1. Open ChatGPT Settings
2. Navigate to **Connected apps** or **MCP Servers**
3. Add a new server with URL: `https://app.nexafin.com/mcp`
4. Authorize with your Nexafin account when prompted

## Your first request

Once connected, just ask your AI assistant a question:

> "What are my account balances?"

The assistant will call the `get_account_balances` tool and return your balances in a readable format.

### Example conversation

> **You:** What did I spend on food this month?
>
> **Assistant:** _(calls `get_transactions` with type: "expense", search: "food")_
>
> Let me check your recent food-related expenses...
>
> Here are your food transactions this month:
> - Mar 7: Whole Foods — $85.20
> - Mar 5: DoorDash — $32.50
> - Mar 3: Trader Joe's — $67.80
>
> Total: $185.50 on food this month.

{% hint style="warning" %}
MCP access requires an active PRO subscription. If you see a subscription error, visit [app.nexafin.com/settings/billing](https://app.nexafin.com/settings/billing) to upgrade.
{% endhint %}
