---
description: >-
  Use the BlockRazor Solana Send Batch API. Review the endpoint, transaction
  construction example, and request parameter.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-batch
---

# Solana Send Batch API Reference

{% hint style="warning" %}
Solana's batch sending service is not bound to the subscription plan, with rate limit default to 3 TPS. API key could be required from [Authentication](../../../../get-started/authentication.md). If you need to increase the TPS limit, please [contact](https://discord.com/invite/qqJuwRb8Nh) us and we will handle it as soon as possible.
{% endhint %}

`Send Batch` is a batch sending interface provided by BlockRazor for Solana, used to send signed transactions to the blockchain in batch form with low latency. A maximum of 25 transactions can be sent in a single batch.

### Endpoint <a href="#xian-liu" id="xian-liu"></a>

* `POST /sendBatch`
* `gRPC`

### Transaction Construction Example <a href="#jiao-yi-gou-jian-dai-ma-shi-li" id="jiao-yi-gou-jian-dai-ma-shi-li"></a>

* [Curl](request-example/curl.md)
* [Go](request-example/go.md)
* [Rust](request-example/rust.md)
* [JS](request-example/js.md)

### Request Parameter

<table><thead><tr><th width="120.8515625">Parameters</th><th width="115.421875">Mandatory</th><th width="109.98046875">Example</th><th>Description</th></tr></thead><tbody><tr><td>transactions</td><td>Mandatory</td><td>string[]</td><td>Signed transactions, Base64 encoded, maximum 25 transactions</td></tr><tr><td>mode</td><td>Optional</td><td>"fast"<br>"sandwichMitigation"</td><td>BlockRazor offers two modes: Fast and SandwichMitigation, with Fast as the default.<br><br>In fast mode, transactions are sent based on globally distributed high-performance network and high-quality SWQoS, reaching the Leader node with the lowest latency.<br><br>In sandwichMitigation mode, BlockRazoz will route transactions to the trusted SWQoS and skip the slot of the blacklisted Leader (dynamically identified by the BlockRazor sandwich monitoring mechanism). In this mode, <strong>DO NOT</strong> send transactions using durable nonce, as it will cause the sandwich protection to become ineffective.</td></tr><tr><td>safeWindow</td><td>Optional</td><td>3</td><td>safeWindow is used to determine the timing of transaction sending in sandwichMitigation mode and represents the number of consecutive slots of  whitelist validators. For example, if it is set to 3, the transaction will only be sent when 3 consecutive slots from the current slot belong to whitelist validators.<br><br>The range of safeWindow is 3-13. The larger the number, the better the effect of mitigating the sandwich attack, but it may have a certain impact on the rate of inclusion. If not set, the default is 3.</td></tr><tr><td>revertProtection</td><td>Optional</td><td>false</td><td>The default value is false. If set to true, the transaction will not fail on chain, but the speed of inclusion will be affected and there is a possibility that it cannot be included. Please choose to enable it carefully according to actual needs.</td></tr></tbody></table>
