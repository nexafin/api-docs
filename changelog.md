---
description: Changes to the Nexafin public API and MCP connector.
---

# Changelog

## 2026-10-06

### Conversion between two non-USD currencies is corrected

Amounts that move between two currencies that are not USD now use the right rate. Before, they were wrong. For example, a EUR -90.00 transaction shown in CAD was -74.07. It now shows -135.00.

This changes amounts in:

* `GET /v1/transactions` when you pass `base-fiat`
* the MCP connector `get_transactions` tool when you pass `currency`

Only the numbers change. Field names and response shapes are the same. Conversions to or from USD, and amounts already in the target currency, are unchanged.

### MCP connector totals use your display currency

The MCP connector now adds up amounts in your display currency. Tool names and input schemas are unchanged.

* `get_transactions`: the `currency` argument now defaults to your display currency. It no longer defaults to USD. Pass `currency` to ask for another one.
* `get_account_balances`: each balance is converted before it is added to the total. Each account line still shows the account's own currency.
* `get_recurring_bills`: each bill is converted before it is added to the monthly total. Each bill line still shows the bill's own currency. Daily and semi-annual cycles now count toward the total.
* `get_spending_by_category`: amounts are shown in your display currency, not in `$`.

When there is no current rate, the connector uses the last rate it has stored. It adds a line that says so, for example `CAD rate from 2026-10-04`.

If a currency has never had a stored rate, its amounts are left out of the total. The tool names them in a `Not included in the total` line. The tool call does not fail.

Bills with an unknown billing cycle are labelled `unknown cycle` and left out of the monthly total. Before, they were counted as monthly.

See the [Tools Reference](mcp/tools.md#how-totals-are-converted) for the exact output lines.
