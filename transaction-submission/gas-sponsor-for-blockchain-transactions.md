---
description: Explore BlockRazor's Gas Sponsor, its core features, and access process.
metaLinks:
  canonical: gas-sponsor-for-blockchain-transactions.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/gas-sponsor
---

# Gas Sponsor for Blockchain Transactions

{% hint style="info" %}
BlockRazor Gas Sponsor currently supports BSC, Solana, Ethereum, and Base. Please [contact](https://discord.com/invite/qqJuwRb8Nh) us if you wish to integrate with our Gas Sponsor service.
{% endhint %}

### Introduction

Gas Sponsor is a feature that enables traders to perform token swaps with zero native blockchain currency (e.g., ETH, BNB, SOL) or zero gas fee. The benefits that Gas Sponsor could bring to projects are:

* Trading booster: Gas Sponsor can be used as a powerful branding and marketing tool to attract incremental trading traffic and boost overall trading activity
* Elimination of the biggest trading barrier: Users no longer need to acquire native tokens (e.g., ETH, BNB, SOL) solely to cover gas fees, which removes the largest obstacle to trading, enabling a truly seamless and frictionless trading experience
* Increased Revenue: More users complete swaps → higher overall volume → more swap fee for the wallet or integrated project.

### Gas Sponsor Feature

* Complete compatibility with `eth_sendRawTransaction` / `sendTransaction` by simply changing the endpoint
* Support transaction filtering and cost-control for gas sponsor
* Transactions are forwarded with full privacy, completely shielding against potential MEV attacks
* Transaction are routed via BlockRazor’s global high-performance network to be included with lowest latency

### Gas Sponso Flow（e.g. Ethereum）

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Integration Steps

1. The user initiates a transaction from the client
2. The client calls `pm_isSponsorable` to check whether the transaction is eligible for sponsorship
3. If `pm_isSponsorable` returns true, the user signs the transaction
4. The client calls `eth_sendRawTransaction` to submit the signed raw transaction.
5. BlockRazor appends a sponsorship transaction before the user's transaction in a Bundle, then forwards the Bundle to Builders
6. The user's transaction is included on-chain as part of a sponsored bundle

