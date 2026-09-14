---
description: >-
  This section introduces the integration of eth_sendRawTransaction provided by
  BlockRazor Robinhood Chain Transaction Sending Mode
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/robinhood-chain/eth_sendrawtransaction
---

# Robinhood Chain eth\_sendRawTransaction

`eth_sendRawTransaction` is a transaction sending interface provided by BlockRazor for Robinhood Chain. Users can use this method to send signed raw transactions to the chain with low latency. Currently, HTTPS protocol is supported.

### Benchmark

We deployed test clients across AWS regions in Frankfurt, Ohio, and Japan, and sent 50 pairs of transactions with identical nonces to both the corresponding regional BlockRazor endpoints and the official RPC endpoints. Within each transaction pair, all parameters were kept exactly the same except for the recipient address, which was used to distinguish between the two submission channels.

Performance was evaluated by comparing the ratio of transactions that were ultimately included through each channel. A higher on-chain inclusion rate indicates faster transaction propagation and execution performance. The benchmark results are shown below.

<table><thead><tr><th width="174.3359375">Region</th><th>BlockRazor Inclusion Rate</th><th>Robinhood Inclusion Rate</th></tr></thead><tbody><tr><td>Frankfurt</td><td>70%</td><td>30%</td></tr><tr><td>Ohio</td><td>60%</td><td>40%</td></tr><tr><td>Tokyo</td><td>65%</td><td>35%</td></tr><tr><td>Singapore</td><td>65%</td><td>35%</td></tr></tbody></table>

### Request parameters

<table><thead><tr><th width="123.7890625">Parameters</th><th width="122.76953125">Mandatory</th><th width="101.30859375">Format</th><th width="106">Example</th><th>Description</th></tr></thead><tbody><tr><td>-</td><td>Mandatory</td><td>String</td><td>"0x…4b"</td><td>Signed raw transaction</td></tr></tbody></table>

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
  "method": "eth_sendRawTransaction",
  "params": ["0x…9c"]
}'
```
{% endtab %}
{% endtabs %}

### Response Example

```json
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0xa06b……f7e8ec"
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

