---
description: Each transaction is an income or expense in a user bank account.
---

# Transactions

<figure><img src="../.gitbook/assets/transactions.png" alt="Transactions list in Nexafin"><figcaption></figcaption></figure>

{% hint style="warning" %}
All API requests must include `Accept: application/json` and `Content-Type: application/json` headers. Without the `Accept` header, error responses will return HTML instead of JSON.
{% endhint %}

### Endpoints

{% swagger baseUrl="https://app.nexafin.com/v1" method="get" path="/transactions" summary="List and filter your transactions" %}
{% swagger-description %}
Returns both `booked` and `pending` transactions. Use the `status` field in the response to distinguish between them.
{% endswagger-description %}

{% swagger-parameter in="query" name="page" type="integer" %}
Page number.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="per-page" type="integer" %}
Number of items per page.&#x20;

Max value is 30.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="include" type="string" %}
You can add one or multiple of category, bankAccount and bankAccount.bank.



Separated by comma.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="filter[text]" type="string" %}
Filter transactions by description or merchant name.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="filter[bank_account_id]" type="integer" %}
Filter transactions by bank account ID.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="filter[type]" type="string" %}
Filter by transaction type: `income` or `expense`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="filter[category]" type="integer" %}
Filter by category ID.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="base-fiat" type="string" %}
In which currency do you want the transactions.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" type="string" %}
Bearer token or API key.



**Example:**&#x20;

Bearer nxfn\_sk\_xxxx...
{% endswagger-parameter %}

{% swagger-response status="200" description="List of transactions" %}
```javascript
{
  "current_page": 1,
  "data": [
    {
      "id": 1,
      "bank_account_id": 1,
      "category_id": 20,
      "status": "booked",
      "type": "expense",
      "from": [],
      "to": [],
      "description": "Any random description for a transaction",
      "notes": null,
      "amount": -73640,
      "currency": "USD",
      "booked_at": "2022-08-14T00:00:00.000000Z",
      "created_at": "2022-08-14T22:13:58.000000Z",
      "updated_at": "2022-08-14T22:13:58.000000Z",
      "deleted_at": null,
      "category": {
        "id": 20,
        "parent_id": 17,
        "type": "expense",
        "level": 1,
        "accountable": 1,
        "slug": "transportation-expenses-expense",
        "icon": "emoji_transportation",
        "name": "Transportation expenses",
        "description": "All transactions that are recognized as payments for public transportation, taxi, toll roads, department of motor vehicles and car inspections",
        "created_at": "2022-08-14T22:13:56.000000Z",
        "updated_at": "2022-08-14T22:13:56.000000Z"
      },

      // ...
    }
  ],
  "first_page_url": "https://app.nexafin.com/v1/transactions?page=1",
  "from": 1,
  "last_page": 32,
  "last_page_url": "https://app.nexafin.com/v1/transactions?page=32",
  "links": [
    {
      "url": null,
      "label": "&laquo; Previous",
      "active": false
    },
    {
      "url": "https://app.nexafin.com/v1/transactions?page=1",
      "label": "1",
      "active": true
    },

    // ...

    {
      "url": "https://app.nexafin.com/v1/transactions?page=2",
      "label": "Next &raquo;",
      "active": false
    }
  ],
  "next_page_url": "https://app.nexafin.com/v1/transactions?page=2",
  "path": "https://app.nexafin.com/v1/transactions",
  "per_page": 30,
  "prev_page_url": null,
  "to": 1,
  "total": 32
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Permission denied" %}

{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Example request**

```bash
curl -X GET "https://app.nexafin.com/v1/transactions?include=category,bankAccount&per-page=10" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Accept: application/json"
```
{% endhint %}

{% swagger method="post" path="/transactions" baseUrl="https://app.nexafin.com/v1" summary="Create a new transaction" %}
{% swagger-description %}
You can only create transactions for manual accounts.
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="amount" type="Integer" %}
In cents. Negative for expenses, positive for income (e.g., -5000 = -$50.00).
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="bank_account_id" type="Integer" %}
The ID of the bank account. Must be a manual account owned by the authenticated user.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="booked_at" type="String" %}
Booked at date in Y-m-d format (e.g., `2026-03-16`).
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="description" type="String" %}
A description of the transaction.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="currency" type="String" %}
Currency code (e.g., `USD`, `EUR`). Inherited from bank account if not provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="notes" type="String" %}
Optional notes for the transaction.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="category_id" type="Integer" %}
Category ID. Use `GET /v1/transaction-categories` to list available categories.
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Transaction created" %}
```javascript
{
    "id": 9
}
```
{% endswagger-response %}

{% swagger-response status="422: Unprocessable Entity" description="Missing or invalid fields" %}
```javascript
{
    "message": "The description field is required. (and 3 more errors)",
    "errors": {
        "amount": [
            "The amount field is required."
        ],
        "bank_account_id": [
            "The bank account id field is required."
        ],
        "booked_at": [
            "The booked at field is required."
        ],
        "description": [
            "The description field is required."
        ]
    }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Account is not manual" %}
```javascript
{
    "message": "You can't create a transaction for a non-manual account."
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Permission denied" %}
```javascript
{
    "message": "Unauthenticated."
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Example request**

```bash
curl -X POST https://app.nexafin.com/v1/transactions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "amount": -5000,
    "bank_account_id": 1,
    "booked_at": "2026-03-16",
    "description": "Monthly subscription",
    "currency": "USD",
    "notes": "Auto-billed",
    "category_id": 42
  }'
```

**Notes:**
- `amount` is in cents (e.g., -5000 = -$50.00). Negative for expenses, positive for income.
- `bank_account_id` must be a **manual** account you own. Use `GET /v1/bank-accounts` to find your account IDs.
- `booked_at` format: `YYYY-MM-DD`
- `currency` is optional — defaults to the bank account's currency.
{% endhint %}

{% swagger method="put" path="/transactions/{id}" baseUrl="https://app.nexafin.com/v1" summary="Update a transaction" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="Integer" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-parameter in="body" required="false" name="category_id" type="Integer" %}
New transaction category
{% endswagger-parameter %}

{% swagger-parameter in="body" name="notes" type="String" %}
New transaction notes
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="Transaction updated successfully" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Permission denied" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="delete" path="/transactions/{id}" baseUrl="https://app.nexafin.com/v1" summary="Delete a transaction" %}
{% swagger-description %}
The transaction will be marked as deleted and this action can't be undone.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="Integer" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="Transaction deleted" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Permission denied" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
