---
description: >-
  Use the BlockRazor BSC Block Builder Send PrivateTransaction API. Review the
  endpoint, parameters, request format, response fields, and integration
  example.
metaLinks:
  canonical: send-privatetransaction.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder/send-privatetransaction
---

# BSC Block Builder Send PrivateTransaction API Reference

### Introduction

This API is used to receive private transactions submitted by users, with the method name `eth_sendPrivateTransaction`

### Request Parameters

<table><thead><tr><th width="169">Parameters</th><th width="117">Mandatory</th><th width="90">Format</th><th width="114">Example</th><th>Description</th></tr></thead><tbody><tr><td>transaction</td><td>Mandatory</td><td>String</td><td>"0x…4b"</td><td>signed raw transaction </td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```javascript
curl https://virginia.builder.blockrazor.io \
  -H 'content-type: application/json' \
  -H 'Authorization: <auth-token>' \
  --data '{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "eth_sendPrivateTransaction",
  "params": ["0x…9c"]
}'
```
{% endtab %}
{% endtabs %}

#### Proto

```go
syntax = "proto3";

package sendbundle;

option go_package = "internal/ethapi/sendbundle;sendbundle";

service BundleService {
  rpc SendBundle (SendBundleArgs) returns (SendBundleResponse);
  rpc SendTransaction (SendTransactionArgs) returns (SendTransactionResponse);
}

message SendBundleArgs {
  repeated bytes txs = 1;
  uint64 maxBlockNumber = 2;
  uint64 minTimestamp = 3;
  uint64 maxTimestamp = 4;
  repeated string revertingTxHashes = 5;
}

message SendTransactionArgs {
  bytes tx = 1;
}

message SendBundleResponse {
    string result = 1;
}

message SendTransactionResponse {
    string result = 1;
}
```

### Reponse Example

```json
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0xa06b……f7e8ec"  // tx hash
}‍‍
```

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "error":{
    "code":-32000,
    "message":"nonce too low: next nonce 57, tx nonce 56"
    }
}
```

