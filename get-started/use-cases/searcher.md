---
description: >-
  See how MEV searchers use BlockRazor for earlier on-chain signals and faster
  transaction submission.
metaLinks:
  canonical: searcher.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/use-cases/searcher
---

# Multi-Chain Searcher Infra: From Signal Acquisition to Transaction Submission

At BlockRazor, we provide multi-chain infrastructure for backrun and arbitrage searchers, covering signal acquisition, state synchronization, and transaction submission. We currently support Solana, BSC, Ethereum, Base, and Robinhood Chain.

### The multi-chain searcher workflow

A searcher's alpha strategy identifies opportunities, calculates expected returns, and controls risk. Our infrastructure is designed to shorten signal acquisition, state synchronization, and transaction submission so that the strategy has more time to compute.

We divide backrun and arbitrage workflows into four stages so that searchers can select services from their actual requirements:

<table><thead><tr><th width="185.00390625">Stage</th><th>Searcher question</th><th>Typical data or interfaces across chains</th></tr></thead><tbody><tr><td>Acquire signals</td><td>How can the strategy see transactions, blocks, or ordering data earlier?</td><td>Public/Private Mempool, Block Stream, Sequencer Feed, Shred/Geyser Stream</td></tr><tr><td>Synchronize state</td><td>How can the strategy obtain current state for simulation and risk checks?</td><td>Local full node, Node Stream, Node-required Sequencer Feed</td></tr><tr><td>Run strategy logic</td><td>How can the system parse, simulate, filter opportunities, and construct transactions?</td><td>The searcher's own strategy, node, simulator, and risk-control system</td></tr><tr><td>Submit transactions</td><td>How should a raw transaction, batch, or bundle reach the correct execution path?</td><td>RPC, Transaction Sending, Bundle API, Block Builder</td></tr></tbody></table>

{% hint style="warning" %}
We do not treat a signal stream as complete state suitable for simulation. BSC NewBlocks and Robinhood Chain Direct Sequencer Feed can provide fresher block signals, but neither replaces complete local world state maintained by the searcher.
{% endhint %}

### Multi-chain capabilities we provide for searchers

<table><thead><tr><th width="153.27734375">Network</th><th>Signal and state capabilities</th><th>Transaction-submission capabilities</th></tr></thead><tbody><tr><td>BSC</td><td>Public Mempool, Private Mempool, NewBlocks, Full Node Synchronization, Network Fee Stream</td><td>RPC, Orderflow Auction, Block Builder, Broadcast Tx</td></tr><tr><td>Robinhood Chain</td><td>Node-required Sequencer Feed and Direct Sequencer Feed, both in Standard and Ultra tiers</td><td><code>eth_sendRawTransaction</code>, <code>eth_sendBatch</code></td></tr><tr><td>Ethereum</td><td>Public Mempool, NewBlocks, CL/EL Client Sync</td><td>RPC, Transaction Sending</td></tr><tr><td>Solana</td><td>Shred Stream, Geyser Stream, Network Fee Stream</td><td>Transaction Sending</td></tr><tr><td>Base</td><td>Block Stream</td><td>RPC, Transaction Sending</td></tr></tbody></table>

### Onchain scenario example: BSC

On BSC, we serve three critical parts of the searcher workflow: receiving pending transactions earlier, synchronizing current blocks or local world state, and submitting raw transactions or bundles through the appropriate RPC or builder path.

#### BSC scenario 1: Find opportunities in public pending transactions

