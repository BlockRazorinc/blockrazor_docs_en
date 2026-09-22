---
description: >-
  Explore the transaction submission modes, including RPC, Block Builder,
  Transaction Sending, and Gas Sponsor, and learn how to choose the appropriate
  mode based on the transation sending scene
metaLinks:
  canonical: overview.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/overview
---

# Blockchain Transaction Submission Guide

### What is Transaction Submission

Transaction Submission is a collection of transaction submission capabilities in BlockRazor. Based on different transaction submission modes, it meets users' needs for fast transaction inclusion, gas sponsorship, MEV protection, and real-time rebates, providing a better trading experience and execution results for wallets, DEXs, Trading Bots, Searchers, and quantitative trading systems.

### What are the modes of Transaction Submission?

**RPC**

RPC is the standard transaction submission entry point for Transaction Submission. Unlike ordinary public RPCs, BlockRazor RPC improves transaction speed and stability while providing private routing, MEV protection, and a refund mechanism, making it suitable as the default transaction channel for most businesses. BlockRazor RPC also provides a bundle submission entry point to meet the needs of scenarios such as Approve + Swap / Backrun / Copy Trading / Sniping.

**Block Builder**

Block Builder is the block building infrastructure provided by BlockRazor on BSC, supporting core capabilities such as bundle submission, private transaction submission, and bundle trace. It enhances block building competitiveness and block success rate through global deployment, low-latency communication with validators, and various block building algorithms.

**Transaction Sending**

Transaction Sending is suitable for scenarios highly sensitive to on-chain latency. It uses [BEF](../core-technology/blockchain-edge-fabric.md) to allow transactions to reach the block-producing node within a shorter time window.

**Gas Sponsor**

Gas Sponsor is a "cost-first" model within Transaction Submission. By paying gas fees on behalf of users, Gas Sponsor allows users to complete transactions without holding native tokens (such as ETH, BNB, and SOL), helping projects lower the barrier to entry for users and optimize trading experience.

### How to choose the Transaction Submission mode

<table><thead><tr><th width="287">Scene</th><th width="223.80859375">User</th><th width="213">Mode</th></tr></thead><tbody><tr><td>Protect against MEV attack and get refunds</td><td>Wallets / DEX</td><td>RPC - RawTransaction</td></tr><tr><td>Extreme speed boost and lower latency</td><td>Wallets / DEX / Trading Bot</td><td>Transaction Sending</td></tr><tr><td>No gas cost for users</td><td>Wallets / DEX</td><td>Gas Sponsor</td></tr><tr><td>Approve + Swap / Backrun / Copy Trading / Sniping </td><td>Wallets / DEX / Trading Bot / Searcher</td><td>RPC - Bundle</td></tr></tbody></table>

### FAQ

<details>

<summary>What is the difference between submitting a Bundle to an RPC and submitting a Bundle to a Block Builder?</summary>

The core difference between the two lies in the different submission paths and final destinations of the Bundle.\
When submitting a Bundle to BlockRazor RPC, BlockRazor RPC forwards the Bundle to mainstream builders with low latency. This approach is more suitable as a unified access point, allowing users to submit Bundles without having to connect to different builders individually.\
When you submit a Bundle to Block Builder, the Bundle is sent directly to BlockRazor Builder. This is more suitable for scenarios where you explicitly want to use BlockRazor Builder capabilities and the access path.

</details>



