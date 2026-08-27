---
description: >-
  This section introduces the services, application scenarios, and integration
  methods of BlockRazor BSC Public Mempool Tx Trace.
metaLinks:
  canonical: tx-trace.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/public-mempool/bsc/tx-trace
---

# BSC Public Mempool Tx Trace

### What is BSC Tx Trace

Tx Trace is a transaction propagation path observation tool provided by BlockRazor, used to query the propagation path, arrival time, and cross-regional latency distribution of a specified transaction in the global network.

For transaction systems, many issues cannot be determined solely from on-chain receipts. For example, although a transaction may be successfully included on-chain, the region where it first appeared and the time it took to propagate between different regions cannot be directly observed from the on-chain results alone. Based on [BEF](../../../core-technology/blockchain-edge-fabric.md), Tx Trace helps users observe transaction behavior from a perspective closer to the network propagation layer, transforming "latency issues" or "regional difference issues" that previously relied solely on experience into data issues that can be analyzed.

### Scenarios of BSC Tx Trace

**Transaction delay troubleshooting**: When transaction sending results are abnormal, performance is unstable, or the actual execution result is inconsistent with expectations, Tx Trace can be used to view the propagation path and time difference of the transaction in the global network, which can help determine whether the problem lies in the network propagation process.

Multi-region deployment evaluation: When a team deploys a bot or sending services in multiple regions, it can use Tx Trace to compare the entry time and propagation effect of transactions in different regions, and evaluate whether the current deployment truly brings better network coverage and propagation performance.

High-frequency trading path optimization: For trading systems that rely on timing and speed, Tx Trace can be used to analyze the spread of trading in different regions, providing a reference for path optimization.

### Price & Rate Limit

<table><thead><tr><th width="132.03515625">Payment Method</th><th width="197.1875">Rate Limit</th><th width="265.8828125">Price</th><th width="127.80078125">Action</th></tr></thead><tbody><tr><td>Free</td><td>20 requests / day</td><td>Free</td><td>-</td></tr><tr><td>Personalized</td><td>500 requests / day</td><td>$20 / day</td><td><a href="https://blockrazor.io/#/portal/pricing?purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_tx_trace&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>500 requests / day</td><td>$1250 / month<br>packaged with 9 other services</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Endpoint

http://tx-trace.blockrazor.io

### Request Example

```bash
curl -H "Authorization: <YOUR_AUTHORIZATION>" \
  http://tx-trace.blockrazor.io/txtrace/0xea4cac1749fcbfd53d798edf795d80c550aa00873d3acb1019000bf74dd18404
```

### Response Example

**Normal**

```json
{
	"txTrace": [{
		"region": "EU Germany",
		"txTime": "2026-06-03 03:18:10.481",
		"diff": "+0ms"
	},
	{
		"region": "NA US Virginia",
		"txTime": "2026-06-03 03:18:10.510",
		"diff": "+29ms" // The transaction spread from EU Germany to NA US Virginia in 29ms.
	},
	},
	{
		"region": "EU Ireland",
		"txTime": "2026-06-03 03:18:10.515",
		"diff": "+34ms" // The transaction spread from EU Germany to EU Ireland in 34ms.
	},
	{
		"region": "AS Japan",
		"txTime": "2026-06-03 03:18:10.574",
		"diff": "+93ms"
	}],
	"txHash": "0xe55b39c4dead92fe956f7ce2d640e0fcf0ce0cd969da9e3f900d493634b64a54",
	"numberOfRegions": 4
}
```

**Abnormal**

```json
{"error":"invalid token"}
```

```json
{"error":"daily limit exceeded"}
```

### FAQ

<details>

<summary>Which transactions can Tx Trace track?</summary>

Tx Trace is primarily suitable for querying the propagation status, arrival time, and latency distribution of transactions that have entered the public propagation process across different regions. If a transaction has not entered the public propagation path, or is outside the valid query window, Tx Trace cannot provide corresponding propagation results.

</details>

<details>

<summary>What is the difference between Tx Trace and Public Mempool?</summary>

The two have different positioning:

* Public Mempool is a real-time subscription service used for low-latency reception of pending transactions in public circulation, with the emphasis on "seeing transactions as early as possible".
* Tx Trace is a propagation observation tool used to query the propagation path and cross-regional latency distribution of a specified transaction within a global network, focusing on "seeing how transactions propagate."

</details>
