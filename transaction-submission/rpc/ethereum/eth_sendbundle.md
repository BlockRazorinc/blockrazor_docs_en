---
description: >-
  Use the BlockRazor Ethereum RPC eth_sendBundle API. Review the endpoint,
  parameters, request format, response fields, and integration example.
metaLinks:
  canonical: eth_sendbundle.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/ethereum-rpc/eth_sendbundle
---

# Ethereum RPC eth\_sendBundle

### Endpoint

{% hint style="info" %}
For project RPC, please refer to the [Integration](integration.md)
{% endhint %}

<table><thead><tr><th width="203.8515625">Endpoint type</th><th width="483.47265625">URL</th></tr></thead><tbody><tr><td>General RPC</td><td>https://eth.blockrazor.xyz</td></tr><tr><td>Project Default RPC</td><td>https://eth.blockrazor.xyz/&#x3C;rpc_id></td></tr><tr><td>Project Custom RPC</td><td>https://&#x3C;custom_domain>.eth.blockrazor.xyz</td></tr></tbody></table>

### Request parameters

<table><thead><tr><th width="183">Parameters</th><th width="118">Mandatory</th><th width="98">Format</th><th width="132">Example</th><th>Remark</th></tr></thead><tbody><tr><td>txs</td><td>mandatory</td><td>[]bytes</td><td>[ "0xf84a……e54284" ]</td><td>raw txs, up to 50 transactions allowed to be set</td></tr><tr><td>revertingTxHashes</td><td>optional</td><td>[]hash</td><td>["0x1f23……0abb1e"]</td><td>Transactions that allow to be reverted, a subset of txs</td></tr><tr><td>blockNumber</td><td>optional</td><td>uint64</td><td>"0x3C04F81"</td><td>The maximum block number for the bundle to be valid, with the default set to the current block number + 100</td></tr></tbody></table>

### Request Example

```json
curl -X POST -H "Content-Type: application/json" --data '{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "eth_sendBundle",
  "params": [{
    "Txs": [
"0xf8668203988405f5e100825208942ee393c739036a7660ec11bf2101d537eb52f3ac80808193a06836d5f4052376dc3114794da5fedd7a5b8090ddae0ec45dfa66c234fcabb6efa07cf17dad992c33e8e3e4d82355cfe12d3be2560fc2b0873c36dce398088d8e4f"
    ]
    "revertingTxHashes":[],
    "blockNumber":"0x3C04F81"
  }]
}' https://eth.blockrazor.xyz
```

### Response Example

normal

```json
{"jsonrpc":"2.0","id":1,"result": "0x11111111..."}
```

abnormal

```json
{"jsonrpc":"2.0","id":1,"jsonerror":{"code":-38000,"message":"nonce too low: address 0x9Abae1b279A4Be25AEaE49a33e807cDd3cCFFa0C, tx: 0 state: 45"}}
```
