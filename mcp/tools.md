---
description: Reference for all available Nexafin MCP tools.
---

# Tools Reference

All tools are **read-only** and never return sensitive data such as account numbers, routing numbers, or card details.

## get\_account\_balances

Returns bank account balances and net worth for all linked accounts.

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `include_hidden` | boolean | No | `false` | Include hidden accounts in the response |

### Example

**Request:**

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_account_balances",
    "arguments": {}
  },
  "id": 1
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
        "text": "Your account balances:\n• Primary Checking (id:1, checking): $8,500.00\n• Savings (id:2, savings): $25,000.00\n\nTotal net worth: $33,500.00"
      }
    ],
    "isError": false
  },
  "id": 1
}
```

---

## get\_transactions

Returns recent transactions with filtering and search. Defaults to the last 30 days if no date filters are provided.

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `currency` | string | No | `"USD"` | Currency for amount conversion (e.g., USD, EUR) |
| `limit` | integer | No | `20` | Maximum transactions to return (1–20) |
| `category_id` | integer | No | — | Filter by category ID |
| `bank_account_id` | integer | No | — | Filter by bank account ID |
| `date_from` | string (date) | No | 30 days ago | Start date (YYYY-MM-DD) |
| `date_to` | string (date) | No | today | End date (YYYY-MM-DD) |
| `search` | string | No | — | Search in transaction description or merchant name |
| `type` | string | No | — | Filter by type: `"income"` or `"expense"` |

### Example

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_transactions",
    "arguments": {
      "limit": 5,
      "type": "expense",
      "date_from": "2026-03-01"
    }
  },
  "id": 1
}
```

---

## get\_recurring\_bills

Returns upcoming bills, subscriptions, and recurring payments.

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `status` | string | No | `"active"` | Filter by status: `"active"` or `"all"` (includes inactive and archived) |
| `include_next_instance` | boolean | No | `true` | Include information about the next upcoming bill instance |

### Example

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_recurring_bills",
    "arguments": {
      "status": "active",
      "include_next_instance": true
    }
  },
  "id": 1
}
```

---

## get\_spending\_by\_category

Returns aggregated spending totals grouped by category for a date range.

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `date_from` | string (date) | No | 30 days ago | Start date for the analysis period (YYYY-MM-DD) |
| `date_to` | string (date) | No | today | End date for the analysis period (YYYY-MM-DD) |
| `top` | integer | No | `10` | Return only the top N categories by spending (1–20) |

### Example

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_spending_by_category",
    "arguments": {
      "date_from": "2026-01-01",
      "date_to": "2026-03-09",
      "top": 5
    }
  },
  "id": 1
}
```
