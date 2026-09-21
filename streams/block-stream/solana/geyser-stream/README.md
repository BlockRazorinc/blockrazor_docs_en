---
description: >-
  This section introduces the services, application scenarios, key features, and
  integration methods of BlockRazor Solana Geyser Stream.
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/solana/geyser-stream
---

# Solana Geyser Stream

<details>

<summary><strong>What is Geyser Stream</strong></summary>

Geyser is a plugin mechanism for Solana validators that enables real-time transmission of Solana's account, slot, block, and transaction data to external data storage media. Geyser Stream is a high-performance Solana data streaming service launched by BlockRazor based on the Yellowstone gRPC (Geyser plugin), allowing clients to subscribe to real-time Solana data streams with extremely low latency via the gRPC protocol.

</details>

<details>

<summary><strong>Applications of Geyser Stream</strong></summary>

Transaction Monitoring: Track transactions for specific accounts, ideal for following smart money trades or targeting new token launches on platforms like pump.fun.

Account Balance Tracking: Monitor balance changes in designated accounts, enabling real-time price calculations for token pairs in DEX pools (e.g., Raydium, Orca, Jupiter).

Block & Slot Insights: Analyze blocks and slots to assess network consensus and health status.

</details>

<details>

<summary><strong>Key Features of Geyser Stream</strong></summary>

High Performance: Geyser Stream delivers gRPC data streams to clients with ultra-low latency in real time.

Data Integrity: Supports transaction replay for the most recent 500 slots (200 seconds), ensuring seamless data continuity during disconnections.

High Stability: Operates across multiple cloud instances with seamless failover, guaranteeing long-term reliability.

</details>

### Endpoint

<table><thead><tr><th width="171.8828125">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Tokyo</td><td>geyserstream-tokyo.blockrazor.xyz:443</td></tr></tbody></table>

### Pricing

{% hint style="info" %}
Geyser Stream charges based on monthly data usage, with the price remaining consistent across new purchases, renewals, and additional data allowances. <a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=solana&#x26;serviceId=solana_geyser_stream&#x26;billing=month" class="button primary small">Subscribe</a>
{% endhint %}

<table data-search="false"><thead><tr><th width="210.109375">Periodic Data Allowance</th><th width="136.5546875">Discount</th><th>Price / Cycle</th></tr></thead><tbody><tr><td>5 TiB</td><td>100%</td><td>$250</td></tr><tr><td>10 TiB</td><td>100%</td><td>$500</td></tr><tr><td>50 TiB</td><td>100%</td><td>$2500</td></tr><tr><td>100 TiB</td><td>95%</td><td>$4750</td></tr><tr><td>150 TiB</td><td>90%</td><td>$6750</td></tr><tr><td>200 TiB</td><td>85%</td><td>$8500</td></tr><tr><td>250 TiB</td><td>80%</td><td>$10000</td></tr></tbody></table>

#### Procurement Instructions

<table><thead><tr><th width="101.15625"></th><th>New procurement</th><th>Renewal</th><th>Add periodic data allowance</th></tr></thead><tbody><tr><td>Scene</td><td>Geyser Stream for initial purchase or repurchase after traffic expires</td><td>Renew your subscription within the Geyser Stream traffic period</td><td>The Geyser Stream cycle data allowance was exhausted prematurely and needs to be replenished.</td></tr><tr><td>Result</td><td>Periodic traffic is generated based on the procurement duration.</td><td>Delayed traffic cycle</td><td>The validity period will not be extended; only the data allowance within the period will be increased.</td></tr><tr><td>Example</td><td><p></p><p>If a new 5 TiB cycle is purchased, the available flow rate will be:</p><ul><li>Starting cycle: 5 Tib</li></ul></td><td><p></p><p>If you purchase 5 TiB for one cycle and renew for one cycle within the initial cycle, the available traffic will be:</p><ul><li>Starting cycle: 5 TiB</li><li>Period 2: 10 Tib</li></ul></td><td><p></p><p>5 TiB was purchased for one cycle, and 10 TiB was renewed for another cycle. During the initial cycle, the data was found to be exhausted. Therefore, an additional 5 TiB was selected to be added to the initial cycle.</p><ul><li>Starting cycle: 5 Tib + 5 Tib</li><li>Period 2: 10 Tib</li></ul></td></tr></tbody></table>

### Integrate Geyser Stream

For auth obtaining steps, see [Authentication](../../../../get-started/authentication.md) guide.

For multi-language integration details, see:

* [CLI](cli.md)
* [Go](go.md)
* [Rust](rust.md)
* [JS](js.md)
