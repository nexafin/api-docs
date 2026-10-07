---
description: Reference for all available Nexafin MCP tools.
---

# Tools Reference

Five tools are read-only. `set_pay_schedule` is the only write tool and requires the separate `pay-schedule:write` OAuth scope. Tools never return sensitive data such as account numbers, routing numbers, or card details.

Totals come out in your display currency. See [How totals are converted](#how-totals-are-converted).

## get\_account\_balances

Returns bank account balances for all linked accounts, with a total in your display currency. Each balance is converted at the latest rate before it is added. Each account line shows the account's own currency.

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
        "text": "Found 2 bank account(s) with total balance of $33,500.00:\n• Primary Checking (id:1, checking): $8,500.00\n• Savings (id:2, savings): $25,000.00"
      }
    ],
    "isError": false
  },
  "id": 1
}
```

---

## get\_transactions

Returns recent transactions with filtering and search. Defaults to the last 30 days if no date filters are provided. The total is in your display currency, unless you pass `currency`. Each row converts at the rate for its booked date. A row with no rate stays in the list in its own currency and is left out of the total. See [Amounts left out of the total](#amounts-left-out-of-the-total).

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `currency` | string | No | Your display currency | Currency for amount conversion (e.g., USD, EUR) |
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

Returns upcoming bills, subscriptions, and recurring payments, with an estimated monthly total in your display currency. Each bill is converted at the latest rate before it is added. Each bill line shows the bill's own currency.

The monthly total uses each bill's billing cycle:

| Cycle | Counted in the monthly total as |
|-------|---------------------------------|
| Daily | amount × 30 |
| Weekly | amount × 4 |
| Bi-weekly | amount × 2 |
| Monthly | amount |
| Quarterly | amount ÷ 3 |
| Semi-annual | amount ÷ 6 |
| Annually | amount ÷ 12 |

A bill with no billing cycle, or an unknown one, is shown as `unknown cycle` and left out of the total.

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

Returns aggregated spending totals grouped by category for a date range. Each row is converted at the rate for its booked date, then grouped. The category lines and the total are in your display currency. Amounts with no rate are the exception. See [Amounts left out of the total](#amounts-left-out-of-the-total).

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

---

## get\_pay\_schedule

Returns the user's pay frequency and upcoming holiday-aware paydays. The response distinguishes the nominal recurring date from the adjusted deposit date and names the holiday or weekend that caused a shift. It uses the user country, then the schedule's bank country, then a weekends-only fallback.

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|:--------:|---------|-------------|
| `count` | integer | No | `3` | Upcoming paydays to return (1–12) |

### Example

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_pay_schedule",
    "arguments": {"count": 3}
  },
  "id": 1
}
```

The text content is JSON with the same `state`, `schedule`, and `upcoming` fields documented for [`GET /v1/pay-schedule`](../reference/pay-schedule.md#get-pay-schedule). When no schedule exists, it returns `state: "not_detected"`, a reason, and the Settings URL instead of an empty list.

---

## set\_pay\_schedule

Sets or changes the same payday override used by Settings. It requires the `pay-schedule:write` OAuth scope. Automatic detection does not replace an override until it is reset.

To set an override, pass:

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `pattern` | string | Yes | `weekly`, `biweekly`, `semimonthly`, or `monthly` |
| `next_date` | string (date) | Yes | Next payday in `YYYY-MM-DD`; cannot be before today in the user's time zone |
| `non_business_shift` | string | No | `before`, `on`, or `after` |

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "set_pay_schedule",
    "arguments": {
      "pattern": "biweekly",
      "next_date": "2026-12-24",
      "non_business_shift": "before"
    }
  },
  "id": 1
}
```

To remove the override and return to automatic detection, pass only `automatic`:

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "set_pay_schedule",
    "arguments": {"automatic": true}
  },
  "id": 1
}
```

`automatic` cannot be combined with schedule fields. Both operations affect only the user represented by the OAuth token.

---

## How totals are converted

Totals are in your display currency. This applies to `get_account_balances`, `get_recurring_bills`, `get_spending_by_category` and `get_transactions`. For `get_transactions`, passing `currency` picks another currency.

### Last known rate

When there is no current rate for a currency, the tool uses a stored rate instead. It adds one line that names each currency and the date of the stored rate.

`get_account_balances` and `get_recurring_bills` use the latest stored rate that has both currencies. `get_transactions` and `get_spending_by_category` use the latest stored rate on or before each row's booked date, or the earliest later one if none is earlier.

For `get_account_balances` and `get_recurring_bills`:

```text
Converted at the last known rate, no current rate to EUR: CAD rate from 2026-10-04
```

For `get_transactions` and `get_spending_by_category`, which convert at each row's booked date:

```text
Converted at the last known rate, no rate to EUR for the booked date: CAD rate from 2026-10-04
```

If more than one date was used, the dates are listed after `rate from`, separated by commas. If more than one currency was used, the entries are separated by semicolons.

### Amounts left out of the total

If no stored rate has both currencies, the amounts cannot be converted. They are left out of the total and named at the end of the output, in their own currency. In `get_transactions`, the row also stays in the list, in its own currency. The tool call does not fail.

| Tool | Line |
|------|------|
| `get_account_balances` | `Not included in the total, no exchange rate to EUR: Savings (id:2, savings): XYZ 100.00` |
| `get_transactions` | `Not included in the total, no exchange rate to EUR: Cafe (id:41): -XYZ 12.50` |
| `get_spending_by_category` | `Not included in the total, no exchange rate to EUR: Travel (id:36): -XYZ 10.00` |
| `get_recurring_bills` | `Not included in the monthly total, no exchange rate to EUR: Gym (id:7): -XYZ 30.00/{cycle}` |

`XYZ` stands for any currency without a stored rate. Outgoing amounts in `get_transactions`, `get_spending_by_category` and `get_recurring_bills` print with a minus sign. `get_account_balances` prints a negative balance as `XYZ 100.00 (negative)`. `{cycle}` is the bill's billing cycle. `get_spending_by_category` shows one subtotal per category and currency, up to `top` of them, then `and N more` if there are others. If no row can be converted, it returns no total:

```text
Spending breakdown for 2026-01-01 to 2026-03-09: no total, no row has an exchange rate to EUR.
Spending without an exchange rate to EUR: Travel (id:36): -XYZ 10.00
```

The second line is the only place those amounts appear. It has the same subtotals, `top` limit and `and N more` ending as the `Not included in the total` line in the table row above.

### Unknown billing cycles

`get_recurring_bills` leaves out bills with an unknown cycle and names them:

```text
Not included in the monthly total, unknown cycle: Old Gym (id:9): -$20.00/unknown cycle
```
