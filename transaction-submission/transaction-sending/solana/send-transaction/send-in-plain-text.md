---
description: >-
  Learn the integration of 'Send Transaction in Plain Text' in BlockRazor Solana
  Transaction Sending mode. See endpoints, request example, and request
  parameter.
metaLinks:
  canonical: send-in-plain-text.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-transaction/send-in-plain-text
---

# Solana Send Transaction in Plain Text

### Introduction <a href="#jie-shao" id="jie-shao"></a>

{% hint style="warning" %}
Solana's transaction sending service is not bound to the subscription plan, with rate limit default to 3 TPS. If you need to increase the TPS limit, please [contact](https://discord.com/invite/qqJuwRb8Nh) us and we will handle it as soon as possible.
{% endhint %}

`Send in Plain Text` is used to send signed transaction on Solana based on HTTP. It presents a much more streamlined and rapid method for submitting transactions compared with [Send Transaction](./)

* Bypasses CORS Preflight: It eliminates the delay (50-100ms) that is typically incurred by OPTIONS preflight.
* Plain Text over JSON: Employing a simple plain text transmission circumvents the computational burden associated with parsing JSON. Furthermore, the resulting smaller data size serves to cut down on network transfer time and costs.
* Base64: Comparing with base58, encoding and decoding of Base64 are significantly faster, while its more compact serialization results in a reduced overall body size.

`Send in Plain Text`'s features make it more suitable for front-end transaction applications with a global user base.

### Endpoint <a href="#xian-liu" id="xian-liu"></a>

* `POST /v2/sendTransaction`

### Request Example <a href="#jiao-yi-gou-jian-dai-ma-shi-li" id="jiao-yi-gou-jian-dai-ma-shi-li"></a>

{% tabs %}
{% tab title="CURL" %}
```javascript
curl -X POST 'http://frankfurt.solana.blockrazor.xyz:443/v2/sendTransaction?auth=<auth_token>' \
-H "Content-Type: text/plain" \
-d "<base64_endcoded_tx>"
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note:**

* **the `auth` and `request parameter`  are compulsory to be added in URI params, e.g.,** `http://frankfurt.solana.blockrazor.xyz:443/v2/sendTransaction?auth=<auth_token>&mode=fast&revertProtection=true`
* **the only header permitted in the request is `Content-Type: text/plain`**
* **Tx should be in Base64 encoded**
{% endhint %}

### Request Parameter

<table><thead><tr><th width="111.44921875">Parameters</th><th width="115.421875">Mandatory</th><th width="109.98046875">Example</th><th>Description</th></tr></thead><tbody><tr><td>transaction</td><td>Mandatory</td><td>"4hXTCk……tAnaAT"</td><td>Fully signed transactions, Base64 encoded</td></tr><tr><td>mode</td><td>Optional</td><td>"fast"<br>"sandwichMitigation"</td><td>BlockRazor offers two modes: Fast and SandwichMitigation, with Fast as the default.<br><br>In fast mode, transactions are sent based on globally distributed high-performance network and high-quality SWQoS, reaching the Leader node with the lowest latency.<br><br>In sandwichMitigation mode, BlockRazoz will route transactions to the trusted SWQoS and skip the slot of the blacklisted Leader (dynamically identified by the BlockRazor sandwich monitoring mechanism). In this mode, <strong>DO NOT</strong> send transactions using durable nonce, as it will cause the sandwich protection to become ineffective.</td></tr><tr><td>safeWindow</td><td>Optional</td><td>3</td><td>safeWindow is used to determine the timing of transaction sending in sandwichMitigation mode and represents the number of consecutive slots of  whitelist validators. For example, if it is set to 3, the transaction will only be sent when 3 consecutive slots from the current slot belong to whitelist validators.<br><br>The range of safeWindow is 3-13. The larger the number, the better the effect of mitigating the sandwich attack, but it may have a certain impact on the rate of inclusion. If not set, the default is 3.</td></tr><tr><td>revertProtection</td><td>Optional</td><td>false</td><td>The default value is false. If set to true, the transaction will not fail on chain, but the speed of inclusion will be affected and there is a possibility that it cannot be included. Please choose to enable it carefully according to actual needs.</td></tr></tbody></table>

### Response

<table><thead><tr><th width="141.20703125">Status Code</th><th width="201.421875">Message</th><th>Meaning</th></tr></thead><tbody><tr><td>200</td><td>OK</td><td>The request is normal</td></tr><tr><td>400</td><td>BadRequest</td><td>Invalid parameter</td></tr><tr><td>403</td><td>Forbidden</td><td>Request denied, as the authentication (auth) is empty, invalid, or expired.</td></tr><tr><td>500</td><td>InternalServerError</td><td>The server encountered an unexpected condition that prevented it from fulfilling the request</td></tr></tbody></table>

111111111



