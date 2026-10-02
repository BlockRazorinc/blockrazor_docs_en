---
description: >-
  Compare Node-required, Direct, Standard, and Ultra Robinhood Chain Sequencer
  Feeds. Choose the right mode and access benchmarks, endpoints, and setup
  guides.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/node-stream/robinhood-chain
---

# Robinhood Chain Sequencer Feed Integration

The Robinhood Chain Sequencer Feed is a real-time WebSocket stream published by the Robinhood Chain Sequencer. Nodes and trading systems use it to receive ordered block data, track the latest chain state, and preserve more processing time for latency-sensitive applications such as arbitrage, order flow analysis, quantitative trading, sniping, and copy trading.

The official Robinhood Chain mainnet Sequencer Feed endpoint is:

```
wss://feed.mainnet.chain.robinhood.com
```

BlockRazor provides low-latency Sequencer Feed services compatible with the official integration method. You can receive sequential blocks through a local node with a Node-required Feed, or access the latest blocks without operating a node through a Direct Feed. Both integration modes are available in Standard and Ultra versions.

> **Quick recommendation**
>
> * Choose **Node-required Sequencer Feed** when you need continuous blocks and complete local node state.
> * Choose **Direct Sequencer Feed** when you do not operate a node and primarily need the latest block.
> * Choose the corresponding **Ultra** version when minimizing delivery latency is a higher priority.
> * If you are unsure, compare the four options below.

### Choose a Robinhood Chain Sequencer Feed

| Service                | Node required | Delivery behavior                                                               | Best suited for                                                                | Integration guide              |
| ---------------------- | ------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------ |
| Node-required Standard | Yes           | Delivers blocks sequentially by height without skipping blocks                  | Node synchronization, backrunning, order flow, and quantitative trading        | Standard integration guide     |
| Node-required Ultra    | Yes           | Preserves sequential delivery while further optimizing latency                  | Advanced arbitrage, order flow, and highly competitive quantitative strategies | Ultra integration guide        |
| Direct Standard        | No            | Prioritizes the latest block and may skip intermediate blocks during congestion | Sniping, copy trading, and real-time signal monitoring                         | Direct integration guide       |
| Direct Ultra           | No            | Prioritizes the latest block through the Ultra delivery path                    | Highly latency-sensitive sniping and copy-trading systems                      | Direct Ultra integration guide |

### Node-required vs Direct Sequencer Feed

#### Node-required Sequencer Feed

The Node-required Sequencer Feed is designed for teams operating a Robinhood Chain node. The node subscribes to the Feed through `--node.feed.input.url` and receives blocks sequentially by block height.

This mode is appropriate for systems that depend on continuous blocks, complete local node state, or local execution results, including:

* Backrunning and arbitrage strategies
* Order flow analysis
* Quantitative trading systems
* Searchers that continuously track chain state
* Systems that use a local node for transaction construction or simulation

This integration requires deploying, maintaining, and monitoring a Robinhood Chain node.

#### Direct Sequencer Feed

The Direct Sequencer Feed allows an application to receive the latest block data through a WebSocket connection without deploying a Robinhood Chain node.

It is easier to operate and is suitable for applications focused on current on-chain signals, including:

* Sniping
* Copy trading
* Address and transaction monitoring
* Real-time signal generation
* Trading systems that do not depend on complete local node state

The Direct Feed prioritizes the latest block. Intermediate blocks may be skipped during congestion, so it should not be used for workloads that require a complete and continuous block history.

### Standard vs Ultra

The Standard version is intended for teams that need a stable, low-latency Feed while balancing infrastructure cost.

The Ultra version builds on Standard and uses BlockRazor's Blockchain Edge Fabric and optimized delivery paths to further reduce Sequencer Feed delivery latency.

Ultra is designed for environments where:

* Competition is measured in milliseconds or microseconds.
* Strategies need access to the latest chain state as early as possible.
* The available transaction-construction window is short.
* Standard performance is not sufficient for the production workload.

Ultra does not change the delivery semantics of Node-required or Direct Feed. Before choosing Ultra, determine whether your application requires every block or only the latest block.

### How BlockRazor Reduces Feed Delivery Latency

