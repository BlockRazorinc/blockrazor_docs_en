---
description: >-
  This section introduces the pain points of Trading Bots in signal listening
  and transaction sending scenarios, and how to use Blockrazor to extend and
  accelerate signal listening and transactions
metaLinks:
  canonical: trading-bot.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/use-cases/trading-bot
---

# Trading Bot

In onchain trading, a Trading Bot's competitiveness depends not only on its strategy, but also on two critical dimensions of speed:

1. **Signal monitoring speed**: How early the bot can detect a new pool, market-opening event, or target-wallet transaction.
2. **Transaction submission speed**: How quickly the bot can deliver a transaction to a Leader, Builder, Validator, or Sequencer.

The complete transaction path is as follows:

> Onchain event → Signal monitoring and parsing → Strategy evaluation → Transaction construction and signing → Transaction submission → Ordering node receives the transaction → Onchain inclusion

Signal monitoring determines when a bot can act, while transaction submission determines when its transaction reaches the chain. Latency at either stage can eliminate the strategy's original advantage.

### Typical Trading Bot Scenarios

#### **Sniper**

A Sniper Bot monitors new token deployments, new liquidity pools, market-opening transactions, or other predefined events. Once a target signal appears, it immediately constructs and submits a transaction.

Its primary objective is to discover trading opportunities earlier, enter the target block or slot, and secure a more favorable ordering position.

#### **Copy Trading**

A Copy Trading Bot monitors the buys, sells, or position changes of a designated Leader wallet. It then constructs a Follower transaction according to predefined amounts, ratios, and strategy rules.

Its primary objective is to reduce the inclusion-time gap between the Leader and Follower and minimize their execution-price difference.

Sniper and Copy Trading Bots monitor different targets: the former focuses on new pools, market openings, and contract events, while the latter focuses on designated wallet activity. However, both depend on the same underlying path: receiving signals, parsing signals, constructing transactions, and submitting them quickly.

### Trading Bot Pain Points

#### **Pain Point 1: Slow Signal Monitoring**

**Waiting for complete blocks delays signals**

Standard RPC or WebSocket services often deliver transaction data only after a node has received, processed, or even reconstructed a block.

This can mean:

* A competitor has already submitted a transaction when a Sniper Bot detects a new pool.
* The market price has already changed when a Copy Trading Bot detects the Leader's transaction.
* The original opportunity decays while the bot waits for data.

**Public nodes introduce longer data paths**

Public nodes may introduce cross-region transmission, multiple proxy layers, shared-resource queues, rate limits during peak traffic, and connection jitter.

Even a difference of tens of milliseconds can directly affect the final execution position in competitive Sniper and Copy Trading scenarios.

**A single data source provides limited signal coverage**

On chains that support Pending Transactions, public and private transactions may propagate through different paths:

* Public Mempool mainly covers publicly broadcast Pending Transactions.
* Private transactions may not appear in the Public Mempool.
* Confirmed onchain signals offer greater certainty but arrive later.

A Trading Bot should select Pending data, private order flow, or block data according to its target chain and strategy.

#### **Pain Point 2: Slow Transaction Submission**

Receiving a signal first does not guarantee execution first. After strategy evaluation, a transaction may still pass through a public RPC, proxy layers, cross-region networks, and intermediate relay nodes.

These stages consume the lead gained during signal monitoring and can cause:

* A Sniper Bot to miss the target block or slot.
* A Copy Trading Bot to land several blocks behind the Leader.
* A Follower to receive a materially worse execution price than the Leader.
* A transaction to fail because the market state has changed.

The destination that must receive a transaction quickly differs by chain:

<table><thead><tr><th width="170.10546875">Chain</th><th width="211.27734375">Transaction Destination</th><th>Primary Speed Consideration</th></tr></thead><tbody><tr><td>Solana</td><td>Current and upcoming Leaders</td><td>Leader routing and SWQoS transmission</td></tr><tr><td>Ethereum</td><td>Builder / Validator</td><td>Faster entry into propagation and block-building paths</td></tr><tr><td>BSC</td><td>Builder / Validator</td><td>Reaching the target block's processing window in time</td></tr><tr><td>Base</td><td>Sequencer</td><td>Shortening the transaction path to the Sequencer</td></tr><tr><td>Robinhood Chain</td><td>Official Sequencer</td><td>Arrival time under FCFS ordering</td></tr></tbody></table>

Note: Robinhood Chain uses First-Come, First-Served ordering. Transaction order depends on arrival time at the Sequencer, and a higher fee cannot move a transaction ahead of one that arrived earlier. The submission path is therefore a direct competitive variable.

### Recommended Services

BlockRazor provides low-latency infrastructure for Sniper and Copy Trading Bots across both signal monitoring and transaction submission.

#### Signal Monitoring Services

