---
description: Update the balance of a manual account at a desired moment
---

# Bank Account Balance

{% swagger method="post" path="/v1/bank-accounts/{id}/balance" baseUrl="https://app.nexafin.com" summary="Set the balance of a bank account" %}
{% swagger-description %}
If you don't pass the date param the balance will be stored as the current account balance.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="Integer" %}
Bank account ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="balance" type="Integer" required="true" %}
Balance in cents (e.g., 150000 = $1,500.00)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="date" type="String" %}
Date in Y-m-d format (e.g., `2026-03-16`). Cannot be in the future.
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Balance created" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="422: Unprocessable Entity" description="Validation error" %}
```javascript
{
    "message": "The balance field is required.",
    "errors": {
        "balance": [
            "The balance field is required."
        ]
    }
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
**Example request**

```bash
curl -X POST https://app.nexafin.com/v1/bank-accounts/1/balance \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "balance": 250000,
    "date": "2026-03-16"
  }'
```
{% endhint %}