BlockRazor uses regional access points, optimized network routing, and underlying delivery infrastructure to distribute Robinhood Chain Sequencer Feed data to nodes and direct clients.

For Node-required Feed, operators configure the appropriate BlockRazor Feed URL in the node startup command. Direct Feed clients connect through a standard WebSocket library without configuring or operating a Robinhood Chain node.

The current documentation lists access points in Ohio and Tokyo. Refer to the relevant integration guide for available regions, WSS endpoints, and authentication instructions.

### Benchmark Methodology and Results

BlockRazor benchmarks establish simultaneous WebSocket connections to the official Robinhood Chain Feed and the corresponding BlockRazor Feed from the same test client.

The published methodology is:

1. Establish both WebSocket connections in the same region and test environment.
2. Match data from both feeds by block.
3. Assign `0 ms` relative latency to the Feed that delivers the block first.
4. Calculate the other Feed's relative latency from the difference between the two arrival timestamps.
5. Aggregate P50, P90, P95, P99, maximum latency, and sample counts.

In the Node-required tests published on October 2, 2026, clients were deployed in the AWS Ohio Availability Zones `use2-az1`, `use2-az2`, and `use2-az3`. Under those test conditions, the BlockRazor Feed maintained 100% win rate compared with the official Robinhood Chain Sequencer Feed.

The benchmark tool is available on GitHub:

[BlockRazor Robinhood Feed Benchmark Tool](https://github.com/BlockRazorinc/robinhood-feed-speed)

### Start Your Robinhood Chain Sequencer Feed Integration

#### If you operate a Robinhood Chain node

Start with the Node-required Standard guide:

Integrate Node-required Sequencer Feed

For more latency-sensitive strategies, compare and configure Ultra:

Integrate Node-required Sequencer Feed Ultra

#### If you do not operate a node

Use Direct Feed to access the latest block data:

Integrate Direct Sequencer Feed

For more latency-competitive workloads:

Integrate Direct Sequencer Feed Ultra

You can also review the [Robinhood Chain product and performance overview](https://blockrazor.io/products/robinhood/) to compare Sequencer Feed and Transaction Sending services.

### Frequently Asked Questions

#### What is the Robinhood Chain Sequencer Feed?

The Robinhood Chain Sequencer Feed is a real-time WebSocket stream published by the chain's sequencer. It provides ordered block data to nodes and compatible clients, allowing systems to track the latest chain state without waiting for the same information to become available through ordinary RPC queries.

#### Is BlockRazor the official Robinhood Chain Feed?

No. The official Feed is operated by Robinhood Chain. BlockRazor provides an independent service compatible with the official integration method and uses regional access points and optimized delivery infrastructure to improve Feed delivery speed and stability.

#### Do I need a node to use BlockRazor Sequencer Feed?

Not always. Node-required Feed must be consumed through a Robinhood Chain node and is intended for systems that require continuous blocks and complete local state. Direct Feed allows a client to connect without a node, but it may skip intermediate blocks during congestion.

#### Why can Direct Feed skip blocks?

Direct Feed prioritizes delivery of the latest block. If a client cannot process data quickly enough or the network becomes congested, intermediate blocks may be skipped so the connection can continue with the newest available data. It is therefore intended for real-time signals rather than complete historical synchronization.

#### Should I choose Standard or Ultra?

Choose Standard when you need a stable, low-latency production Feed at the standard service tier. Evaluate Ultra for highly competitive arbitrage, sniping, or other strategies where small delivery-time differences matter. Run an A/B test from your production region before making the final decision.

#### Does 0 ms in the benchmark mean there is no latency?

No. `0 ms` is a relative measurement indicating that the Feed delivered a matching block first in the comparison. It does not mean that the absolute transmission time from the Robinhood Chain Sequencer to the client was zero.

### Next Step

Choose the Feed that matches your node architecture and block-continuity requirements, then use the corresponding integration guide to configure its WSS endpoint, authentication token, and client.

Before switching production traffic, connect to both the official Feed and the BlockRazor Feed from your deployment region. Compare first-arrival rate, P50 and P99 latency, disconnect frequency, and missing-block behavior under your own operating conditions.
