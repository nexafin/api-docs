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

The MCP connector now adds up amounts in your display currency. Tool names are unchanged. One input schema changes: `get_transactions` no longer has a `USD` default for `currency`.

* `get_transactions`: the `currency` argument now defaults to your display currency. It no longer defaults to USD. Pass `currency` to ask for another one.
* `get_account_balances`: each balance is converted before it is added to the total. Each account line still shows the account's own currency.
* `get_recurring_bills`: each bill is converted before it is added to the monthly total. Each bill line still shows the bill's own currency. Each bill is now turned into a monthly amount by its billing cycle. Before, every bill was added at its full amount whatever its cycle, so totals with non-monthly bills change.
* `get_spending_by_category`: amounts are shown in your display currency, not in `$`.

When there is no current rate, the connector uses the last rate it has stored. For `get_transactions` and `get_spending_by_category`, that is the stored rate nearest to each row's booked date. It adds a line that says so, for example `CAD rate from 2026-10-04`.

If no stored rate has both currencies, its amounts are left out of the total. The tool names them in a `Not included in the total` line. The tool call does not fail.

Bills with an unknown billing cycle are labelled `unknown cycle` and left out of the monthly total. Before, they were added at their full amount.

See the [Tools Reference](mcp/tools.md#how-totals-are-converted) for the exact output lines.
