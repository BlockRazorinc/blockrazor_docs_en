---
description: >-
  Use the BlockRazor Robinhood Chain eth_sendBatch API. Review the request
  parameters, and request example.
metaLinks:
  canonical: eth_sendbatch.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/robinhood-chain/send-batch
---

# Robinhood Chain eth\_sendBatch API

`eth_sendBatch` is a transaction batch sending interface provided by BlockRazor for Robinhood Chain. Users can use this method to send signed raw transactions in batches to the chain with low latency. Currently, HTTPS protocol is supported.

A batch has a maximum transaction capacity of 10 transactions, which are sent sequentially to Robinhood's official sequencer every 5ms. It's important to note that batches are not atomic and there is no guarantee that the final on-chain order will match the expected request order.

### Request parameters

<table><thead><tr><th width="123.7890625">Parameters</th><th width="122.76953125">Mandatory</th><th width="101.30859375">Format</th><th width="203.80078125">Example</th><th>Description</th></tr></thead><tbody><tr><td>-</td><td>Mandatory</td><td>String[]</td><td>["0x…9c","0x…6a"]</td><td>Signed raw transactions</td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```bash
curl https://robinhood.blockrazor.io \
  -H 'content-type: application/json' \
  -H 'Authorization: Bearer <auth-token>' \
  --data '{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "eth_sendBatch",
  "params": ["0x…9c","0x…6a"]
}'
```
{% endtab %}
{% endtabs %}

### Response Example

```json
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0xa06b……f7e8ec" // batch hash
}‍
```

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "error":{
    "code":-32000,
    "message":"auth is invalid"
    }
}
```

