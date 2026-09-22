---
description: >-
  Explore the service, benchmark, price, target users and access methods of Fast
  Submit
metaLinks:
  canonical: fast-submit.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/block-builder/bsc-block-builder-fast-submit
---

# BSC Block Builder Fast Submit Guide

### What is Fast Submit?

Fast Submit is a dedicated transaction submission channel provided by BlockRazor for BSC Block Builder. It optimizes the network path and request processing path of transactions from the client to the Builder, improving submission speed and stability.

In the BSC Builder scenario, transaction quality depends on more than just transaction content, gas, slippage, and timing of delivery. For latency-sensitive users, those deploying across regions, or those prioritizing stability, the path a transaction takes to reach the Builder is itself a part of the commit quality.

Fast Submit does not change the transaction logic, nor does it require users to rewrite the existing submission process. It optimizes the submission link between the client and the Builder, allowing transactions to reach the Builder faster and making the submission link more stable under high load and cross-region scenarios, reducing the additional overhead caused by intermediate forwarding, proxy processing, and public network fluctuations.

### Benchmark

According to benchmark results, Fast Submit can reduce latency by about 50ms on intercontinental routes and about 20ms on inland cross-region routes.

**1. For orderflow projects**\
For orderflow projects such as DEXs and wallets, transaction send timing is not fully controllable. In this case, a shorter submission path means the transaction has a higher probability of entering the Builder’s processing window earlier, helping deliver an optimal 0-block inclusion experience for end users.\
Using BSC’s current block time of about 450ms as a rough reference, 50ms and 20ms of latency reduction correspond to roughly 11% and 4.4% improvements in time-window efficiency. If block time further shortens to around 250ms in the future, the improvement could expand to about 20% and 8%.

**2. For trading systems competing for transaction position**\
For strategy-driven systems such as backrunning, sniping, and copy trading, the value of Fast Submit is not only a better chance of making the target block, but also that under the same competitive conditions and the same target Builder arrival time, it leaves more time available for strategy computation.

### Price

<table><thead><tr><th width="134.90625">Payment</th><th width="370.83984375">Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td><strong>$100</strong> / day<br><strong>$1000</strong> / month</td><td><a href="https://blockrazor.io/#/portal/pricing?purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_fast_submit&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td><strong>$1250 / month</strong><br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Why is Fast Submit faster and more stable?

**Dedicated HTTP domain**

Transaction requests in the regular submission channel pass through the CloudFront proxy layer before reaching the Block Builder, incurring more intermediate forwarding, network hops, and additional TLS and proxy processing overhead. In scenarios with network congestion or high concurrency, the queuing, forwarding, and processing time caused by these additional steps is often amplified, making request latency and fluctuations more pronounced.

In contrast, Fast Submit supports submitting requests via a dedicated HTTP domain, removes the CloudFront proxy layer, compresses the submission path between the client and the Block Builder, and reduces the additional overhead caused by intermediate layers.

**Intercontinental express lines**

In cross-regional scenarios, regular submission channels rely on public network paths, making them susceptible to routing fluctuations, link congestion, and network jitter. Especially during intercontinental transmissions, these unstable factors amplify request latency and volatility, reducing the consistency of the submission link.

Fast Submit submits transactions to Block Builder via intercontinental dedicated network lines, minimizing the impact of public network instability on the request link and providing a more stable and lower latency submission experience.

### What users is Fast Submit suitable for?

Fast Submit is more suitable for users who already have a mature submission process and have specific requirements for latency, stability, and cross-region performance.

* **Searcher**: MEV participants who focus on transaction submission timing, path quality, and cross-regional stability.
* **Trading Bot / Quant Team**: Team with specific requirements for execution speed, high-frequency submissions, and regional consistency.
* **Wallets / DEXs**: Team aiming to optimize the user transaction submission experience without altering the existing sending logic.

### Access method

Developers simply need to switch their existing HTTP submission requests to the dedicated Fast Submit domain. The integration process is as follows:

1. Go to the Pricing page to purchase the package
2. Contact us to obtain your dedicated Fast Submit domain.
3. Keep the original request logic unchanged, and send the existing transaction submission request to this dedicated HTTP domain.
4. The submission latency and stability performance were evaluated by testing in actual deployment areas and compared with the original submission channel.

### Precautions

Fast Submit aims to optimize the speed and stability of transactions reaching the BSC Block Builder, but it does not guarantee that a transaction will be packaged. Fast Submit is an infrastructure optimization capability for the submission chain, not a promise of a inclusion result.

Whether a transaction is ultimately included in a block still depends on a variety of factors beyond the submission chain, including but not limited to: market competition, Builder processing strategy, current block space, transaction quality, and real-time on-chain congestion.
