---
description: >-
  This section introduces the pain points of quantitative trading systems and
  explains how to use Blockrazor services to improve on-chain certainty.
metaLinks:
  canonical: quant-trading.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/use-cases/algorithmic-trading
---

# Quant Trading

DEX-CEX Arbitrage is a low-risk quantitative trading strategy. Due to factors such as liquidity, market mechanisms, and trading speed, certain trading pairs may have price differences at the same time on different exchanges. The arbitrage strategy involves buying assets on the exchange with lower prices and selling them on the exchange with higher prices, thereby earning the price difference.

Although DEX-CEX arbitrage trading is relatively low-risk, its stable profitability can still be influenced by several factors, with "certainty" being a crucial one.

### Certainty of Price Difference

The price difference determines the arbitrage space, and the price difference is determined by the prices of the target token on both CEX and DEX. Price difference certainty refers to ensuring that a price difference exists for the target token while the price on both CEX and DEX are changing continuously.

#### Certainty of DEX Price

Quantitative trading strategies can use DEX aggregator APIs (such as 1inch) to query prices from multiple DEXs. However, this approach may involve data query lantency, and APIs often have rate limits that require payment to lift. Additionally, they may not be compatible with emerging or niche DEXs, making it impossible to retrieve prices.

Another method is to deploy a full node oneself, synchronize the latest blocks, and directly query the token prices from DEX contracts. This approach can solve the rate limiting and compatibility issues. To reduce data query lantency, it is necessary to synchronize the latest blocks at the earliest opportunity.

#### Certainty of CEX Price

Unlike DEX prices, which are updated per block, the prices of token on CEXs are in continuous real-time flux, which poses a risk.

Suppose the chain where the DEX is located produces a block every 3 seconds. The strategy detects a price difference signal at 1000ms and completes the construction and sending of the DEX transaction at 1600ms. The remaining 1400ms is a period during which the price changes of token on the CEX are beyond the control. If the CEX price experiences significant fluctuations during this time, erasing the price difference or even causing a negative price difference, the strategy will face a loss.

To eliminate risk of price difference and enhance the certainty of CEX prices, the time period that the quantitative strategy cannot control needs to be minimized. In other words, the quantitative strategy needs to send transactions at the last possible moment of the block to ensure that the transaction can be included in the current block while still leaving room for profit.

### Certainty of Inclusion

After confirming that a price difference exists for the target token, the arbitrage strategy will place orders simultaneously on both CEX and DEX. Unlike CEXs, which use order book matching mechanisms, DEX transactions may not be included in the next block, leading to the price difference being erased. Enhancing the inclusion certainty of DEX transactions is also key to the strategy's profitability.

Improving the inclusion certainty of DEX transactions can also be understood as increasing the probability that DEX transactions will be included in the next block. For more details, see the [Analysis of Swap Transaction Elements](https://www.blockrazor.io/#/blogs/20250331swap). For quantitative trading systems that focus on strategy, it is recommended to directly integrate with a professional RPC to increase transaction inclusion rate, thus investing time and energy into the core strategy.

### How does BlockRazor enhance “Certainty”?

To enhance DEX price certainty, a quantitative trading system can use the [Node Stream](../../streams/node-stream/bsc/full-node-synchronization.md) to synchronize the latest blocks obtain the latest DEX trading pair prices at the earliest opportunity.

For CEX price certainty, a quantitative trading system can establish a direct connection with the BlockRazor Builder to send transactions with extremely low latency at the "last moment" of a block, minimizing the risk of CEX price fluctuations. BlockRazor Builder is the top block producer on BSC. If you are interested in establishing a direct connection with BlockRazor Builder, please [contact](https://discord.com/invite/qqJuwRb8Nh) us.

To enhance inclusion certainty, a quantitative trading system can choose to submit strategy transactions to BlockRazor RPC. Based on a globally distributed network, BlockRazor RPC can receive transactions from clients with extremely low latency and then forward these transactions to geographically nearby Builders, which achieves end-to-end low-latency forwarding at the network level and improves the transaction inclusion rate.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption><p>The benchmark client sends transactions to BlockRazor RPC, A RPC, and B RPC, recording the difference between the latest block number at the time of sending the transaction and the block number in which the transaction is included. A smaller difference indicates a faster on-chain speed.</p></figcaption></figure>

The chart data shows that transactions submitted to BlockRazor RPC have a probability of being included in the next block as high as 95%, and 100% probability being included in the next 2 blocks, which significantly surpasses that of competitors.

### How to Use BlockRazor

#### Node Stream

1. [Register](https://www.blockrazor.io/#/register) for BlockRazor
2. [Log in](https://www.blockrazor.io/#/login) to BlockRazor and subscribe to the Tier2 or Tier1 plan
3. Configure the full node, for details, see [here](https://blockrazor.gitbook.io/blockrazor/tc/gao-xing-neng-wang-luo-fu-wu/bsc/quan-jie-dian-tong-bu)

#### Integrate Scutum RPC

BlockRazor RPC will generate a dedicated RPC URL for quantitative trading systems registered with BlockRazor. The dedicated RPC supports same JSON RPC methods as other Public RPC. The specific steps are as follows:

1. [Register](https://www.blockrazor.io/#/register) for BlockRazor
2. [Login](https://www.blockrazor.io/#/login) to the portal of BlockRazor
3. Go to the RPC module to view your dedicated RPC (currently supporting Ethereum and BSC). You can configure RPC parameters with one click, and then copy the RPC URL.
4. Go to the project of quantitative trading system, locate the chain's RPC configuration file, and replace the default endpoint with the dedicated RPC URL.
5. Update and publish the project of quantitative trading system(optional).
6. Execute the strategy by submitting transactions via `eth_sendRawTransaction` to the RPC. If the strategy involves multiple transactions, submit them as a Bundle to the RPC.
7. Log in to the portal of BlockRazor to check the refund and transactions.

#### Direct Connection to Builder

* If you need to establish a direct connection with the BlockRazor Builder, please go to [Discord](https://discord.com/invite/qqJuwRb8Nh) to get in touch with us.
