---
description: >-
  Explore BlockRazor BSC RPC with private transaction routing, MEV protection,
  low-latency inclusion, and real-time backrun rebates. See endpoints and setup.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/bsc
---

# BSC RPC: MEV Protection & Backrun Refund

### What Is BSC RPC

BSC RPC is an RPC service provided by BlockRazor for BNB Smart Chain (BSC). It supports commonly used standard JSON-RPC methods, allowing users to query on-chain data and submit transactions. When sending transactions, users can also benefit from transaction privacy, MEV protection, low-latency inclusion, and real-time rebates.

### Why Choose BSC RPC

**Transaction Privacy**: Transactions are submitted through private paths, reducing the risk of exposing transaction intent through public propagation and protecting users against malicious MEV attacks such as sandwich attacks and frontrunning.

**Real-Time Rebates**: Controlled data disclosure enables harmless backruns. By default, 60% of the distributable backrun revenue is returned to users in real time.

**Low-Latency Inclusion**: Powered by BEF path and topology optimization, along with global and regional endpoints, BSC RPC provides more stable transaction submission for latency-sensitive, high-frequency, and multi-region deployments.

**Zero-Barrier Integration**: BSC RPC can be added to a wallet with one click. It uses standard JSON-RPC methods, requires no authentication, and can quickly replace an existing BSC RPC endpoint.

### Who Is BSC RPC For

**Wallets / DEXs**: Teams that want to improve transaction protection and execution quality on BSC while offering rebates to their users.

**Trading Bots / Quant Teams**: Teams focused on low latency, transaction ordering, inclusion stability, and execution quality across regions.

**Searchers**: Professional users who need to submit Bundles or participate in the Orderflow Auction.

**Project Builders**: Projects that require a dedicated RPC, low-latency Builder connections, and custom data disclosure and rebate configurations.

**Individual Traders**: Users who want a safer transaction path on BSC and the opportunity to receive MEV rebates.

### FAQ

<details>

<summary>How can a transaction receive a rebate while still being protected from MEV?</summary>

MEV protection primarily prevents strategies that harm users, such as sandwich attacks and frontrunning. For safe backruns that do not negatively affect the user’s expected execution outcome, BlockRazor can provide eligible Searchers with the necessary information within the user-authorized disclosure scope. A portion of the resulting revenue is then returned to the user.

</details>

<details>

<summary>In which regions is BSC RPC available?</summary>

BSC RPC is currently deployed across NewYork, Tokyo, Frankfurt, and Dublin.

</details>

<details>

<summary>What is the difference between BSC RPC and BSC Block Builder?</summary>

BSC RPC serves as a unified transaction submission endpoint and a standard JSON-RPC access point for wallets, DEXs, trading systems, and individual users. It can also forward Bundles to mainstream Builders with low latency. BSC Block Builder is designed for professional use cases that specifically require block building, transaction ordering, Bundle submission, or private transaction capabilities.

</details>

<details>

<summary>When will the rebate arrive?</summary>

If a transaction has an executable backrun opportunity and the backrun successfully generates revenue, the rebate is usually processed in real time through an on-chain transaction and may be included in the same block as the user’s transaction.

</details>

### Privacy Statement

BlockRazor does not collect users’ personal information, such as IP addresses or location data, for advertising or tracking purposes. Data is processed only when necessary to provide services and improve the user experience. BlockRazor may retain information that is already publicly available on-chain, such as transaction timestamps. For complete details, please refer to the latest BlockRazor Privacy Statement.
