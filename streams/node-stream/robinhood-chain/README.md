---
description: >-
  This section introduces BlockRazor's Node Stream service for Robinhood Chain,
  primarily Sequencer Feed.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/node-stream/robinhood-chain
---

# Robinhood Chain Sequencer Feed

### What is Sequencer Feed?

The Sequencer Feed is a real-time data stream pushed by the Robinhood Chain Sequencer. Nodes subscribe to the Feed via WebSocket to quickly receive block data and keep track of the latest on-chain status.

For node operators, e.g. searchers, the transmission speed and stability of the Sequencer Feed directly impact the speed at which nodes catch up with blocks and update their state. When the official Feed endpoint experiences issues such as network latency, network jitter, or unstable connections, nodes may fail to receive the latest data in a timely manner, resulting in node height lag and data update delays.

### Why BlockRazor Sequencer Feed

BlockRazor Sequencer Feed provides Robinhood Chain nodes with a more stable and efficient Sequencer Feed access service.

Compared to directly connecting to the official feed endpoint, BlockRazor Sequencer Feed reduces latency caused by network jitter and connection interruptions during feed synchronization by using the nearest access point and optimizing the transmission path.

Furthermore, the BlockRazor Sequencer Feed is compatible with the official standard access method. Nodes only need to replace the Feed URL to receive the latest data faster and more stably, reducing block tracking latency and keeping the on-chain state synchronized in real time.

### FAQ

<details>

<summary><strong>What is the difference between the Node-required Sequencer Feed and the Direct Sequencer Feed?</strong></summary>

<table><thead><tr><th width="127.98828125">Comparison</th><th width="255.9140625">Node-required Sequencer Feed</th><th>Direct Sequencer Feed</th></tr></thead><tbody><tr><td>Access Method</td><td>Must be received through a node</td><td>Clients can connect directly without running a nod</td></tr><tr><td>Block Delivery</td><td>Delivers blocks sequentially by block height without skipping any blocks</td><td>Prioritizes the latest block; intermediate blocks may be skipped during network congestion</td></tr><tr><td>Node State Dependency</td><td>Relies on the node’s low-latency synchronized state</td><td>Does not require a local node to maintain complete and continuous state synchronization</td></tr><tr><td>Deployment Cost</td><td>Requires node deployment, maintenance, and monitoring</td><td>Easy to integrate, with lower operational costs</td></tr><tr><td>Suitable Use Cases</td><td>Backrunning and order flow projects</td><td>Sniping and copy trading</td></tr></tbody></table>

</details>
