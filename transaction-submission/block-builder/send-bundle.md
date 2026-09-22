---
description: >-
  Use the BlockRazor BSC Block Builder Send Bundle API. Review the endpoint,
  parameters, request format, response fields, and integration example.
metaLinks:
  canonical: send-bundle.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder/send-bundle
---

# BSC Block Builder Send Bundle API Reference

### Introduction

This API is used to receive bundles, with the method name `eth_sendBundle`.

The block construction algorithm of BlockRazor Builder favors bundles that transfer more native tokens (BNB) to BlockRazor Builder EOA of which address is 0x1266C6bE60392A8Ff346E8d5ECCd3E69dD9c5F20 currently.

The gas price for transactions within the bundle must be at least the minimum standard required by BSC Validators (Currently at 0.05 gwei).

### Request Parameter

<table data-search="false"><thead><tr><th width="144">Parameters</th><th width="118">Mandatory</th><th width="116">Format</th><th width="136">Example</th><th>Description</th></tr></thead><tbody><tr><td>txs</td><td>Mandatory</td><td>array[hex]</td><td>["0x…4b", "0x…5c"]</td><td>List of signed raw transactions</td></tr><tr><td>maxBlockNumber</td><td>Optional</td><td>uint64</td><td>39177941</td><td>The maximum block number for the bundle to be valid, with the default set to the current block number + 100</td></tr><tr><td>minTimestamp</td><td>Optional</td><td>uint64</td><td>1710229370</td><td>Expected minimum Unix timestamp (in seconds) for the bundle to be valid</td></tr><tr><td>maxTimestamp</td><td>Optional</td><td>uint64</td><td>1710829390</td><td>Expected maximum Unix timestamp (in seconds) for the bundle to be valid</td></tr><tr><td>revertingTxHashes</td><td>Optional</td><td>array[hash]</td><td>["0x…c7", "0x…b7"]</td><td>List of transaction hashes allowed for revert</td></tr><tr><td>noMerge</td><td>Optional</td><td>bool</td><td>false</td><td>Bundle merge can increase block value and inclusion rate. If not set, the default value is false (allowing bundle merging)</td></tr><tr><td>positionFirst</td><td>Optional</td><td>bool</td><td>false</td><td>Strictly order bundles by priorityFee. If set to false (default), late-arriving bundles may be appended to the block tail to guarantee inclusion in the current block.</td></tr></tbody></table>

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
    "method": "eth_sendBundle",
    "params": [{
      "txs": [
        "0x...4b",
        "0x...5c"
      ],
      "maxBlockNumber": 39177941,
      "minTimestamp": 1710229370,
      "maxTimestamp": 1710829390,
      "revertingTxHashes": [
        "0x44b89abe860142d3c3bda789cf955b69ba00b71882cd968ec407a70f4719ff06",
        "0x7d7652c685e9fda4fe2e41bad017519cffeed8ba03d59aa6401284be2ec4244c"
      ],
      "noMerge": false
    }]
  }'
```
{% endtab %}
{% endtabs %}

### Response Example

```json
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0xa06b……f7e8ec"  //bundle hash
}‍
```

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "error":{
    "code":-38000,
    "message":"the maxBlockNumber should not be smaller than currentBlockNum"
    }
}
```

