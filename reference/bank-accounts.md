---
description: List, create, update or delete your bank accounts
---

# Bank Accounts

<figure><img src="../.gitbook/assets/bank-accounts.png" alt="Bank accounts in Nexafin"><figcaption></figcaption></figure>

### Endpoints

{% swagger method="get" path="/v1/bank-accounts" baseUrl="https://app.nexafin.com" summary="Get a list of all your bank accounts" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="include" type="string" %}
If you want to receive also the bank information, add `bank` as the value.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="List of Bank Accounts" %}
```javascript
[
    {
	"id": 1,
	"external_id": "4a2e413e-949e-3804-aa6s-0f2ca6a08bcb",
	"bank_id": 3,
	"bank_connection_id": 1,
	"name": "My Checking Account",
	"type": "checking",
	"subtype": null,
	"currency": "EUR",
	"balance": 4399311,
	"hidden": false,
	"last_transaction_created_at": "2026-03-10T12:00:00.000000Z",
	"last_balance_created_at": "2026-03-10T12:00:00.000000Z",
	"deleted_at": null,
	"created_at": "2022-09-04T16:10:06.000000Z",
	"updated_at": "2022-09-04T16:10:06.000000Z"
    },

    // ...
]
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Example request**

```bash
curl -X GET "https://app.nexafin.com/v1/bank-accounts?include=bank" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Accept: application/json"
```
{% endhint %}

{% swagger method="post" path="/v1/bank-accounts" baseUrl="https://app.nexafin.com" summary="Create a new manual bank account" %}
{% swagger-description %}
You can create a new manual account to track your balance and transactions. Unlike connected accounts, manual accounts need to be updated manually.
{% endswagger-description %}

{% swagger-parameter in="body" name="name" type="String" required="true" %}
Bank account name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="currency" type="String" required="true" %}
Currency code: EUR, USD, etc.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="balance" type="Integer" %}
Initial balance in cents. Optional.
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Account created" %}
```javascript
{
    "id": 1
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Example request**

```bash
curl -X POST https://app.nexafin.com/v1/bank-accounts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "name": "Cash Savings",
    "currency": "USD",
    "balance": 150000
  }'
```
{% endhint %}

{% swagger method="put" path="/v1/bank-accounts/{id}" baseUrl="https://app.nexafin.com" summary="Update an account" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="Integer" %}
Bank account ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="String" %}
New name for bank account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hidden" type="Boolean" %}
Hide or show the account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="type" type="String" %}
Account type (e.g., `checking`, `savings`, `credit`)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="subtype" type="String" %}
Account subtype
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="Bank account edited" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="delete" path="/v1/bank-accounts/{id}" baseUrl="https://app.nexafin.com" summary="Delete an account" %}
{% swagger-description %}
You can only delete manual accounts.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="Integer" %}
Bank account ID
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="Account deleted" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="You can't delete this account" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
