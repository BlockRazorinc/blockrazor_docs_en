---
description: >-
  Explore BlockRazor's Transaction Sending mode, along with the provided
  services and API integration documentation. See the full developer guide.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending
---

# Low-Latency Transaction Sending APIs

### What is Transaction Sending mode

Transaction Sending is a fast transaction sending mode provided by BlockRazor, aimed at users who have higher requirements for the speed of transaction inclusion, and is applicable to Solana, BSC, Robinhood Chain, Ethereum, and Base.

Compared to RPC, Transaction Sending focuses more on how to get transactions into the on-chain execution process more quickly after they are sent from the client. It leverages [BEF](../../core-technology/blockchain-edge-fabric.md) to fully utilize the underlying mechanisms of different chains, providing a lower latency sending experience for the trading system.

### What users are suitable for Transaction Sending mode

* **Wallets / DEXs**: Team aiming to provide a faster sending experience for users worldwide.
* **Trading Bot / Quant Team:** Team with specific requirements for transaction inclusion speed and execution timing

### FAQ

<details>

<summary>What is the difference between Transaction Sending and RPC?</summary>

Both Transaction Sending and RPC are transaction sending capabilities, but they have different design goals.

RPC focuses more on transaction protection and general access capabilities. It provides standard JSON-RPC methods, focusing on addressing the MEV risks that transactions may encounter during public dissemination, and supports refund and disclosure policy configuration and customized RPC access, making it suitable for Wallets, DEXs, and project teams as a standard transaction sending entry point.

Transaction Sending prioritizes the speed of transaction on-chain processing. It optimizes the sending path through [BEF](../../core-technology/blockchain-edge-fabric.md), helping transactions enter the on-chain execution process with lower latency. It is suitable for trading bots, quantitative strategies, and timing-sensitive trading scenarios that have higher requirements for on-chain timeliness.

</details>
