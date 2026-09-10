---
description: >-
  This section introduces services, price and integration methods of the
  BlockRazor Robinhood Chain Direct Sequencer Feed.
---

# Robinhood Chain Direct Sequencer Feed(Ultra)

### What Is the Direct Sequencer Feed(Ultra)?

Direct Sequencer Feed (Ultra) is an ultra-low-latency data transmission solution built on the [Standard version](direct-sequencer-feed.md). It deeply optimizes network routing and underlying transmission mechanisms to further reduce the end-to-end latency of Sequencer Feed delivery.

Powered by BEF technology, the Ultra version provides access to the latest block data at exceptional speed—without requiring users to deploy or operate a node.

The solution is purpose-built for advanced sniping and copy-trading strategies where timing is critical. It enables trading systems to capture on-chain activity earlier and secure valuable time for strategy computation and transaction execution—turning every microsecond into a competitive advantage.

### FAQ

<details>

<summary><strong>What is the difference between the Node-required Sequencer Feed and the Direct Sequencer Feed?</strong></summary>

<table><thead><tr><th width="127.98828125">Comparison</th><th width="255.9140625">Node-required Sequencer Feed</th><th>Direct Sequencer Feed</th></tr></thead><tbody><tr><td>Access Method</td><td>Must be received through a node</td><td>Clients can connect directly without running a nod</td></tr><tr><td>Block Delivery</td><td>Delivers blocks sequentially by block height without skipping any blocks</td><td>Prioritizes the latest block; intermediate blocks may be skipped during network congestion</td></tr><tr><td>Node State Dependency</td><td>Relies on the node’s low-latency synchronized state</td><td>Does not require a local node to maintain complete and continuous state synchronization</td></tr><tr><td>Deployment Cost</td><td>Requires node deployment, maintenance, and monitoring</td><td>Easy to integrate, with lower operational costs</td></tr><tr><td>Suitable Use Cases</td><td>Backrunning and order flow projects</td><td>Sniping and copy trading</td></tr></tbody></table>

</details>

### Price

The price is $200 per unit per day and $2000 per unit per month. <a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_direct_feed_stream_speedup&#x26;billing=day" class="button primary small">Subscribe</a>

### Endpoint

<table><thead><tr><th width="148.26171875">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Ohio</td><td>wss://us.robinhood-feeder.blockrazor.io/ws/direct/ultra/{authToken}</td></tr><tr><td>Tokyo</td><td>wss://jp.robinhood-feeder.blockrazor.io/ws/direct/ultra/{authToken}</td></tr></tbody></table>

### Usage Instructions

{% stepper %}
{% step %}
<a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_direct_feed_stream_speedup&#x26;billing=day" class="button primary small">Subscribe</a> **BlockRazor Sequencer Feed**
{% endstep %}

{% step %}
**Retrieve the auth from the portal and append it as the URI to the WSS URL.**

wss://us.robinhood-feeder.blockrazor.io/ws/{authToken}
{% endstep %}

{% step %}
**Choose a WebSocket Client and Establish a WSS Connection**

Use any standard WebSocket client, such as a WebSocket library for Node.js, Go, Python, or Rust. There is no need to deploy a Robinhood Chain node or configure `--node.feed.input.url`.
{% endstep %}

{% step %}
**Receive and parse the data. The parsed data structure is as follows:**

{% code overflow="wrap" %}
```json
{
  "version": 1,
  "messages": [
    {
      "sequenceNumber": 50543784,
      "message": {
        "message": {
          "header": {
            "kind": 3,
            "sender": "0xa4b000000000000000000073657175656e636572",
            "blockNumber": 25872577,
            "timestamp": 1788146984,
            "requestId": null,
            "baseFeeL1": 0
          },
          "l2Msg": {
            "encoding": "Nitro L2 message",
            "kind": 3,
            "kindName": "Batch",
            "decodedByteLength": 3487,
            "transactionCount": 10,
            "items": [
              {
                "index": 0,
                "messageKind": 4,
                "messageKindName": "SignedTx",
                "messageLength": 244,
                "transaction": {
                  "hash": "0xc4d8935f3e63b7b4…1e46fd37",
                  "from": "0x0dd10d651d18bc70594cb070e1117b6cd5e4a8a4",
                  "type": 2,
                  "typeName": "EIP-1559",
                  "chainId": 4663,
                  "nonce": 901,
                  "maxPriorityFeePerGas": 1,
                  "maxFeePerGas": 466288000,
                  "gasLimit": 120000,
                  "to": "0x000000000022d473030f116ddee9f6b43ac78ba3",
                  "value": 0,
                  "input": "0x87517c4500000000…6a94fc2f",
                  "accessList": [],
                  "yParity": 0,
                  "r": "0x1a6881f8f30266eb…090c7b15",
                  "s": "0x15e0901b330dfdba…0e9eb53a",
                  "rawTransaction": "0x02f8f08212378203…0e9eb53a"
                }
              },
              {
                "index": 2,
                "messageKind": 4,
                "messageKindName": "SignedTx",
                "messageLength": 341,
                "transaction": {
                  "hash": "0xef96fbbeb7072b85…6c67ee47",
                  "from": "0x137fe0e0fdbfe6487588f0ef842f418bbb738b8e",
                  "type": 0,
                  "typeName": "Legacy",
                  "chainId": 4663,
                  "nonce": 14,
                  "gasPrice": 250000000,
                  "gasLimit": 400000,
                  "to": "0xcaf681a66d020601342297493863e78c959e5cb2",
                  "value": 40000000000000,
                  "input": "0x04e45aaf00000000…00000000",
                  "v": 9362,
                  "yParity": 1,
                  "r": "0xe65d56b7f3eb63d7…4656bb9c",
                  "s": "0x4362c24ab37d7590…88c290ce",
                  "rawTransaction": "0xf901510e840ee6b2…88c290ce"
                }
              },
              {
                "index": 4,
                "messageKind": 4,
                "messageKindName": "SignedTx",
                "messageLength": 119,
                "transaction": {
                  "hash": "0x00f4bb0661c05fe8…3938c8bf",
                  "from": "0x6ae3de978e63b63e8486d1f495339633495d8264",
                  "type": 2,
                  "typeName": "EIP-1559",
                  "chainId": 4663,
                  "nonce": 3,
                  "maxPriorityFeePerGas": 234192000,
                  "maxFeePerGas": 542634600,
                  "gasLimit": 35010,
                  "to": "0xc45aa399aa75ace0ae56c372898abcb36d33161f",
                  "value": 330170416874734,
                  "input": "0x",
                  "accessList": [],
                  "yParity": 1,
                  "r": "0xdda460f047c07f2a…67caa4a0",
                  "s": "0x5f82128ed043cc80…4d2f6024",
                  "rawTransaction": "0x02f8738212370384…4d2f6024"
                }
              }
            ]
          },
          "l2MsgBase64": "AwAAAAAAAAD0BAL48I…jqF1iw=="
        },
        "delayedMessagesRead": 193164
      },
      "blockHash": "0x220c2e737333e10d…2e261230",
      "signatureV2": "Rg3J2yhBQ8pxYUqc6e…xkNk7wA=",
      "blockMetadata": null
    }
  ]
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
