---
description: >-
  See how wallets/DEXs get transactions faster and mev-protected by using
  Blockrazor services, including RPC and Transaction Sending mode
metaLinks:
  canonical: wallet-dex.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/use-cases/wallet-dex
---

# Global Low-latency Infrastructure for Wallets & DEXs

In the current DeFi ecosystem, wallets andvDEXs are facing increasingly severe challenges in user retention. As the market matures, users are no longer satisfied with basic transaction fulfillment; their expectations for security, convenience, and real-time execution are rising. Furthermore, there is a growing interest in emerging trading models such as copy trading and token sniping.

### Pain Point Analysis

* MEV Attacks and Value Leakage: When swap transactions are submitted to standard RPC nodes, the user's intent is fully exposed in the public Mempool. MEV bots can monitor these transactions and employ "sandwich attacks" to exploit slippage, resulting in execution prices far worse than expected.
* Barriers to Native Token-less Transactions: New users withdrawing from CEXs or multi-chain players often face the dilemma of having no native tokens to pay for Gas fees. This interrupts the transaction flow and lowers conversion rates.
* On-chain Latency: During periods of network congestion or volatility, standard RPC nodes propagate transactions slowly, causing users to miss the optimal timing for on-chain inclusion.

### Services

BlockRazor provides targeted product services for wallets and DEXs to address the aforementioned pain points.

**MEV Protection**: To combat MEV attacks and value leakage, BlockRazor offers MEV-protected RPC services. Transactions are under full-privacy protection during the submission and inclusion process, being shielded from MEV bot monitoring and malicious attacks. Additionally, RPC service allows wallets and DEXs to securely disclose transaction data to earn real-time rebates. Currently, BlockRazor provides [MEV-protected RPC](../../transaction-submission/rpc/) for Ethereum and BSC.

**Gas Sponsor**: To solve the issue of native token shortages, BlockRazor provides [Gas Sponsor ](../../transaction-submission/gas-sponsor-for-blockchain-transactions.md)service. This allows users to perform swaps without paying any native blockchain currency (e.g., ETH, BNB, SOL). For wallets and DEXs, integrating Gas Sponsor brings additional benefits:

* Attract Incremental Traffic: Gas Sponsor serves as a powerful branding and marketing tool to attract new traffic and boost overall trading activity.
* Increase Revenue: By enabling more users to complete swaps, the overall volume increases, generating higher swap fee revenue for the platform.

Currently, BlockRazor provides Gas Sponsor services for Ethereum, BSC, and Solana.

**Transaction Sending Mode**: To address slow on-chain speeds, BlockRazor offers "[Transaction Sending Mode](../../transaction-submission/transaction-sending/)", which utilizes a globally accelerated, high-performance network to achieve the lowest possible latency for transaction inclusion—ideal for global user bases with extreme speed requirements.
