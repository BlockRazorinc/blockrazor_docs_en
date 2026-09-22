---
description: >-
  Explore BlockRazor Block Builder: Who is it suitable for? What capabilities
  does it provide? And how to choose endpoint?. See the full developer guide.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder
---

# BSC Block Builder APIs & Integration

### What is Block Builder?

Block Builder is a BSC block building service provided by BlockRazor. It maintains competitive block building and high block production success rate based on [BEF](../../core-technology/blockchain-edge-fabric.md) and Flow Coordination Engine. Block Builder currently ranks first in block production rate across the entire BSC chain. Real-time block production rate can be viewed at [https://dune.com/bnbchain/bnb-smart-chain-mev-stats](https://dune.com/bnbchain/bnb-smart-chain-mev-stats).

Block Builder is designed for users who require high-quality BSC transaction execution. Its core value includes:

* Higher win rate block building capability
* Low-latency access capabilities for global deployment
* Supports multiple transaction submission modes to adapt to different execution needs.
* Provide more suitable submission paths for latency-sensitive, privacy-sensitive, and sequence-sensitive scenarios.

### Who is Block Builder suitable for?

* **Searcher**: Users who need to submit bundles, capture MEV opportunities, and pay attention to the order of transactions and the timing of execution.
* **Trading Bot / Quant Team**: Team with specific requirements for latency, execution stability, and cross-regional performance.
* **Wallets / DEXs**: Teams that need a better trade execution experience, privacy protection, or a better submission path.

### What capabilities does Block Builder provide?

Block Builder currently supports the following core capabilities:

* **Send Bundle**: Suitable for scenarios that require transaction order, atomicity, and execution within the same block.
* **Send PrivateTransaction**: Suitable for scenarios where you want to avoid exposing transactions to the public mempool and reduce the risk of being preempted or maliciously observed.
* **Trace Bundle**: Suitable for scenarios that track and analyze Bundle submission results and execution performance. Combined with Bundle Explorer, it can help users observe the performance of Bundle in the Builder chain in a more granular way, providing a reference for strategy optimization, problem investigation and effect review.

### Quick Start

{% stepper %}
{% step %}
Apply for auth

For details, see [Authentication](../../get-started/authentication.md)
{% endstep %}

{% step %}
Select the appropriate capabilities and endpoints based on the scenario and requirements.

* [Send Bundle](send-bundle.md)
* [Send PrivateTransaction](send-privatetransaction.md)
* [Trace Bundle](trace-bundle.md)
* [Call Bundle](call-bundle.md)
{% endstep %}

{% step %}
Verify latency and stability performance in the real deployment area.
{% endstep %}
{% endstepper %}

### FAQ

<details>

<summary>What is the difference between submitting a Bundle to an RPC and submitting a Bundle to a Block Builder?</summary>

The core difference between the two lies in the different submission paths and final destinations of the Bundle.

When submitting a Bundle to BlockRazor RPC, BlockRazor RPC forwards the Bundle to mainstream builders with low latency. This approach is more suitable as a unified access point, allowing users to submit Bundles without having to connect to different builders individually.

When you submit a Bundle to Block Builder, the Bundle is sent directly to BlockRazor Builder. This is more suitable for scenarios where you explicitly want to use BlockRazor Builder capabilities and the access path.

</details>

<details>

<summary><strong>How to understand that the average gas price of a bundle must not be less than 0.05 gwei?</strong></summary>

Assume a bundle contains three transactions: `{tx1, tx2, tx3}`. Since `tx1` comes from the mempool, BlockRazor Builder excludes it and calculates the average gas price using only `tx2` and `tx3`. If `tx3` includes an additional tip paid to the Builder, the tip amount is added to the numerator, while the denominator still includes only the gas used by `tx2` and `tx3`. The formula is:

`(tx2.gasPrice × tx2.gasUsed + tx3.gasPrice × tx3.gasUsed + tx3.tip) / (tx2.gasUsed + tx3.gasUsed)`

</details>
