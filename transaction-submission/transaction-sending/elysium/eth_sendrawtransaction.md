---
description: >-
  Learn about the BlockRazor Elysium eth_sendRawTransaction API, including its
  overview, pricing, request parameters, request examples, and response examples
---

# Elysium eth\_sendRawTransaction API

Elysium is a high-performance Layer 2 network in the Hyperliquid ecosystem, built on Arbitrum Orbit. It settles on HyperEVM and uses HYPE to pay for gas. Compatible with the EVM and mainstream Ethereum development tools, it targets approximately 100-millisecond block times and high-throughput execution, providing a low-latency onchain environment for spot trading, professional market making, token launches, and other DeFi applications.

`eth_sendRawTransaction` is BlockRazor’s transaction submission API for Elysium. Users can submit signed raw transactions to the network with low latency through this method. HTTPS is currently supported.

#### Pricing

<table><thead><tr><th width="117.859375">User Type</th><th width="264.87890625">Price</th><th width="310.44921875">Rate Limit</th></tr></thead><tbody><tr><td>Newly Registered Users</td><td>1 Tx / 5s</td><td>Free</td></tr><tr><td>Paid Users</td><td>20 Txs / 1s</td><td>$100 / day<br>$1000 / month<br><br><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=hyperliquid_l2&#x26;serviceId=hyperliquid_l2_transaction_sending&#x26;billing=day" class="button primary medium">Subscribe</a></td></tr></tbody></table>

#### Endpoint

<table><thead><tr><th width="115.1171875">Region</th><th width="580.09765625">Endpoint</th></tr></thead><tbody><tr><td>Tokyo</td><td>https://ap.elysium.blockrazor.io</td></tr></tbody></table>

#### Request Parameters

<table><thead><tr><th width="115.6328125">Parameter</th><th width="112.89453125">Required</th><th width="82">Format</th><th width="106">Example</th><th width="276.828125">Description</th></tr></thead><tbody><tr><td>-</td><td>Yes</td><td>String</td><td>"0x…4b"</td><td>A signed raw transaction</td></tr></tbody></table>

#### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```bash
curl https://ap.elysium.blockrazor.io \
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

#### Response Examples

```json
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0xa06b……f7e8ec"  // Transaction hash
}
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
