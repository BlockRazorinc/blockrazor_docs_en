---
description: >-
  This section introduces benchmark, price and integration methods of the
  BlockRazor Robinhood Chain Node-required Sequencer Feed(Ultra).
---

# Robinhood Chain Node-required Sequencer Feed(Ultra)

### What is Node-required Sequencer Feed(Ultra)

Building on the [standard version](sequencer-feed.md), Node-required Sequencer Feed (Ultra) introduces deeper optimizations to network transmission paths and mechanisms, further reducing the end-to-end latency of Sequencer Feed delivery.

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
