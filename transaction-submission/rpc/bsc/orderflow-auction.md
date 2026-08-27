---
description: >-
  Introduce how Seachers can integrate BlockRazor BSC RPC’s `eth_sendMevBundle`
  method.
metaLinks:
  canonical: orderflow-auction.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/bsc/orderflow-auction
---

# BSC RPC Orderflow Auction

### Introduction

Searcher can subscribe [Private Mempool](../../../streams/private-mempool.md) to execute the backrun strategy, and then send the backrun bundles to BlockRazor RPC to obtain benefits via backrun auction.

In addition, Searcher can also skip bundle subscription and send the raw bundle directly to BlockRazor RPC. With the high-performance network, BlockRazor can forward the bundle to mainstream builders with extremely low latency, eliminating the need for repeated integrations with each builder.

### Auction Mechanism

<figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption><p>bundle flow</p></figcaption></figure>

#### Bidding timing

Searcher can continuously submit bundles (repeated submissions are not allowed), and BlockRazor RPC will choose the best time to submit winning bundles to top builders. If the bundle has been included in the block or has expired, it will stop being disclosed in the data stream.

#### Auction rules

BlockRazor RPC conducts English bidding based on the bid value, of which the receipt and distribution is realized via smart contract.

#### Bidding method

When constructing a backrun transaction, the backrun contract could call the proxyBid method of the bidding proxy contract as follows.&#x20;

```
interface IProxyBid { 
    function proxyBid(address refundAddress, uint256 refundCfg) external payable; 
}
```

The biding proxy contract address (proxyBidContract), refundAddress and refundCfg can be obtained from [Private Mempool](../../../streams/private-mempool.md) and msg.value(the biding value) must be greater than 0.

The correctness of the parameters will be strictly verified by BlockRazor RPC. Please do not directly transfer to the refundAddress and the address of bidding proxy contract or perform other operations that may cause changes to the balance of the above account.

### RPC Endpoint

{% hint style="info" %}
Please keep the domain for subscribing to the bundle consistent with the domain for sending the bundle. For example, if subscribing to <mark style="color:$info;">https://jp-bscscutum.blockrazor.xyz/stream</mark>, then send the bundle to <mark style="color:$info;">https://jp-bscscutum.blockrazor.xyz</mark>
{% endhint %}

<table><thead><tr><th width="149.453125">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Tokyo</td><td>https://jp-bscscutum.blockrazor.xyz</td></tr><tr><td>New York</td><td>https://us-bscscutum.blockrazor.xyz</td></tr><tr><td>Frankfurt</td><td>https://ger-bscscutum.blockrazor.xyz</td></tr><tr><td>Dublin</td><td>https://ire-bscscutum.blockrazor.xyz</td></tr></tbody></table>

### Request Parameters

#### **Bundle**

<table><thead><tr><th width="138">Parameters</th><th width="123">Mandatory</th><th width="102">Format</th><th width="124">Example</th><th>Remark</th></tr></thead><tbody><tr><td>hash</td><td>optional</td><td>hash</td><td>"0xa06b……f7e8ec"</td><td>The bundle hash received from the data stream, that is, the object being backrun</td></tr><tr><td>txs</td><td>mandatory</td><td>[]bytes</td><td>[ "0xf84a……e54284" ]</td><td>raw tx. If the <code>hash</code> is empty, up to 50 raw txs are allowed to set up, and if the <code>hash</code> is not empty, only 1 raw tx is allowed to set up</td></tr><tr><td>revertingTxHashes</td><td>optional</td><td>[]hash</td><td>["0x1f23……0abb1e"]</td><td>Transactions that allow to be reverted, a subset of txs</td></tr><tr><td>maxBlockNumber</td><td>optional</td><td>uint64</td><td>39177941</td><td>The maximum block number for the bundle to be valid, with the default set to the current block number + 100</td></tr><tr><td><a href="orderflow-auction.md#hint">hint</a></td><td>optional</td><td><a href="orderflow-auction.md#hint">map[string]bool</a></td><td></td><td>See <a href="orderflow-auction.md#hint">hint</a> for details</td></tr><tr><td>refundAddress</td><td>optional</td><td>address</td><td>"0x9abae1b279a4be25aeae49a33e807cdd3ccffa0c"</td><td>If there is a field with a value of true in hint, this field should be set to EOA.</td></tr></tbody></table>

#### hint

The disclosure for the transaction data in field `txs`is set by hint.  If it is set to true, it will be regarded as disclosing the corresponding transaction field. If it is false, it will be regarded as not disclosing the corresponding transaction field. If it is not set, the default is false.

<table data-search="false"><thead><tr><th width="143">Parameters</th><th width="113">Mandatory</th><th width="97">Format</th><th width="104">Example</th><th>Remark</th></tr></thead><tbody><tr><td>hash</td><td>optional</td><td>bool</td><td>true</td><td>transaction hash</td></tr><tr><td>from</td><td>optional</td><td>bool</td><td>false</td><td>sender of the transaction</td></tr><tr><td>to</td><td>optional</td><td>bool</td><td>true</td><td>receiver of the transaction</td></tr><tr><td>value</td><td>optional</td><td>bool</td><td>false</td><td>value being transacted</td></tr><tr><td>nonce</td><td>optional</td><td>bool</td><td>false</td><td>nonce</td></tr><tr><td>calldata</td><td>optional</td><td>bool</td><td>true</td><td>calldata</td></tr><tr><td>functionSelector</td><td>optional</td><td>bool</td><td>true</td><td>the first 4 bytes of the contract function signature hash</td></tr><tr><td>logs</td><td>optional</td><td>bool</td><td>true</td><td>event logs emitted during transaction execution(this field synchronously sets whether to disclose state changes in the state object)</td></tr></tbody></table>

