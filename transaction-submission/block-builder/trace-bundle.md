---
description: >-
  Use the BlockRazor BSC Block Builder Trace Bundle API. Review the endpoint,
  parameters, request format, response fields, and integration example.
metaLinks:
  canonical: trace-bundle.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder/trace-bundle
---

# BSC Block Builder Trace Bundle API Reference

### Introduction

This method supports querying the current status of the bundle by bundle hash. Please query 5 minutes after sending the bundle to the builder. Endpoint: [https://bsc-bundle-stats.blockrazor.io/](https://bsc-bundle-stats.blockrazor.io/)

### Price

{% hint style="info" %}
Users who purchase the Trace Bundle service can also access the [Bundle Explorer](bundle-explorer.md) in the portal.
{% endhint %}

<table><thead><tr><th width="131.859375">User Type</th><th width="179.0234375">Limit</th><th>Price</th><th>Action</th></tr></thead><tbody><tr><td>Paid User</td><td>1000 txs / day</td><td>$150 / day<br>$1500 / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_bundle_tracing&#x26;billing=day" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Request Parameter

<table><thead><tr><th width="156">Parameters</th><th width="124">Mandatory</th><th width="122">Format</th><th>Example</th><th>Description</th></tr></thead><tbody><tr><td>hash</td><td>Mandatory</td><td>hash</td><td>0x25f9……317097</td><td>bundle hash</td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="HTTPS" %}
```json
curl -X GET "https://bsc-bundle-stats.blockrazor.io/bundlestate?hash=0x25f9fc35e978709195c00e864b9a19fb41ad5c5c5b8a3e003813ae9727317097" \
     -H "Content-Type: application/json" \
     -H "Authorization: M2ZiZj……JhODA1"‍
```
{% endtab %}
{% endtabs %}

### Response Example

**Success**

```json
{
  "bundle": {
    "timestamp": "2025-04-07T09:04:40Z", // The time when the builder receives the bundle (UTC)
    "bundleHash": "0x25f9fc35e978709195c00e864b9a19fb41ad5c5c5b8a3e003813ae9727317097", // bundle hash
    "state": "onchain", // bundle is included on chain
    "blockNumber": 48144954, // The block number where the bundle is located
    "priority": "358911000000000" // bundle value, in wei
  }
}
```

**Error**

```json
{
  "bundle": {
    "timestamp": "2025-04-08T05:21:10Z", // The time when the builder receives the bundle (UTC)
    "bundleHash": "0xe06a923bce1f46b2a0602b8fb2263dffde22f21277b0b71d84b79aff6a58772b", // bundle hash
    "state": "failed", // builder has received bundle，but the bundle is not included
    "err": "non-reverting tx in bundle failed" // reason why the bundle is not included
  }
}
```

```json
{
  "bundle": {
    "bundleHash": "0xabc", // bundle hash in the request
    "state": "not found" // bundle is not found
  }
}
```

