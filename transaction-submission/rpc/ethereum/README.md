---
description: >-
  Explore BlockRazor Ethereum RPC with private transaction routing, MEV
  protection, low-latency inclusion, and real-time backrun rebates. See
  endpoints and setup.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/ethereum-rpc
---

# Ethereum RPC: MEV Protection & Backrun Refund

### What Is Ethereum RPC

Ethereum RPC is an RPC service provided by BlockRazor for Ethereum. It supports commonly used standard JSON-RPC methods, allowing users to query on-chain data and submit transactions. When sending transactions, users can also benefit from transaction privacy, MEV protection, low-latency inclusion, backrun refunds, and Gas refunds.

### Why Choose Ethereum RPC

**Transaction Privacy**: Transactions are submitted through private paths, reducing the risk of exposing transaction intent through the public mempool and protecting users against malicious MEV attacks such as sandwich attacks and frontrunning.

**Backrun Refunds**: Transactions enter the orderflow under predefined data disclosure rules. Eligible Searchers use the permitted information to independently identify backrun opportunities that do not harm the user’s transaction outcome. If a Searcher’s backrun is successfully included on-chain and generates revenue, the user receives **90% of the distributable backrun revenue by default**.

**Gas Refunds**: In addition to backrun refunds, Ethereum transactions that meet the Ethereum Builder refund policy may receive a Gas refund, reducing the user’s effective transaction cost.

**Low-Latency Inclusion**: Powered by BEF path and topology optimization, BlockRazor Ethereum RPC sends transactions to more effective processing entry points. This reduces propagation latency and network jitter while providing more stable inclusion for latency-sensitive, high-frequency, and multi-region deployments.

**Zero-Barrier Integration**: BlockRazor Ethereum RPC can be added to a wallet with one click and uses standard JSON-RPC methods. The public RPC requires no authentication, allowing users and projects to quickly replace their existing Ethereum RPC. Projects that need custom transaction disclosure rules, refund recipient addresses, dedicated domains, or revert protection can apply for a dedicated RPC.

### Who Is Ethereum RPC For

**Wallets / DEXs**: Teams that want to improve Ethereum transaction protection and execution quality while providing backrun and Gas refunds to their users.

**Trading Bots / Quant Teams**: Teams focused on low latency, transaction privacy, inclusion stability, and execution quality across regions.

**Project Builders**: Projects that require a dedicated RPC, custom data disclosure rules, refund recipient addresses, and revert protection configurations.

**Individual Traders**: Users who want a safer transaction path on Ethereum and the opportunity to receive backrun and Gas refunds.

### FAQ

<details>

<summary>What is the difference between a backrun refund and a Gas refund?</summary>

A backrun refund comes from the distributable revenue generated when a Searcher executes a harmless backrun on the orderflow. A Gas refund is related to the transaction’s Priority Fee and is available to Ethereum transactions that meet the Ethereum Builder refund policy. The refund percentage for both types is 90%.

</details>

<details>

<summary>Does every transaction receive both types of refunds?</summary>

No. A backrun refund requires a Searcher to identify a valid opportunity, successfully submit a backrun, and generate revenue. A Gas refund requires the transaction to meet the current Gas refund policy. A transaction may receive one type of refund, both types, or no refund.

</details>

<details>

<summary>Do I need to modify my transaction code to use Ethereum RPC?</summary>

No. BlockRazor Ethereum RPC is compatible with standard JSON-RPC methods, including `eth_sendRawTransaction`. Individual users can simply replace the RPC in their wallet, while applications and trading systems usually only need to replace their RPC endpoint.

</details>

### Privacy Statement

BlockRazor does not collect users’ personal information, such as IP addresses or location data, for advertising or tracking purposes. Data is processed only when necessary to provide services and improve the user experience. BlockRazor may retain information that is already publicly available on-chain, such as transaction timestamps. For complete details, please refer to the latest BlockRazor Privacy Statement.
