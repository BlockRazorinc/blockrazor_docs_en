---
description: >-
  Use the BlockRazor Robinhood Chain Send Transaction in Plain Text API. Review
  the parameters and request example.
metaLinks:
  canonical: send-in-plain-text.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/robinhood-chain/eth_sendrawtransaction/send-in-plain-text
---

# Robinhood Chain Send Transaction in Plain Text API

`Send in Plain Text` is used to send signed transaction on Robinhood. It presents a much more streamlined and rapid method for submitting transactions compared with [eth\_sendRawTransaction](./)

* Bypasses CORS Preflight: It eliminates the delay that is typically incurred by OPTIONS preflight.
* Plain Text over JSON: Employing a simple plain text transmission circumvents the computational burden associated with parsing JSON. Furthermore, the resulting smaller data size serves to cut down on network transfer time and costs.

`Send in Plain Text`'s features make it more suitable for front-end transaction applications with a global user base.

### Request parameters

<table><thead><tr><th width="123.7890625">Parameters</th><th width="122.76953125">Mandatory</th><th width="101.30859375">Format</th><th width="106">Example</th><th>Description</th></tr></thead><tbody><tr><td>-</td><td>Mandatory</td><td>String</td><td>"0x…4b"</td><td>Signed raw transaction</td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```bash
curl -X POST 'https://robinhood.blockrazor.io/v2/eth_sendRawTransaction?auth=<auth_token>' \
-H "Content-Type: text/plain" \
-d "<RawTx>"
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note:**

* **the `auth` and `method` are compulsory to be added in URI params, e.g.,** `https://robinhood.blockrazor.io/v2/eth_sendRawTransaction?auth=<auth_token>`
* **the only header permitted in the request is `Content-Type: text/plain`**
{% endhint %}

### Response

<table><thead><tr><th width="141.20703125">Status Code</th><th width="201.421875">Message</th><th>Meaning</th></tr></thead><tbody><tr><td>200</td><td>OK</td><td>The request is normal</td></tr><tr><td>400</td><td>BadRequest</td><td>Invalid parameter</td></tr><tr><td>403</td><td>Forbidden</td><td>Request denied</td></tr><tr><td>500</td><td>InternalServerError</td><td>The server encountered an unexpected condition that prevented it from fulfilling the request</td></tr></tbody></table>