<table><thead><tr><th width="151.9765625">Chain</th><th width="207.79296875">Recommended Services</th><th>Differences and Selection Guidance</th></tr></thead><tbody><tr><td>Solana</td><td><a href="../../streams/block-stream/solana/shred-stream.md">Shred Stream</a><br><a href="../../streams/block-stream/solana/geyser-stream/">Geyser Stream</a></td><td>Shred Stream transmits shreds before complete block reconstruction; Geyser Stream provides structured transaction and account data.</td></tr><tr><td>Ethereum</td><td><a href="../../streams/public-mempool/ethereum/public-mempool.md">Public Mempool</a><br><a href="https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/ethereum/newblocks">Block Stream</a></td><td>Public Mempool monitors publicly broadcast Pending Transactions; Block Stream monitors confirmed onchain signals.</td></tr><tr><td>BSC</td><td><a href="../../streams/public-mempool/bsc/public-mempool.md">Public Mempool</a><br><a href="../../streams/private-mempool.md">Private Mempool</a><br><a href="../../streams/block-stream/bsc/newblocks.md">Block Stream</a></td><td>Public and Private Mempool complement each other to provide earlier Pending signals; Block Stream monitors confirmed onchain signals.</td></tr><tr><td>Base</td><td><a href="../../streams/block-stream/base/get-flashblockstream/">FlashBlock Stream</a><br><a href="../../streams/block-stream/base/get-blockstream.md">Block Stream</a></td><td>FlashBlock Stream provides preconfirmation data before a complete block is formed; Block Stream provides complete block data.</td></tr><tr><td>Robinhood Chain</td><td><a href="../../streams/node-stream/robinhood-chain/sequencer-feed.md">Sequencer Feed</a></td><td>Proximity-based access and optimized transmission paths provide faster and more stable delivery of block data pushed by the Sequencer.</td></tr></tbody></table>

> Sniper and Copy Trading Bots can use the same monitoring services. The difference lies in the signals each bot filters and parses.

#### Transaction Submission Services

<table><thead><tr><th width="149.85546875">Chain</th><th width="228.19140625">Recommended Services</th><th>Differences and Selection Guidance</th></tr></thead><tbody><tr><td>Solana</td><td><a href="../../transaction-submission/transaction-sending/solana/send-transaction/">Send Transaction</a></td><td>Uses a global high-performance network and SWQoS paths to deliver transactions quickly to current and upcoming Leaders.</td></tr><tr><td>Ethereum</td><td><a href="../../transaction-submission/transaction-sending/ethereum/broadcast-tx.md">Broadcast Tx</a></td><td>Optimized for maximum broadcast speed but does not provide MEV protection. Use BlockRazor RPC when MEV protection is required.</td></tr><tr><td>BSC</td><td><p><a href="../../transaction-submission/transaction-sending/bsc/broadcast-tx.md">Broadcast Tx</a></p><p><a href="../../transaction-submission/block-builder/fast-submit.md">Fast Submit</a></p></td><td>Broadcast Tx provides maximum transaction broadcast speed but no MEV protection; use BlockRazor RPC when MEV protection is required. Fast Submit uses a dedicated submission entry point and optimized cross-region paths to shorten the route to the BlockRazor Builder.</td></tr><tr><td>Base</td><td><a href="../../transaction-submission/transaction-sending/base/eth_sendrawtransaction.md">eth_sendRawTransaction</a></td><td>Provides standardized, multi-region transaction submission for quickly delivering signed transactions to the Base Sequencer.</td></tr><tr><td>Robinhood Chain</td><td><a href="../../transaction-submission/transaction-sending/robinhood-chain/eth_sendrawtransaction/">eth_sendRawTransaction</a></td><td>Uses multi-region entry points and optimized routing to reach the official FCFS Sequencer faster.</td></tr></tbody></table>

> Signal monitoring services determine when a bot can act. Transaction submission services determine when the transaction reaches the ordering node. Prefer a regional endpoint close to the bot's deployment location.

### Benchmark

BlockRazor has published the following relevant performance tests:

<table data-search="false"><thead><tr><th width="116.55859375">Chain</th><th width="169.1484375">Service</th><th>Benchmark</th><th>What It Measures</th></tr></thead><tbody><tr><td>Solana</td><td>Shred Stream</td><td><a href="https://blockrazor.io/blog/20250818shredbenchmark/">Solana Shred Stream Benchmark</a></td><td>Compares the first-arrival rate and arrival-time difference of BlockRazor and Jito shreds across multiple regions.</td></tr><tr><td>Solana</td><td>Send Transaction</td><td><a href="https://blockrazor.io/blog/20250801Benchmarking/">Benchmarking Solana Send Transaction Service</a></td><td>Uses consistent Tips, Priority Fees, and Durable Nonce transactions to compare transaction races over SWQoS paths.</td></tr><tr><td>BSC</td><td>Fast Submit</td><td><a href="https://blockrazor.io/blog/20260625BSC-Builder-Fast-Submit/">BSC Fast Submit Benchmark</a></td><td>Measures submission-latency improvements from fewer proxies, network hops, and optimized cross-region paths.</td></tr><tr><td>Base</td><td>RPC<br>Block Stream<br>FlashBlock Stream</td><td><a href="https://blockrazor.io/blog/20250922basebenchmark/">Base Benchmark</a></td><td>Compares transaction ordering positions and the data-arrival latency of Block Stream and FlashBlock Stream.</td></tr><tr><td>Robinhood Chain</td><td>Sequencer Feed</td><td><a href="https://docs.blockrazor.io/streams/node-stream/robinhood-chain/sequencer-feed">Sequencer Feed Benchmark</a></td><td>Compares first-arrival rates and latency distributions between BlockRazor and the official Feed across three AWS Ohio Availability Zones.</td></tr><tr><td>Robinhood Chain</td><td>Robinhood Chain RPC</td><td><a href="https://www.blockrazor.io/blog/RobinhoodChainRPC/">Robinhood Chain RPC Benchmark</a></td><td>Uses same-nonce transactions to compare the competitive inclusion rates of BlockRazor RPC and the official RPC across multiple regions.</td></tr></tbody></table>

