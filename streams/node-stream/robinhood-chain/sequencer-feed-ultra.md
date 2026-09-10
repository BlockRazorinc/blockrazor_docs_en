---
description: >-
  This section introduces benchmark, price and integration methods of the
  BlockRazor Robinhood Chain Node-required Sequencer Feed(Ultra).
---

# Robinhood Chain Node-required Sequencer Feed(Ultra)

### What is Node-required Sequencer Feed(Ultra)

Node-required Sequencer Feed (Ultra) is an ultra-low-latency data transmission solution built on the [Standard version](sequencer-feed.md). It deeply optimizes network routing and underlying transmission mechanisms to further reduce the end-to-end latency of Sequencer Feed delivery.

Powered by BEF technology, the Ultra version delivers ordered block data to your local node with the lowest possible latency, enabling trading systems to access critical on-chain state earlier.

The solution is purpose-built for latency-sensitive applications, including advanced arbitrage, order flow analysis, and quantitative trading. In an environment where competition is measured in microseconds, earlier access to ordered block data provides more time for strategy computation and transaction execution. Microseconds define the edge.

### Benchmark

We established WSS connections with both the Robinhood Chain Sequencer Feed and the BlockRazor Sequencer Feed using the same test client. The Robinhood Chain Sequencer Feed endpoint is wss://[feed.mainnet.chain.robinhood.com](http://feed.mainnet.chain.robinhood.com/), and the BlockRazor used the `/ws/ultra` endpoint.

One test client was deployed in each AWS US East (Ohio) Availability Zone (`use2-az1`, `use2-az2`, and `use2-az3`) to compare the relative block delivery latency of the two Sequencer Feeds. For each block, the Sequencer Feed that delivered the block first was assigned a relative latency of `0 ms`. The relative latency of the other feed was calculated from the difference between their block arrival timestamps.

You can use the [robinhood-feed-speed benchmark tool](https://github.com/BlockRazorinc/robinhood-feed-speed) to reproduce the test.

Benchmark data is as follows:

{% tabs %}
{% tab title="use2-az1" %}
Total samples: `4,232`

| Sequencer Feed                 |          P50 |          P90 |          P95 |          P99 |          Max |
| ------------------------------ | -----------: | -----------: | -----------: | -----------: | -----------: |
| **BlockRazor Sequencer Feed**  | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** |
| Robinhood Chain Sequencer Feed |    28.026 ms |    45.177 ms |    52.780 ms |    85.805 ms |   818.664 ms |
{% endtab %}

{% tab title="use2-az2" %}
Total samples: `4,305`

| Sequencer Feed                 |          P50 |          P90 |          P95 |          P99 |          Max |
| ------------------------------ | -----------: | -----------: | -----------: | -----------: | -----------: |
| **BlockRazor Sequencer Feed**  | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** |
| Robinhood Chain Sequencer Feed |    97.404 ms |   195.406 ms |   301.405 ms |   953.111 ms | 1,616.810 ms |
{% endtab %}

{% tab title="use2-az3" %}
Total samples: `4,714`

| Sequencer Feed                 |          P50 |          P90 |          P95 |          P99 |          Max |
| ------------------------------ | -----------: | -----------: | -----------: | -----------: | -----------: |
| **BlockRazor Sequencer Feed**  | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** | **9.580 ms** |
| Robinhood Chain Sequencer Feed |    27.310 ms |    52.828 ms |    66.711 ms |   111.686 ms |   736.300 ms |
{% endtab %}

{% tab title="Tokyo" %}
Total samples: `3,996`

| Sequencer Feed                  |          P50 |          P90 |          P95 |          P99 |           Max |
| ------------------------------- | -----------: | -----------: | -----------: | -----------: | ------------: |
| **BlockRazor Sequencer Feed**   | **0.000 ms** | **0.000 ms** | **0.000 ms** | **0.000 ms** | **68.232 ms** |
|  Robinhood Chain Sequencer Feed |    71.365 ms |   108.596 ms |   116.049 ms |   220.859 ms |   1969.576 ms |


{% endtab %}
{% endtabs %}

Across all three Availability Zones, the BlockRazor Sequencer Feed maintained a relative latency of `0 ms` through P99. In comparison, the Robinhood Chain Sequencer Feed recorded median relative latencies ranging from `27.310 ms` to `97.404 ms`.

The difference was most pronounced in `use2-az2`, where the Robinhood Chain Sequencer Feed reached `97.404 ms` at P50, `953.111 ms` at P99, and a maximum relative latency of `1,616.810 ms`.

In summary, the benchmark results show that the BlockRazor Sequencer Feed consistently delivered blocks earlier and with substantially lower relative latency across all three tested Availability Zones. This provides a faster and more stable first-delivery window for latency-sensitive applications and transactions.

### FAQ

<details>

<summary><strong>What is the difference between the Node-required Sequencer Feed and the Direct Sequencer Feed?</strong></summary>

<table><thead><tr><th width="127.98828125">Comparison</th><th width="255.9140625">Node-required Sequencer Feed</th><th>Direct Sequencer Feed</th></tr></thead><tbody><tr><td>Access Method</td><td>Must be received through a node</td><td>Clients can connect directly without running a nod</td></tr><tr><td>Block Delivery</td><td>Delivers blocks sequentially by block height without skipping any blocks</td><td>Prioritizes the latest block; intermediate blocks may be skipped during network congestion</td></tr><tr><td>Node State Dependency</td><td>Relies on the node’s low-latency synchronized state</td><td>Does not require a local node to maintain complete and continuous state synchronization</td></tr><tr><td>Deployment Cost</td><td>Requires node deployment, maintenance, and monitoring</td><td>Easy to integrate, with lower operational costs</td></tr><tr><td>Suitable Use Cases</td><td>Backrunning and order flow projects</td><td>Sniping and copy trading</td></tr></tbody></table>

</details>

### Price

The price is $200 per unit per day and $2000 per unit per month. <a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_feed_stream_speedup&#x26;billing=day" class="button primary small">Subscribe</a>

### Endpoint

<table><thead><tr><th width="148.26171875">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Ohio</td><td>wss://us.robinhood-feeder.blockrazor.io/ws/ultra/{authToken}</td></tr><tr><td>Tokyo</td><td>wss://jp.robinhood-feeder.blockrazor.io/ws/ultra/{authToken}</td></tr></tbody></table>

### Usage Instructions

{% stepper %}
{% step %}
<a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_feed_stream_speedup&#x26;billing=day" class="button primary small">Subscribe</a> **BlockRazor Sequencer Feed**
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
