---
description: >-
  Use the BlockRazor BSC Block Builder Trace Bundle API. Review the endpoint,
  parameters, request format, response fields, and integration example.
metaLinks:
  canonical: call-bundle.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder/call-bundle
---

# BSC Block Builder Call Bundle API Reference

### Introduction

This API is used to receive requests for simulating bundles. The method name is `eth_callBundle`.

### Price

<table><thead><tr><th width="134.90625">Payment</th><th width="370.83984375">Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td><strong>$20</strong> / day</td><td><a href="https://blockrazor.io/#/portal/pricing?purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_call_bundle&#x26;billing=day" class="button primary small">訂閱</a></td></tr><tr><td>Package</td><td><strong>$1250</strong> / month<br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">訂閱</a></td></tr></tbody></table>

### Rate Limit

1 TPS

### Request Parameter

<table><thead><tr><th width="156">Parameters</th><th width="124">Mandatory</th><th width="122">Format</th><th>Example</th><th>Description</th></tr></thead><tbody><tr><td>txs</td><td>Mandatory</td><td>array[hex]</td><td>["0x…4b", "0x…5c"]</td><td>List of signed raw transactions</td></tr><tr><td>blockNumber</td><td>Mandatory</td><td>number</td><td>39177941</td><td>Current block number + 1</td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```bash
curl https://virginia.builder.blockrazor.io \
  -H 'content-type: application/json' \
  -H 'Authorization: <auth-token>' \
  --data '{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "eth_callBundle",
  "params": [
    {
      "txs":["0x…4b"],
      "blockNumber":39177941
    }
  ]
  }'
```
{% endtab %}
{% endtabs %}

### Response Example

**Success**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "bundleHash": "0xb13bb92ebee57b42b7e8e91b41891e1693a449dcdb8530c6bffe684157e988da",
    "ethSentToBuilder": "63003000000000",
    "gasFees": "21000000000000",
    "results": [
      {
        "ethSentToBuilder": "63003000000000",
        "fromAddress": "0x12994B3004Daab21035EDa1D4b31F7F63D606128",
        "gasFees": "21000000000000",
        "gasPrice": "4000142857",
        "gasUsed": 21000,
        "toAddress": "0x1266C6bE60392A8Ff346E8d5ECCd3E69dD9c5F20",
        "txHash": "0x0fbaa913aa0ffc22bba63a81ca2958ad14d630928c5a5240130fec7037605648",
        "value": "0x"
      }
    ],
    "stateBlockNumber": 756983,
    "totalGasUsed": 21000
  }
}
```

**Error**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32000,
    "message": "missing auth token"
  }
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32000,
    "message": "rate limit exceeded, try again later"
  }
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32000,
    "message": "err: nonce too low: address 0x6c85F133fa06Fe5eb185743FB6c79f4a7cb9C076, tx: 28 state: 29; txhash 0x300c95d2b3086ddc1836de1e8c87878916d332ff42e8312cd404264ab8cdcd18"
  }
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "bundleHash": "0x592da70a510720d149567bfbc5935a05780963607c3c75b93a9190bb60818f21",
    "ethSentToBuilder": "0",
    "gasFees": "218070000000000",
    "results": [
      {
        "error": "execution reverted",
        "ethSentToBuilder": "0",
        "fromAddress": "0xb0b10B09780aa6A315158EF724404aa1497e9E6E",
        "gasFees": "218070000000000",
        "gasPrice": "10000000000",
        "gasUsed": 21807,
        "revert": "unlock error: locked",
        "toAddress": "0xdA51Cf6ed22740FD8fAfbBe61577A577915e7526",
        "txHash": "0xa09745617e0a212c5da379ea066f64e33f5215ce235d653bbbc1359b38d78baa",
      }
    ],
    "stateBlockNumber": 29115,
    "totalGasUsed": 21807
  }
}
```

