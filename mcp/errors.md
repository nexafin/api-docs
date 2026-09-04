---
description: Error codes returned by the Nexafin MCP server.
---

# Error Codes

All errors follow the [JSON-RPC 2.0](https://www.jsonrpc.org/specification#error_object) error format:

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32001,
    "message": "Authentication required"
  },
  "id": 1
}
```

## Standard JSON-RPC errors

| Code | Name | Description |
|------|------|-------------|
| `-32700` | Parse error | Invalid JSON in the request body |
| `-32600` | Invalid request | The request is not a valid JSON-RPC request |
| `-32601` | Method not found | The requested method does not exist |
| `-32602` | Invalid params | Invalid tool arguments or missing required parameters |
| `-32603` | Internal error | Unexpected server error |

## Nexafin MCP errors

| Code | Name | HTTP Status | Description |
|------|------|:-----------:|-------------|
| `-32001` | Authentication error | 401 | Missing or invalid OAuth token |
| `-32002` | Authorization error | 403 | Token is valid but account is not linked to Nexafin |
| `-32003` | Tool execution error | 500 | The tool encountered an error during execution |
| `-32004` | Rate limit exceeded | 429 | Too many requests. Check the `Retry-After` header. |
| `-32005` | Subscription required | 403 | An active PRO subscription is required |

## HTTP status codes

| Status | Meaning |
|--------|---------|
| `200` | Success |
| `400` | Parse error or invalid request |
| `401` | Authentication required. Includes a `WWW-Authenticate` header. |
| `403` | Authorization error. Account not linked, subscription inactive, or subscription required. |
| `404` | Method not found, or MCP is disabled |
| `429` | Rate limit exceeded. Check the `Retry-After` header. |
| `500` | Internal server error |

## Connection troubleshooting

### Browser asks you to log in

This is expected when you do not have an active Nexafin login in that browser. Log in with the Nexafin account you want to connect. You will then be sent to the **Authorize application access** page, where you must approve or deny the request.

### You denied the authorization request

Selecting **Deny** clears the pending authorization and returns you to the Nexafin dashboard. The MCP client is not connected. When you are ready to approve it, start login again from the client. For Claude Code, run:

```bash
claude mcp login nexafin
```

### Authorization request expired

The approval page's pending authorization expires after 10 minutes. If approval fails with a message that the connection could not be completed, restart the connection from your MCP client. For Claude Code, rerun `claude mcp login nexafin`.

### OAuth token expired or is invalid

An expired OAuth token is rejected with HTTP `401` and an authentication error. Start OAuth login again so the MCP client can obtain valid credentials. For Claude Code, run `claude mcp login nexafin`.

### An API key or Authorization header was pasted into the client

Nexafin MCP does not accept Nexafin API keys or legacy web and mobile tokens. A manually supplied value can produce HTTP `401`, including `MCP requires OAuth authentication` for a recognized non-OAuth token.

Remove the custom header or API key from the MCP server configuration, keep only `https://app.nexafin.com/mcp` as the server URL, and complete the client's OAuth login. For Claude Code:

```bash
claude mcp login nexafin
```
