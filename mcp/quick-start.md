---
description: Get started with Nexafin MCP in Claude or ChatGPT.
---

# Quick Start

## Claude

### Claude Code (CLI)

1. Add the Nexafin MCP server:

```bash
claude mcp add --transport http nexafin https://app.nexafin.com/mcp
```

2. Start the OAuth login separately:

```bash
claude mcp login nexafin
```

3. Claude Code opens your browser. Log in to Nexafin if needed. Nexafin then shows an **Authorize application access** page explaining the requested read access and, when requested, payday write access. Select **Approve** only if you started the connection and want to grant those permissions.

4. After approval, the browser returns control to Claude Code and the connection is ready.

If no browser opens, rerun the login with `claude mcp login --no-browser nexafin`. Claude Code prints the authorization URL instead. Open it in a browser, complete authorization, and paste the redirect URL back into the terminal when prompted.

{% hint style="danger" %}
Do not add an API key or set an `Authorization` header. Nexafin MCP accepts OAuth tokens only. `claude mcp login nexafin` obtains and manages the token for you.
{% endhint %}

### Claude Desktop

You can connect Nexafin to Claude Desktop using the built-in Connectors UI or by editing the config file.

#### Option A: Connectors UI (recommended)

1. Open Claude Desktop and go to **Settings → Connectors**, then click **Add custom connector**

<figure><img src="images/claude-desktop-settings-connectors.png" alt="Claude Desktop Settings, Connectors page"><figcaption></figcaption></figure>

2. Or from the **Customize** panel, click the **+** button next to Connectors

<figure><img src="images/claude-desktop-connectors-plus.png" alt="Claude Desktop Customize, Connectors + button"><figcaption></figcaption></figure>

3. Enter the connector name (`Nexafin`) and URL (`https://app.nexafin.com/mcp`), then click **Add**

<figure><img src="images/claude-desktop-custom-connector.png" alt="Claude Desktop custom connector form with Nexafin URL"><figcaption></figcaption></figure>

4. After connecting, you can manage tool permissions. Five tools read data. `set_pay_schedule` also needs the separate `pay-schedule:write` OAuth scope.

<figure><img src="images/claude-desktop-tool-permissions.png" alt="Claude Desktop Nexafin tool permissions"><figcaption></figcaption></figure>

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

## ChatGPT

ChatGPT supports MCP servers natively. Add Nexafin as an MCP connection:

1. Open ChatGPT Settings and go to **Apps**

<figure><img src="images/chatgpt-settings-apps.png" alt="ChatGPT Settings, Apps page"><figcaption></figcaption></figure>

2. Click **Create app**, enter the name (`Nexafin`), the MCP Server URL (`https://app.nexafin.com/mcp`), select **OAuth** for authentication, and check the risk acknowledgment

<figure><img src="images/chatgpt-new-app.png" alt="ChatGPT New App form with Nexafin MCP URL"><figcaption></figcaption></figure>

3. Click **Create** and authorize with your Nexafin account when prompted

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
> - Mar 7: Whole Foods, $85.20
> - Mar 5: DoorDash, $32.50
> - Mar 3: Trader Joe's, $67.80
>
> Total: $185.50 on food this month.

{% hint style="warning" %}
MCP access requires an active PRO subscription. If you see a subscription error, visit [app.nexafin.com](https://app.nexafin.com) to upgrade.
{% endhint %}
