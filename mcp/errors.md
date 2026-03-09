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
| `-32004` | Rate limit exceeded | 429 | Too many requests — check `Retry-After` header |
| `-32005` | Subscription required | 403 | An active PRO subscription is required |

## HTTP status codes

| Status | Meaning |
|--------|---------|
| `200` | Success |
| `400` | Parse error or invalid request |
| `401` | Authentication required — includes `WWW-Authenticate` header |
| `402` | Subscription required |
| `403` | Authorization error (account not linked or subscription inactive) |
| `404` | Method not found, or MCP is disabled |
| `429` | Rate limit exceeded — check `Retry-After` header |
| `500` | Internal server error |
