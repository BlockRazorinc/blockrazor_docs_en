---
description: >-
  Use the BlockRazor Solana Send Bundle API. Review the endpoint, parameters,
  and request example.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-bundle
---

# Solana Send Bundle API Reference

{% hint style="warning" %}
Solana's bundle sending service is not bound to the subscription plan and is not available by default. If you need to increase the BPS limit, please [contact](https://discord.com/invite/qqJuwRb8Nh) us and we will handle it as soon as possible.
{% endhint %}

`Send Bundle` is a bundle sending interface provided by BlockRazor for Solana, used to send signed transactions to the blockchain in bundle form with low latency.

A maximum of four transactions can be sent in a single bundle. These transactions are executed sequentially, and if any one transaction fails, the entire bundle will fail.

To enhance MEV protection, use any valid Solana public key starting with `jitodontfront` and `dontbund1e` in the instructions of the first transaction in the bundle. If the transaction is subjected to a frontrun attack, it will be rejected by the Jito/Harmonic block engine.

### Endpoint <a href="#xian-liu" id="xian-liu"></a>

* `POST /sendBundle`
* `gRPC`

### Transaction Construction Example <a href="#jiao-yi-gou-jian-dai-ma-shi-li" id="jiao-yi-gou-jian-dai-ma-shi-li"></a>

* [Curl](curl.md)
* [Go](go.md)
* [Rust](rust.md)
* [JS](js.md)

### Request Parameter

<table><thead><tr><th width="111.44921875">Parameters</th><th width="115.421875">Mandatory</th><th width="104.94140625">Format</th><th width="109.98046875">Example</th><th>Description</th></tr></thead><tbody><tr><td>transactions</td><td>Mandatory</td><td>string[]</td><td>["$base64_tx_1","$base64_tx_2","$base64_tx_3"]</td><td>Fully signed transactions, Base64 encoding protocal </td></tr></tbody></table>
