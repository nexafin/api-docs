---
description: Get started with Nexafin MCP in ChatGPT or Claude.
---

# Quick Start

## ChatGPT

ChatGPT supports MCP servers natively. Add Nexafin as an MCP connection:

1. Open ChatGPT Settings
2. Navigate to **Connected apps** or **MCP Servers**
3. Add a new server with URL: `https://app.nexafin.com/mcp`
4. Authorize with your Nexafin account when prompted

## Claude

### Claude Code (CLI)

```bash
claude mcp add --transport http nexafin https://app.nexafin.com/mcp
```

You will be prompted to authorize via OAuth the first time you use a Nexafin tool.

### Claude Desktop

You can connect Nexafin to Claude Desktop using the built-in Connectors UI or by editing the config file.

#### Option A: Connectors UI (recommended)

1. Open Claude Desktop and go to **Settings → Connectors**
2. Click the **+** button and select **Add custom connector**

<figure><img src="images/claude-desktop-add-connector.png" alt="Claude Desktop Add custom connector menu"><figcaption></figcaption></figure>

3. Enter the connector name (`Nexafin`) and URL (`https://app.nexafin.com/mcp`), then click **Add**

<figure><img src="images/claude-desktop-custom-connector.png" alt="Claude Desktop custom connector form with Nexafin URL"><figcaption></figcaption></figure>

4. When you first use a Nexafin tool, Claude Desktop will ask for permission

<figure><img src="images/claude-desktop-tool-permission.png" alt="Claude Desktop tool permission prompt"><figcaption></figcaption></figure>

#### Option B: Config file

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
MCP access requires an active PRO subscription. If you see a subscription error, visit [app.nexafin.com](https://app.nexafin.com) to upgrade.
{% endhint %}