### Request Example

#### **Raw Bundle**

There is no backrun object so the hash field should not be set. The txs in the bundle come from the public mempool or are self-constructed, and up to 50 rawtransactions can be set. Searchers can authorize the disclosure of txs in raw bundles to allow other Searchers to backrun, or they can keep transactions private and Scutum will forward the raw bundles to mainstream builders.

```json
curl -X POST -H "Content-Type: application/json" --data'{
	"id": 1,
	"jsonrpc": "2.0",
	"method": "eth_sendMevBundle",
	"params": [{
		"txs": ["0xf84a8080808080808193a0437a5584216e68d1ff5bd7803161865e058f9bf4637fd1391213eac03ae64444a00df12bffe475d5dd8cc1544b72ee280471f1dcb5173827ba41eb25cfc3e54284"],
		"revertingTxHashes": [],
		"maxBlockNumber": 39177941,
		"hint": {
			"hash": true,
			"from": false,
			"to": false,
			"value": false,
			"nonce": false,
			"calldata": false,
			"functionSelector": false,		
			"logs": true
		},
		"refundAddress": "0x9abae1b279a4be25aeae49a33e807cdd3ccffa0c"
	}]
}'<ETH_NODE_URL>
```

#### First Backrun Bundle

Searcher executes the backrun strategy on Raw Bundle, and can choose to continue to disclose the bundle to other Searchers to execute nested backrun strategies. The overall structure of First Backrun Bundle is generally \[Raw Bundle tx1, backrun tx1].

Searcher executes the backrun strategy on the raw bundle to form the first backrun bundle.  The `hash` field sets the raw bundle hash received in the data stream, and `txs` field sets the backrun tx. Searchers can disclose the backrun bundle to other Searchers to execute nested backrun strategy. The overall structure of the backrun bundle in the data stream is generally \[raw bundle txs…, backrun tx].

```json
curl -X POST -H "Content-Type: application/json" --data'{
	"id": 1,
	"jsonrpc": "2.0",
	"method": "eth_sendMevBundle",
	"params": [{
		"hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
		"txs": ["0xf84a8080808080808193a0437a5584216e68d1ff5bd7803161865e058f9bf4637fd1391213eac03ae64444a00df12bffe475d5dd8cc1544b72ee280471f1dcb5173827ba41eb25cfc3e54284"],
		"revertingTxHashes": [],
		"maxBlockNumber": 39177941,
		"hint": {
			"hash": true,
			"from": false,
			"to": true,
			"value": false,
			"nonce": false,
			"calldata": true,
			"functionSelector": true,		
			"logs": true
		},
		"refundAddress": "0x9abae1b279a4be25aeae49a33e807cdd3ccffa0c"
	}]
}'<ETH_NODE_URL>
```

#### Second Backrun Bundle

Searcher can execute the backrun strategy again on the first backrun bundle submitted by other Searchers, forming a nested bundle that is backrun twice. The `hash` field sets the hash of the first backrun bundle, and `txs` sets the second backrun tx. The overall structure of the second backrun bundle is generally \[raw bundle txs…, first backrun tx, second backrun tx].

{% hint style="info" %}
The second backrun bundle will no longer be disclosed to other Searchers, and the parameters hint, refundRecipient will be invalid.
{% endhint %}

```json
curl -X POST -H "Content-Type: application/json" --data'{
	"id": 1,
	"jsonrpc": "2.0",
	"method": "eth_sendMevBundle",
	"params": [{
		"hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
		"txs": ["0xf84a8080808080808193a0437a5584216e68d1ff5bd7803161865e058f9bf4637fd1391213eac03ae64444a00df12bffe475d5dd8cc1544b72ee280471f1dcb5173827ba41eb25cfc3e54284"],
		"revertingTxHashes": [],
		"maxBlockNumber": 39177941,
	}]
}'<ETH_NODE_URL>
```

### Response Example

**normal**

```json
{"jsonrpc":"2.0","id":1,"result": "0x11111111..."}
```

**abnormal**

```json
{"jsonrpc":"2.0","id":1,"jsonerror":{"code":-38000,"message":"nonce too low: address 0x9Abae1b279A4Be25AEaE49a33e807cDd3cCFFa0C, tx: 0 state: 45"}}
```

### FAQ

<details>

<summary>What is the difference between submitting a Bundle to an RPC and submitting a Bundle to a Block Builder?</summary>

The core difference between the two lies in the different submission paths and final destinations of the Bundle.\
When submitting a Bundle to BlockRazor RPC, BlockRazor RPC forwards the Bundle to mainstream builders with low latency. This approach is more suitable as a unified access point, allowing users to submit Bundles without having to connect to different builders individually.\
When you submit a Bundle to Block Builder, the Bundle is sent directly to BlockRazor Builder. This is more suitable for scenarios where you explicitly want to use BlockRazor Builder capabilities and the access path.

</details>



