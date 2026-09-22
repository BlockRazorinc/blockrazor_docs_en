---
description: Use this cURL example to build and send Solana transactions with BlockRazor.
metaLinks:
  canonical: curl.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-transaction/request-example/curl
---

# Solana Send Transaction Curl Example

### Request Example

```json
// below is the example of fast mode

curl --request POST \
  --url http://frankfurt.solana.blockrazor.xyz:443/sendTransaction \
  --header 'Content-Type: application/json' \
  --header 'apikey: $auth_token' \
  --data '{
  "transaction":"$base64_tx",
  "mode":"fast",
  "revertProtection":false
}'
```
