---
description: >-
  Global low-latency infrastructure for orderflow projects, trading bots,
  searchers, and quant trading on BSC, Solana, Ethereum, Base, and Robinhood
  Chain.
metaLinks:
  canonical: ./
  alternates:
    - https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/overview
---

# Overview

BlockRazor is a research institution focused on Web3 infrastructure and DeFi trading. It focuses on solving key problems in trading scenarios and continuously transforms its research results into infrastructure products and services, creating a globally distributed, high-performance multi-chain infrastructure system for builders who pursue excellence.

Through long-term research and engineering practice, BlockRazor provides integrated capabilities covering Transaction Submission and Streams for orderflow projects, trading bots, searchers, and quantitative trading systems, helping customers obtain faster block transaction signals and better transaction execution results on BSC, Solana, Robinhood Chain, Ethereum and Base.

### Get Started

<mark style="color:$info;">Service</mark>

<table data-card-wrap="false" data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Obtain Auth Token</td><td><a href="https://blockrazor.io/#/register?redirect=onboarding">https://blockrazor.io/#/register?redirect=onboarding</a></td></tr><tr><td>Start for Free</td><td><a href="get-started/start-for-free.md">start-for-free.md</a></td></tr><tr><td>Pricing</td><td><a href="get-started/pricing.md">pricing.md</a></td></tr></tbody></table>

<mark style="color:$info;">Use Case</mark>

<table data-card-wrap="false" data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Trading Bot</td><td><a href="get-started/use-cases/trading-bot.md">trading-bot.md</a></td></tr><tr><td>Searcher</td><td><a href="get-started/use-cases/searcher.md">searcher.md</a></td></tr><tr><td>Quant Trading</td><td><a href="get-started/use-cases/quant-trading.md">quant-trading.md</a></td></tr><tr><td>Orderflow Projects</td><td><a href="get-started/use-cases/wallet-dex.md">wallet-dex.md</a></td></tr><tr><td>Individual Trader</td><td><a href="get-started/use-cases/individual-trader.md">individual-trader.md</a></td></tr></tbody></table>

<mark style="color:$info;">Chain</mark>

<table data-card-wrap="false" data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>BSC</td><td><a href="get-started/supported-chains.md#bsc">#bsc</a></td></tr><tr><td>Solana</td><td><a href="get-started/supported-chains.md#solana">#solana</a></td></tr><tr><td>Robinhood Chain</td><td><a href="get-started/supported-chains.md#robinhood-chain">#robinhood-chain</a></td></tr><tr><td>Ethereum</td><td><a href="get-started/supported-chains.md#ethereum">#ethereum</a></td></tr><tr><td>Base</td><td><a href="get-started/supported-chains.md#base">#base</a></td></tr></tbody></table>

### Streams

<mark style="color:$info;">BlockRazor offers a variety of high-performance real-time data streaming capabilities, helping trading systems and infrastructure systems obtain signals earlier and synchronize states faster</mark>

<table data-card-wrap="false" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Public Mempool</strong></td><td><mark style="color:$info;">See BSC pending transactions earlier and gain a decisive edge in smart-money tracking, sniping, copy trading, and backruns.</mark></td><td><a href="streams/public-mempool/bsc/public-mempool.md">public-mempool.md</a></td></tr><tr><td><strong>Private Mempool</strong></td><td><mark style="color:$info;">Access exclusive private order flow as an early alpha signal for faster sniping, copy-trading and backrun opportunities.</mark></td><td><a href="streams/private-mempool.md">private-mempool.md</a></td></tr><tr><td><strong>Block Stream</strong></td><td><mark style="color:$info;">Receive the latest BSC blocks and confirmed transactions with low latency so your strategies can react immediately.</mark></td><td><a href="streams/block-stream/">block-stream</a></td></tr><tr><td><strong>Node Stream</strong></td><td><mark style="color:$info;">Accelerate world-state synchronization of node to keep state-dependent strategies ahead of your competitors.</mark></td><td><a href="streams/node-stream/">node-stream</a></td></tr><tr><td><strong>Network Fee Stream</strong></td><td><mark style="color:$info;">Track gas prices and Tip in real time to optimize every transaction for speed, cost, and inclusion probability.</mark></td><td><a href="streams/network-fee-stream/">network-fee-stream</a></td></tr></tbody></table>

### Transaction Submission

<mark style="color:$info;">BlockRazor offers multiple transaction submission modes to adapt to the different needs of various businesses in terms of mev protection, inclusion speed, and transaction costs</mark>

<table data-card-wrap="false" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>RPC</strong></td><td><mark style="color:$info;">Provides standard JSON-RPC methods, offers MEV protection, low-latency inclusion, and real-time refunds for transactions</mark></td><td><a href="transaction-submission/rpc/">rpc</a></td></tr><tr><td><strong>Block Builder</strong></td><td><mark style="color:$info;">A block building service for BSC, providing users with a high-win-rate block building commitment and low-latency access capabilities.</mark></td><td><a href="transaction-submission/block-builder/">block-builder</a></td></tr><tr><td><strong>Transaction Sending</strong></td><td><mark style="color:$info;">Utilizes</mark> <a href="core-technology/blockchain-edge-fabric.md"><mark style="color:$info;">BEF</mark></a> <mark style="color:$info;">to improve inclusion speed, suitable for users with extremely high requirements for transaction speed.</mark></td><td><a href="transaction-submission/transaction-sending/">transaction-sending</a></td></tr><tr><td><strong>Gas Sponsor</strong></td><td><mark style="color:$info;">Provides gas sponsorship for users whose native tokens are insufficient to cover transaction fees, enhancing the user trading experience.</mark></td><td><a href="transaction-submission/gas-sponsor.md">gas-sponsor.md</a></td></tr></tbody></table>

