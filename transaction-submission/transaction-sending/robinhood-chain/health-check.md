---
description: >-
  Explore the integration of Health Check method provided by BlockRazor
  Robinhood Chain Transaction Sending mode.
metaLinks:
  canonical: health-check.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/robinhood-chain/health-check
---

# Robinhood Chain Transaction Sending Health Check

Send post request to the health endpoint to keep connection alive, the request is as follows:

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X POST 'https://robinhood.blockrazor.io/health' \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <auth-token>" \
-d ""
```
{% endtab %}
{% endtabs %}
