---
description: Use this cURL example to send Solana transaction batches with BlockRazor.
metaLinks:
  canonical: curl.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-batch/request-example/curl
---

# Solana Send Batch Curl Example

### Request Example

{% code overflow="wrap" %}
```bash
curl --request POST \
  --url http://frankfurt.solana.blockrazor.xyz:443/sendBatch \
  --header 'Content-Type: application/json' \
  --header 'apikey: $auth_token' \
  --data '{
  "transactions":["$base64_tx_1","$base64_tx_2","$base64_tx_3"],
  "mode": "fast"
}'
```
{% endcode %}

### Response Example

Normal

{% code overflow="wrap" %}
```json
{
  "result": [{
    "signature": "58pw71yy2LDSw......Kdx5akJeRGUTS9bwGctXcHPAu",
    "error": ""
  }],
  "error": ""
}
```
{% endcode %}

Abnormal

{% code overflow="wrap" %}
```json
{"result":[],"error":"error: Invalid authentication credentials. Please ensure your auth token is correct and try again"}
```
{% endcode %}
