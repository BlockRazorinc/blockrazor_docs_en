---
description: >-
  This section introduces benchmark, price and integration methods of the
  BlockRazor Robinhood Chain Node-required Sequencer Feed.
metaLinks:
  canonical: sequencer-feed.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/node-stream/robinhood-chain/sequencer-feed
---

# Robinhood Chain Node-required Sequencer Feed

### Benchmark

We established WSS connections with both the Robinhood Chain Sequencer Feed and the BlockRazor Sequencer Feed using the same test client, comparing the relative latency of receiving blocks from both. The Sequencer Feed that received the block first had a relative latency of 0ms, while the Sequencer Feed that received the block later had a relative latency equal to the difference in timestamps between the received blocks. Specific data is as follows:

We established WSS connections with both the Robinhood Chain Sequencer Feed and the BlockRazor Sequencer Feed using one test client deployed in each AWS US East (Ohio) Availability Zone (use2-az1, use2-az2, use2-az3) , comparing the relative latency of receiving blocks from both. The Sequencer Feed that received the block first had a relative latency of 0ms, while the Sequencer Feed that received the block later had a relative latency equal to the difference in timestamps between the received blocks.&#x20;

You can go to [GitHub - BlockRazorinc/robinhood-feed-speed](https://github.com/BlockRazorinc/robinhood-feed-speed) to get the benckmark tool.

Benchmark data is as follows:

{% tabs %}
{% tab title="use2-az1" %}
快照時間：2026-08-12T10:18:57.825039121Z，测试測試區塊總數：252798

<table><thead><tr><th>Sequencer Feed</th><th width="109.0546875">Win rate</th><th width="101.1484375">P50</th><th width="110.5859375">P90</th><th width="105.84375">P95</th><th width="117.4296875">P99</th></tr></thead><tbody><tr><td>BlockRazor Sequencer Feed</td><td><strong>80.27%</strong></td><td><strong>0.000 ms</strong></td><td><strong>0.000 ms</strong></td><td><strong>2.771 ms</strong></td><td><strong>6.116 ms</strong></td></tr><tr><td>Robinhood Chain Sequencer Feed</td><td><strong>19.73%</strong></td><td><strong>4.640 ms</strong></td><td><strong>9.604 ms</strong></td><td><strong>15.651 ms</strong></td><td><strong>19.749 ms</strong></td></tr></tbody></table>
{% endtab %}

{% tab title="use2-az2" %}
快照時間：2026-08-12T10:18:56.218075456Z，测试測試區塊總數：252451

<table><thead><tr><th>Sequencer Feed</th><th width="103.23046875">Win rate</th><th width="108.203125">P50</th><th width="105.90234375">P90</th><th width="101.8671875">P95</th><th width="109.13671875">P99</th></tr></thead><tbody><tr><td>BlockRazor Sequencer Feed</td><td><strong>87.01%</strong></td><td><strong>0.000 ms</strong></td><td><strong>0.445 ms</strong></td><td><strong>1.659 ms</strong></td><td><strong>4.831 ms</strong></td></tr><tr><td>Robinhood Chain Sequencer Feed</td><td><strong>12.98%</strong></td><td><strong>5.302 ms</strong></td><td><strong>17.379 ms</strong></td><td><strong>23.194 ms</strong></td><td><strong>70.723 ms</strong></td></tr></tbody></table>
{% endtab %}

{% tab title="use2-az3" %}
快照時間：2026-08-12T10:18:17.587552975Z，测试測試區塊總數：251670

<table><thead><tr><th>Sequencer Feed</th><th width="97.79296875">Win rate</th><th width="108.33984375">P50</th><th width="103.75">P90</th><th width="107.421875">P95</th><th width="102.87890625">P99</th></tr></thead><tbody><tr><td>BlockRazor Sequencer Feed</td><td><strong>76.48%</strong></td><td><strong>0.000 ms</strong></td><td><strong>1.640 ms</strong></td><td><strong>2.842 ms</strong></td><td><strong>6.336 ms</strong></td></tr><tr><td>Robinhood Chain Sequencer Feed</td><td><strong>23.52%</strong></td><td><strong>2.742 ms</strong></td><td><strong>11.219 ms</strong></td><td><strong>16.373 ms</strong></td><td><strong>32.839 ms</strong></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

In terms of latency distribution, BlockRazor not only arrives first on most blocks, but this lead is also highly consistent; in contrast, Robinhood Chain Sequencer Feed is more often in a lagging position and has more pronounced latency fluctuations.

In summary, BlockRazor Sequencer Feed demonstrates significant advantages in block transmission speed, first-to-delivery rate, and latency stability, providing a more reliable first-to-delivery window for latency-sensitive transactions.

### FAQ

<details>

<summary><strong>What is the difference between the Node-required Sequencer Feed and the Direct Sequencer Feed?</strong></summary>

<table><thead><tr><th width="127.98828125">Comparison</th><th width="255.9140625">Node-required Sequencer Feed</th><th>Direct Sequencer Feed</th></tr></thead><tbody><tr><td>Access Method</td><td>Must be received through a node</td><td>Clients can connect directly without running a nod</td></tr><tr><td>Block Delivery</td><td>Delivers blocks sequentially by block height without skipping any blocks</td><td>Prioritizes the latest block; intermediate blocks may be skipped during network congestion</td></tr><tr><td>Node State Dependency</td><td>Relies on the node’s low-latency synchronized state</td><td>Does not require a local node to maintain complete and continuous state synchronization</td></tr><tr><td>Deployment Cost</td><td>Requires node deployment, maintenance, and monitoring</td><td>Easy to integrate, with lower operational costs</td></tr><tr><td>Suitable Use Cases</td><td>Backrunning and order flow projects</td><td>Sniping and copy trading</td></tr></tbody></table>

</details>

### Price

The price is $80 per unit per day and $800 per unit per month. <a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_feed_stream&#x26;billing=day" class="button primary small">Subscribe</a>

### Endpoint

<table><thead><tr><th width="148.26171875">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Ohio</td><td>wss://us.robinhood-feeder.blockrazor.io/ws/{authToken}</td></tr><tr><td>Tokyo</td><td>wss://jp.robinhood-feeder.blockrazor.io/ws/{authToken}</td></tr></tbody></table>

### Usage Instructions

{% stepper %}
{% step %}
<a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_feed_stream&#x26;billing=day" class="button primary small">Subscribe</a> **BlockRazor Sequencer Feed**
{% endstep %}

{% step %}
**Retrieve the auth from the portal and append it as the URI to the WSS URL.**

wss://us.robinhood-feeder.blockrazor.io/ws/{authToken}
{% endstep %}

{% step %}
**Stop the running Robinhood Chain node**

The specific command depends on the current deployment method, such as Docker, Docker Compose, or systemd. Before stopping, it is recommended to ensure that the node data directory is correctly mounted to avoid losing existing synchronized data after restarting.
{% endstep %}

{% step %}
**Add Feed URL**

The following configuration can be found in the node startup command:

```bash
--node.feed.input.url=wss://feed.mainnet.chain.robinhood.com
```

replace it with BlockRazor Sequencer Feed：

```bash
--node.feed.input.url=wss://feed.mainnet.chain.robinhood.com
--node.feed.input.url=wss://us.robinhood-feeder.blockrazor.io/ws/{authToken}
```

The complete mainnet startup example is as follows:

```bash
DATA_DIR="$HOME/rh/robinhood-nitro-data"

docker run --rm -it \
  -v "$DATA_DIR":/home/nitro/.arbitrum \
  -v "$HOME/rh/config":/home/nitro/config \
  -p 8547:8547 \
  -p 8548:8548 \
  offchainlabs/nitro-node:v3.11.2-3599aca \
    --chain.info-files=/home/nitro/config/robinhood-chain-info.json \
    --parent-chain.connection.url=<L1_EXECUTION_RPC_URL> \
    --parent-chain.blob-client.beacon-url=<L1_BEACON_URL> \
    --init.genesis-json-file=/home/nitro/config/robinhood-genesis.json \
    --node.feed.input.url=wss://<BLOCKRAZOR_FEED_URL> \
    --http.addr=0.0.0.0 \
    --http.port=8547 \
    --http.api=net,web3,eth
```
{% endstep %}

{% step %}
**Restart the node**

After saving the configuration and restarting the node, the node will receive Robinhood Sequencer data via the BlockRazor Sequencer Feed.

Check the node logs to confirm:

* BlockRazor Feed connection successful
* No persistent reconnection, timeout, or WebSocket error.
* The node continuously receives the latest Sequencer data.
* Node height is keeping up with Robinhood Chain
{% endstep %}

{% step %}
**Verify node status**

Check synchronization status:

```bash
curl -d '{"id":0,"jsonrpc":"2.0","method":"eth_syncing","params":[]}' \
  -H "Content-Type: application/json" \
  http://localhost:8547
```

After full synchronization, eth\_syncing should return:

```bash
false
```
{% endstep %}
{% endstepper %}
