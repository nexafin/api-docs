---
description: Changes to the Nexafin public API and MCP connector.
---

# Changelog

## 2026-10-07

### Holiday-aware paydays are available through the API and MCP

The Public API adds `GET`, `PUT`, and `DELETE /v1/pay-schedule`. The MCP connector adds `get_pay_schedule` and `set_pay_schedule`. Both use the same holiday-aware schedule as Nexafin Settings and return nominal and adjusted dates, including holiday names or weekend reasons.

Payday writes require a separate `pay-schedule:write` grant. Existing API keys and OAuth connections do not gain write access from read access. See [Pay Schedule](reference/pay-schedule.md) and the [MCP Tools Reference](mcp/tools.md#get_pay_schedule).

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

When there is no current rate, the connector uses the last rate it has stored. For `get_transactions` and `get_spending_by_category`, that is the latest stored rate on or before each row's booked date, or the earliest later one if none is earlier. It adds a line that says so, for example `CAD rate from 2026-10-04`.

If no stored rate has both currencies, the amounts that cannot be converted are left out of the total. The tool names them at the end of its output. The line starts with `Not included in the total` or, for bills, `Not included in the monthly total`. When `get_spending_by_category` has no total at all, the line starts with `Spending without an exchange rate to`. The tool call does not fail.

Bills with an unknown billing cycle are labelled `unknown cycle` and left out of the monthly total. Before, they were added at their full amount.

See the [Tools Reference](mcp/tools.md#how-totals-are-converted) for the exact output lines.