If your backrun or arbitrage strategy depends on unconfirmed transactions already propagating through the public BSC network, we recommend [BSC Public Mempool](https://docs.blockrazor.io/streams/public-mempool/bsc/public-mempool).

We recommend using Public Mempool to:

* Monitor pending transactions that may trigger a backrun or arbitrage opportunity
* Track target addresses, contracts, and transaction types as arbitrage signals
* Discover backrun opportunities from public transactions
* Feed cross-market, cross-protocol, or state-change arbitrage systems in real time

```
BSC Public Mempool
      ↓
Transaction parsing and opportunity detection
      ↓
Simulation and risk controls using current state
      ↓
Raw transaction or bundle construction
      ↓
BSC RPC / Block Builder / Broadcast Tx
```

Our Public Mempool covers publicly propagating pending transactions. It does not include private orderflow entering through a BlockRazor private RPC path.

#### BSC scenario 2: Backrun private orderflow

If your backrun strategy needs to process orderflow that entered through a BlockRazor RPC private path and has not propagated publicly, use [BSC Private Mempool](https://docs.blockrazor.io/streams/private-mempool).

Through Private Mempool, we provide authorized and uniformly anonymized disclosed fields. A searcher can parse a Raw Bundle, run simulations and risk checks, and submit a backrun transaction through the `eth_sendMevBundle` method provided by [BSC RPC Orderflow Auction](https://docs.blockrazor.io/transaction-submission/rpc/bsc/orderflow-auction).

```
BSC Private Mempool Raw Bundle
      ↓
Parse disclosed fields and run simulations
      ↓
Construct a backrun transaction
      ↓
eth_sendMevBundle → BSC RPC in the same region
      ↓
Orderflow Auction and downstream builder submission
```

{% hint style="warning" %}
We require matching regions and domains for BSC Private Mempool subscription and bundle submission. Check the current Private Mempool and Orderflow Auction endpoint documentation before integration.
{% endhint %}

#### BSC scenario 3: Choose NewBlocks or Full Node Synchronization

We recommend different state services based on whether the searcher operates a local full node and depends on complete world state:

| Requirement                                                                                                               | Recommended service                                                                                       | Boundary                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| The application only needs low-latency confirmed blocks, transaction lists, block headers, and next-validator information | [NewBlocks](https://docs.blockrazor.io/streams/block-stream/bsc/newblocks)                                | A structured application-facing block stream; it does not provide complete local world state |
| The searcher operates a BSC full node and the strategy depends on local world state                                       | [Full Node Synchronization](https://docs.blockrazor.io/streams/node-stream/bsc/full-node-synchronization) | Accelerates a user-operated node over P2P; it is not a structured block-subscription API     |
| The strategy needs both pending transactions and current local state                                                      | Public/Private Mempool + Full Node Synchronization                                                        | The mempool supplies signals while node synchronization accelerates local state updates      |

#### BSC scenario 4: Choose a submission path based on transaction type

We provide several BSC submission modes. Choose based on whether the transaction is a bundle, requires private routing, or targets a specific builder:

| Submission requirement                                                                             | Recommended capability                                                                                     | Important boundary                                                                         |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Forward a Raw Bundle or backrun bundle to mainstream builders through one endpoint                 | [BSC RPC Orderflow Auction](https://docs.blockrazor.io/transaction-submission/rpc/bsc/orderflow-auction)   | Uses `eth_sendMevBundle`; forwarding does not guarantee inclusion                          |
| Submit directly to BlockRazor Builder                                                              | [BSC Block Builder](https://docs.blockrazor.io/transaction-submission/block-builder)                       | The bundle enters the BlockRazor Builder processing path directly                          |
| Shorten the network and request path to BlockRazor Builder for an existing mature flow             | [Fast Submit](https://docs.blockrazor.io/transaction-submission/block-builder/fast-submit)                 | Optimizes the submission path without changing transaction logic or guaranteeing inclusion |
| Send single or batched raw transactions with speed as the priority and no private-path requirement | [BSC Broadcast Tx](https://docs.blockrazor.io/transaction-submission/transaction-sending/bsc/broadcast-tx) | Transactions enter public propagation and do not receive MEV protection                    |

### Onchain scenario example: Robinhood Chain

On Robinhood Chain, we provide block signals to backrun and arbitrage searchers through the Sequencer Feed rather than a BSC-style public mempool. We offer Node-required and Direct access modes, each available in Standard and Ultra tiers.

#### Choose Node-required or Direct first

<table><thead><tr><th width="172.078125">Mode</th><th width="126.52734375">Node required</th><th>Delivery behavior</th><th>Recommended searcher use</th></tr></thead><tbody><tr><td>Node-required Standard</td><td>Yes</td><td>Delivers blocks sequentially by height without skipping blocks</td><td>Node synchronization, backrunning, orderflow analysis, and arbitrage</td></tr><tr><td>Node-required Ultra</td><td>Yes</td><td>Preserves sequential delivery while further optimizing latency</td><td>Highly competitive backrun, arbitrage, and high-frequency strategies</td></tr><tr><td>Direct Standard</td><td>No</td><td>Prioritizes the latest block and may skip intermediate blocks during congestion</td><td>Arbitrage signal acquisition without complete local state</td></tr><tr><td>Direct Ultra</td><td>No</td><td>Prioritizes the latest block through the Ultra delivery path</td><td>More latency-sensitive arbitrage signal acquisition</td></tr></tbody></table>

#### Robinhood Chain scenario 1: Backrun or arbitrage using complete local state

If your backrun or arbitrage strategy requires continuous blocks, complete local state, or local execution results, we recommend [Node-required Sequencer Feed](https://docs.blockrazor.io/streams/node-stream/robinhood-chain/sequencer-feed).

```
Node-required Sequencer Feed Standard / Ultra
      ↓
Continuous synchronization of a local Robinhood Chain node
      ↓
Local state, simulation, and strategy computation
      ↓
Construct and sign a raw transaction or batch
      ↓
Robinhood Chain Transaction Sending
```

Evaluate our [Node-required Sequencer Feed Ultra](https://docs.blockrazor.io/streams/node-stream/robinhood-chain/sequencer-feed-ultra) when millisecond-level differences have measurable value for your production strategy. Ultra preserves Node-required delivery semantics and optimizes the transmission path.

#### Robinhood Chain scenario 2: Arbitrage signal acquisition without a node

If your arbitrage strategy primarily needs the latest block signal and does not depend on complete continuous state, use our [Direct Sequencer Feed](https://docs.blockrazor.io/streams/node-stream/robinhood-chain/direct-sequencer-feed).

```
Direct Sequencer Feed Standard / Ultra
      ↓
Client parses the latest block and transaction signals
      ↓
Strategy decision and transaction construction
      ↓
Robinhood Chain Transaction Sending
```

Our Direct Feed may skip intermediate blocks during congestion or when the client cannot process data quickly enough. It is not suitable for backrun or arbitrage strategies requiring complete block history or continuous local state. Evaluate [Direct Sequencer Feed Ultra](https://docs.blockrazor.io/streams/node-stream/robinhood-chain/direct-sequencer-feed-ultra) for more latency-sensitive workloads, and configure a reliable state and simulation source separately.

#### Robinhood Chain transaction submission

Our [Robinhood Chain Transaction Sending](https://docs.blockrazor.io/transaction-submission/transaction-sending/robinhood-chain) provides regional endpoints supporting:

* `eth_sendRawTransaction` for one signed raw transaction.
* `eth_sendBatch` for batched transaction submission.

### Benchmark summary

The findings below come from benchmarks published in our GitBook and were rechecked on October 22, 2026. We summarize only the overall results here; use each link for the test environment, samples, metric definitions, and complete data.

#### BSC

* **Public Mempool:** Across the published multi-region tests, BlockRazor delivered the same pending transaction to the client earlier than a regular node in the vast majority of samples. [View the full benchmark](https://docs.blockrazor.io/streams/public-mempool/bsc/public-mempool#benchmark)
* **NewBlocks:** Across the published multi-region tests, BlockRazor delivered the same block to the client earlier in nearly all samples. [View the full benchmark](https://docs.blockrazor.io/streams/block-stream/bsc/newblocks#benchmark)
* **Full Node Synchronization:** Nodes connected to BlockRazor Relay received the same block earlier in the vast majority of published samples. [View the full benchmark](https://docs.blockrazor.io/streams/node-stream/bsc/full-node-synchronization#benchmark)
* **Broadcast Tx:** In paired same-nonce transaction tests, the BlockRazor path more often reached an earlier block or obtained a better transaction index within the same block. [View the full benchmark](https://docs.blockrazor.io/transaction-submission/transaction-sending/bsc/broadcast-tx#benchmark)
* **Block Builder Fast Submit:** The published test reports lower cross-region path latency when submitting to BlockRazor Builder through Fast Submit. [View the full benchmark](https://docs.blockrazor.io/transaction-submission/block-builder/fast-submit#benchmark)

#### Robinhood Chain

* **Node-required Sequencer Feed Ultra:** In the published multi-AZ AWS Ohio test, BlockRazor Ultra Feed delivered matched blocks earlier than the official Feed. [View the full benchmark](https://docs.blockrazor.io/streams/node-stream/robinhood-chain/sequencer-feed-ultra#benchmark)
* **Transaction Sending:** In the published multi-region paired same-nonce transaction tests, BlockRazor regional endpoints achieved the higher inclusion share. [View the full benchmark](https://docs.blockrazor.io/transaction-submission/transaction-sending/robinhood-chain#benchmark)

{% hint style="warning" %}
We recommend reading every benchmark together with its test date, region, endpoint, sample count, comparison target, and metric definition. Lead rate, win rate, inclusion share, and latency percentiles are different metrics and must not be added together or ranked as if they were equivalent.
{% endhint %}

### More supported-chain capabilities

We also provide corresponding data and submission capabilities for backrun or arbitrage systems on Ethereum, Solana, and Base. Refer to each product page for current availability.

#### Ethereum

* Signals: [Ethereum Public Mempool](https://docs.blockrazor.io/streams/public-mempool/ethereum/public-mempool)
* Blocks: [Ethereum NewBlocks](https://docs.blockrazor.io/streams/block-stream/ethereum/newblocks)
* Node synchronization: [Ethereum CL/EL Client Sync](https://docs.blockrazor.io/streams/node-stream/ethereum/cl-el-client-sync)
* Submission: [Ethereum RPC](https://docs.blockrazor.io/transaction-submission/rpc/ethereum/integration) or [Ethereum Broadcast Tx](https://docs.blockrazor.io/transaction-submission/transaction-sending/ethereum/broadcast-tx)

#### Solana

* Signals and blocks: [Shred Stream](https://docs.blockrazor.io/streams/block-stream/solana/shred-stream) or [Geyser Stream](https://docs.blockrazor.io/streams/block-stream/solana/geyser-stream)
* Fee signals: [Network Fee Stream](https://docs.blockrazor.io/streams/network-fee-stream/solana/get-transactionfee)
* Submission: [Solana Transaction Sending](https://docs.blockrazor.io/transaction-submission/transaction-sending/solana/send-transaction)

#### Base

* Block signals: [Base Block Stream](https://docs.blockrazor.io/streams/block-stream/base/get-blockstream)
* Submission: [Base Transaction Sending](https://docs.blockrazor.io/transaction-submission/transaction-sending/base/eth_sendrawtransaction)
* Tip mode: [Base `eth_sendRawTransaction` with Tip](https://docs.blockrazor.io/transaction-submission/transaction-sending/base/eth_sendrawtransaction-tip)

We still use the “signal → state → compute → submit” framework to help searchers select services, but each network requires its own APIs, data fields, and benchmarks.

### FAQ

<details>

<summary><strong>How should I choose between Robinhood Chain Node-required and Direct Feed?</strong></summary>

If your backrun or arbitrage strategy needs continuous blocks, complete local state, or local execution results, we recommend Node-required. If the system has no node and only needs the latest arbitrage signal, Direct may be used, while accounting for the possibility that intermediate blocks may be skipped during congestion.

</details>

<details>

<summary><strong>Does lower latency guarantee inclusion or higher profit?</strong></summary>

No. Our services are designed to shorten the signal, state-synchronization, or submission path, but transaction outcomes still depend on validity, fees, bids, block space, builder or sequencer behavior, onchain state, and market competition.

</details>
