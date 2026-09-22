---
description: >-
  Explore the Keep Alive integration method of BlockRazor Solana Transaction
  Sending mode.
metaLinks:
  canonical: keep-alive.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/keep-alive
---

# Solana Transaction Sending Keep Alive

Send post request to the health endpoint to keep connection alive, the request is as follows:

{% tabs %}
{% tab title="CURL" %}
```bash
curl -X POST 'http://frankfurt.solana.blockrazor.xyz:443/health' \
-H "Content-Type: application/json" \
-H "apikey: <auth_token>" \
-d ""
```
{% endtab %}
{% endtabs %}
