---
description: >-
  Nexafin MCP uses OAuth via WorkOS, proxied through app.nexafin.com. MCP
  clients manage the tokens for you.
---

# Authentication

## OAuth via WorkOS

Nexafin MCP implements [OAuth 2.0 Protected Resource Metadata](https://datatracker.ietf.org/doc/html/rfc9728) (RFC 9728). WorkOS is the authorization server behind the connection, but Nexafin proxies OAuth discovery, registration, authorization, token, user information, and signing-key traffic through `https://app.nexafin.com`.

### Discovery endpoint

```
GET https://app.nexafin.com/.well-known/oauth-protected-resource
```

**Response:**

```json
{
  "resource": "https://app.nexafin.com",
  "authorization_servers": ["https://app.nexafin.com"],
  "scopes_supported": ["openid", "profile", "email", "offline_access", "pay-schedule:write"],
  "bearer_methods_supported": ["header"],
  "resource_documentation": "https://nexafin.com/docs/api"
}
```

### How it works

1. The MCP client receives a `401` response with a `WWW-Authenticate` header that points to Nexafin's protected-resource metadata, or reads that metadata during setup.
2. The metadata advertises `https://app.nexafin.com` as the authorization server. The client reads OAuth or OpenID discovery metadata from that domain. Nexafin fetches the upstream WorkOS metadata and rewrites the supported endpoint URLs to `app.nexafin.com`.
3. The client starts the authorization-code flow through `https://app.nexafin.com/oauth2/authorize`. Nexafin forwards the authorization request to WorkOS.
4. The browser reaches Nexafin's login flow. If you are not already signed in, Nexafin asks you to log in before continuing.
5. Nexafin shows an **Authorize application access** page asking you to approve or deny connecting the external application. A read-only request says **It cannot change your data.** If the client requested `pay-schedule:write`, it says **It can change your payday settings.** Select **Approve** only if you started the connection and want the access shown.
6. After Nexafin completes authentication, WorkOS handles OAuth consent. New connections request only the standard read scopes by default. If the client explicitly requested `pay-schedule:write`, WorkOS includes that additional permission in its consent step; the permission is granted only when you approve it.
7. WorkOS returns the browser to the MCP client. Denying on Nexafin's page clears the pending authentication and returns you to the Nexafin dashboard.
8. The client exchanges the authorization code through `https://app.nexafin.com/oauth2/token`. WorkOS issues the OAuth token, and the client sends it to the MCP endpoint as a bearer token.

The pending approval is valid for 10 minutes. If it expires, restart login from your MCP client.

{% hint style="info" %}
The MCP client manages the bearer token. Do not paste an API key or manually configure an `Authorization` header. Nexafin rejects legacy web or mobile tokens and accepts WorkOS OAuth tokens for MCP.
{% endhint %}

## Public vs authenticated methods

Some MCP protocol methods do not require authentication:

| Method | Auth required | Description |
|--------|:---:|-------------|
| `initialize` | No | Protocol handshake |
| `notifications/initialized` | No | Client ready notification |
| `ping` | No | Connection health check |
| `tools/list` | **Yes** | List available tools |
| `tools/call` | **Yes** | Execute a tool |

## Error responses

| Status | Meaning |
|--------|---------|
| `401` | Missing or invalid token. Response includes a `WWW-Authenticate` header pointing to the OAuth discovery endpoint: `Bearer resource_metadata="https://app.nexafin.com/.well-known/oauth-protected-resource", scope="openid profile email offline_access"` |
| `403` | Token is valid but your account is not linked, or `set_pay_schedule` was called without `pay-schedule:write`. A missing write scope includes `WWW-Authenticate: Bearer error="insufficient_scope", scope="pay-schedule:write", resource_metadata="https://app.nexafin.com/.well-known/oauth-protected-resource"` so compatible clients can request the additional permission. |

The `401` challenge intentionally lists only `openid profile email offline_access`, the read-only default. The discovery document advertises `pay-schedule:write` as supported, but does not request it. The `get_pay_schedule` tool needs only the standard read scopes. The `set_pay_schedule` tool requires `pay-schedule:write`; having read access never implies it.
