---
description: >-
  API keys are used to authenticate requests to the Nexafin Public API.
---

# Authentication

## API Keys

You can create and manage API keys in the **Settings > API Keys** panel at [app.nexafin.com](https://app.nexafin.com).

API keys use the prefix `nxfn_sk_` and expire 1 year after creation. When a key expires, create a new one from the settings panel. The full key is only shown once at creation — store it securely.

### Using your API key

Include the key in the `Authorization` header as a Bearer token:

```bash
curl -X GET https://app.nexafin.com/v1/transactions \
  -H "Authorization: Bearer nxfn_sk_your_api_key_here" \
  -H "Accept: application/json"
```

{% hint style="warning" %}
**Required headers for all API requests:**
- `Authorization: Bearer YOUR_API_KEY`
- `Accept: application/json`
- `Content-Type: application/json` (for POST/PUT requests with a JSON body)

Without the `Accept: application/json` header, error responses will return HTML instead of JSON.
{% endhint %}

{% hint style="info" %}
The Public API is a pro accounts feature. If you don't have a pro account, API requests will return a 401 Unauthorized response.
{% endhint %}

## Legacy tokens

If you previously created JWT tokens, they will continue to work until they expire. New integrations should use API keys instead.
