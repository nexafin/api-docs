---
description: >-
  Technical details of the Nexafin MCP protocol implementation for developers
  building custom integrations.
---

# Protocol

## Endpoint

```
POST https://app.nexafin.com/mcp
```

All requests use the [JSON-RPC 2.0](https://www.jsonrpc.org/specification) protocol over HTTP.

## Supported protocol versions

* `2025-11-25`
* `2025-03-26`
* `2024-11-05`

## Rate limiting

Requests are rate-limited to **60 per minute** per user. When you exceed the limit, you'll receive a `429` response with:

* `Retry-After` header — seconds to wait before retrying
* `X-RateLimit-Limit` — maximum requests allowed
* `X-RateLimit-Remaining` — remaining requests in the current window

## Protocol flow

### 1. Initialize

Negotiate the protocol version with the server.

**Request:**

```json
{
  "jsonrpc": "2.0",
  "method": "initialize",
  "params": {
    "protocolVersion": "2025-11-25"
  },
  "id": 1
}
```

**Response:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "protocolVersion": "2025-11-25",
    "capabilities": {
      "tools": {
        "listChanged": false
      }
    },
    "serverInfo": {
      "name": "Nexafin",
      "version": "1.0.0"
    }
  },
  "id": 1
}
```

### 2. List tools

Discover available tools and their input schemas.

**Request:**

```json
{
  "jsonrpc": "2.0",
  "method": "tools/list",
  "id": 2
}
```

**Response:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "tools": [
      {
        "name": "get_account_balances",
        "description": "Use this when the user wants to see their bank account balances or net worth...",
        "inputSchema": {
          "type": "object",
          "properties": {
            "include_hidden": {
              "type": "boolean",
              "description": "Include hidden accounts in the response",
              "default": false
            }
          }
        },
        "annotations": {
          "readOnlyHint": true,
          "openWorldHint": false,
          "destructiveHint": false
        }
      }
    ]
  },
  "id": 2
}
```

### 3. Call a tool

Execute a tool with arguments.

**Request:**

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_transactions",
    "arguments": {
      "limit": 5,
      "date_from": "2026-03-01",
      "search": "coffee"
    }
  },
  "id": 3
}
```

**Response:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Found 2 transaction(s) totaling -$9.50:\n• Mar 7, 2026: Starbucks — -$5.50 (id:456)\n• Mar 3, 2026: Blue Bottle Coffee — -$4.00 (id:789)"
      }
    ],
    "isError": false
  },
  "id": 3
}
```

### 4. Ping

Health check for connection monitoring.

**Request:**

```json
{
  "jsonrpc": "2.0",
  "method": "ping",
  "id": 4
}
```

**Response:**

```json
{
  "jsonrpc": "2.0",
  "result": {},
  "id": 4
}
```
