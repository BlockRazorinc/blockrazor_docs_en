---
description: >-
  Use the BlockRazor Ethereum RPC eth_sendRawTransaction API. Review the
  endpoint, parameters, request format, response fields, and integration
  example.
metaLinks:
  canonical: eth_sendrawtransaction.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/ethereum-rpc/eth_sendrawtransaction
---

# Ethereum RPC eth\_sendRawTransaction

&#x20;`eth_sendRawTransaction` of BlockRazor RPC is compatible with native JSON-RPC methods and requires no additional modifications.&#x20;

If you need to modify parameters such as hint, refund address and revert protection, you can configure the dedicated RPC in the [portal](https://www.blockrazor.io/#/login).

### Endpoint

{% hint style="info" %}
For project RPC, please refer to the [Integration](integration.md)
{% endhint %}

<table><thead><tr><th width="203.8515625">Endpoint type</th><th width="483.47265625">URL</th></tr></thead><tbody><tr><td>General RPC</td><td>https://eth.blockrazor.xyz</td></tr><tr><td>Project Default RPC</td><td>https://eth.blockrazor.xyz/&#x3C;rpc_id></td></tr><tr><td>Project Custom RPC</td><td>https://&#x3C;custom_domain>.eth.blockrazor.xyz</td></tr></tbody></table>

### Request parameters

<table><thead><tr><th width="124">Parameters</th><th width="127">Mandatory</th><th width="110">Format</th><th width="180">Example</th><th>Remark</th></tr></thead><tbody><tr><td>-</td><td>Mandatory</td><td>bytes</td><td>"0xd46e……445675"</td><td>raw tx</td></tr></tbody></table>

### Request Example

```json
curl -X POST -H "Content-Type: application/json" --data '{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xd46e……445675"
    ]
}' https://bsc.blockrazor.xyz
```

### Response Example

**normal**

```json
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670……527331"
}
```

**abnormal**

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
	"code": -32000,
	"message": "rlp: element is larger than containing list"
  }
}
```

### Other JSON-RPC Methods

BlockRazor RPC supports standard JSON-RPC method, you can refer to [https://ethereum.org/zh/developers/docs/apis/json-rpc/#json-rpc-methods](https://ethereum.org/zh/developers/docs/apis/json-rpc/#json-rpc-methods)

