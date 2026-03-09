---
description: >-
  Nexafin MCP uses OAuth 2.1 via WorkOS for authentication. Most clients handle
  this automatically.
---

# Authentication

## OAuth 2.1 via WorkOS

Nexafin MCP implements the [OAuth 2.0 Protected Resource Metadata](https://datatracker.ietf.org/doc/html/rfc9728) standard (RFC 9728). MCP clients like Claude Desktop and ChatGPT discover the authorization server automatically via the discovery endpoint.

### Discovery endpoint

```
GET https://app.nexafin.com/.well-known/oauth-protected-resource
```

**Response:**

```json
{
  "resource": "https://app.nexafin.com",
  "authorization_servers": ["https://your-workos-issuer.authkit.app"],
  "scopes_supported": ["openid", "profile", "email", "offline_access"],
  "bearer_methods_supported": ["header"],
  "resource_documentation": "https://nexafin.com/docs/api"
}
```

### How it works

1. Your MCP client reads the discovery endpoint to find the authorization server (WorkOS)
2. You are redirected to WorkOS to log in with your Nexafin account
3. WorkOS issues an OAuth token to the MCP client
4. The client sends requests with `Authorization: Bearer <token>`

{% hint style="info" %}
Most MCP clients handle this flow automatically. You just need to provide the MCP endpoint URL and the client takes care of the rest.
{% endhint %}

## Public vs authenticated methods

Some MCP protocol methods do not require authentication:

| Method | Auth required | Description |
|--------|:---:|-------------|
| `initialize` | No | Protocol handshake |
| `notifications/initialized` | No | Client ready notification |
| `ping` | No | Connection health check |
| `tools/list` | No | List available tools |
| `tools/call` | **Yes** | Execute a tool |

## Error responses

| Status | Meaning |
|--------|---------|
| `401` | Missing or invalid token. Response includes a `WWW-Authenticate` header pointing to the OAuth discovery endpoint: `Bearer resource_metadata="https://app.nexafin.com/.well-known/oauth-protected-resource", scope="openid profile email offline_access"` |
| `403` | Token is valid but your Nexafin account isn't linked or set up yet. The error message includes a link to complete setup. |
