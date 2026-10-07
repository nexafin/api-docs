---
description: Read and manage the holiday-aware payday schedule for your account.
---

# Pay Schedule

Nexafin keeps one pay schedule per user. Dates are calculated by the same service used in Settings: a nominal payday can move before, stay on, or move after a weekend or bank holiday. The country comes from your profile, then the schedule's bank, then a weekends-only fallback.

## Get pay schedule

{% swagger method="get" path="/pay-schedule" baseUrl="https://app.nexafin.com/v1" summary="Get the pay schedule and upcoming paydays" %}
{% swagger-description %}
Returns three upcoming paydays by default. A nominal date is the recurring calendar date; `adjusted_date` is the expected deposit date after applying the learned rule.
{% endswagger-description %}

{% swagger-parameter in="query" name="count" type="Integer" %}
Number of upcoming paydays to return. From 1 to 12; defaults to 3.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Schedule detected or set" %}
```json
{
  "state": "detected",
  "schedule": {
    "frequency": "monthly",
    "nominal_anchor": [25],
    "learned_rule": "before",
    "country": "US",
    "country_source": "user",
    "set_by": "detected",
    "source_description": "Acme payroll",
    "confirmed": true,
    "may_have_changed": false
  },
  "upcoming": [
    {
      "nominal_date": "2026-12-25",
      "adjusted_date": "2026-12-24",
      "shift_reason": {"type": "holiday", "name": "Christmas Day"}
    }
  ]
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="No regular payday detected" %}
```json
{
  "state": "not_detected",
  "reason": "no_regular_payday_detected",
  "settings_url": "https://app.nexafin.com/app/settings?section=payday"
}
```
{% endswagger-response %}
{% endswagger %}

`country_source` is `user`, `bank`, or `fallback`. With `fallback`, `country` is `null` and only weekends are considered. `shift_reason` is `null` when no shift occurs; otherwise its type is `holiday` or `weekend`. Weekend reasons have a `null` name.

`nominal_anchor` is an array describing the unadjusted cadence. Monthly and semimonthly schedules use calendar-day anchors such as `[25]` or `["middle", "end"]`. Weekly and biweekly schedules use a nominal reference payday as an ISO date anchor, such as `["2026-12-21"]`; this date establishes the repeating 7- or 14-day interval.

## Set or change pay schedule

{% swagger method="put" path="/pay-schedule" baseUrl="https://app.nexafin.com/v1" summary="Set a payday override" %}
{% swagger-description %}
Sets the same override used by the Settings payday editor. Automatic detection will not replace it until you reset to automatic.
{% endswagger-description %}

{% swagger-parameter in="body" name="pattern" type="String" required="true" %}
One of `weekly`, `biweekly`, `semimonthly`, or `monthly`.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="next_date" type="String" required="true" %}
The next payday in `YYYY-MM-DD` format. It cannot be before today in your saved time zone.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="non_business_shift" type="String" %}
One of `before`, `on`, or `after`.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Override saved" %}
The response has the same shape as `GET /v1/pay-schedule`.
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="API key lacks pay-schedule:write" %}
The key was authenticated but was not created with payday write access.
{% endswagger-response %}
{% endswagger %}

```bash
curl -X PUT https://app.nexafin.com/v1/pay-schedule \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "pattern": "biweekly",
    "next_date": "2026-12-24",
    "non_business_shift": "before"
  }'
```

## Reset to automatic detection

{% swagger method="delete" path="/pay-schedule" baseUrl="https://app.nexafin.com/v1" summary="Reset payday to automatic detection" %}
{% swagger-description %}
Removes the user override and resumes automatic detection. If enough payroll history already exists, Nexafin may detect a schedule immediately.
{% endswagger-description %}

{% swagger-response status="204: No Content" description="Reset complete" %}
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="API key lacks pay-schedule:write" %}
{% endswagger-response %}
{% endswagger %}

Both write endpoints require an API key created with `pay-schedule:write`. Read access does not grant write access, and legacy JWT API tokens cannot write pay schedules.
