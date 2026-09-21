---
description: >-
  Explore the services, target users, advantages, price, and integration methods
  of BlockRazor Solana Shred Stream.
metaLinks:
  canonical: shred-stream.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/solana/shred-stream
---

# Solana Shred Stream Integration Guide

<details>

<summary><strong>What is Shred Stream</strong></summary>

Shred Stream is a shred data subscription service provided by BlockRazor for Solana. It has been deployed in multiple regions such as Frankfurt, Amsterdam, Tokyo, and New York, providing users worldwide with lower latency shred distribution capabilities.

</details>

<details>

<summary><strong>Who is Shred Stream suitable for?</strong></summary>

* **Validators / RPCs**: For validators and RPC nodes that need to receive Solana Shred as early as possible, Shred Stream can serve as a lower-latency data input source.
* **Trading Bots:** Trading bots that are highly sensitive to Shred arrival time can leverage Shred Stream to receive Shred earlier, allowing for more response time for subsequent transaction parsing and strategy execution.
* **DeFi Builders:** For DeFi system builders who need to quickly detect on-chain changes and shorten data processing paths, Shred Stream offers a lower-latency way to obtain on-chain transactions.

</details>

<details>

<summary><strong>Why choose BlockRazor Shred Stream?</strong></summary>

Shred Stream directly connects to high-staking validators in Solana to obtain shreds through [BEF](../../../core-technology/blockchain-edge-fabric.md). It uses UDP to forward data with minimal hops, resulting in a shorter overall link and faster speed, making it suitable for scenarios with extremely low latency requirements for obtaining Solana transaction data.

</details>

<details>

<summary><strong>After receiving Shreds, how do I perform Shred parsing, and what data can I extract?</strong></summary>

Shred Stream receives raw shred streams from the Solana network and cannot be used directly as structured transaction or block data. It typically requires 3 steps: receiving, recovering, and parsing, to extract usable transaction content.

You can directly refer to the [shreds-subscribe](https://github.com/BlockRazorinc/shreds-subscribe). It provides a complete parsing chain:

1. **UDP Reception:** Continuously receive shreds on the specified port.
2. **FEC Recovery:** Recovering partially lost shreds using the Reed-Solomon algorithm
3. **Deshred + Parsing:** Reassembles the shred into complete block entries and further parse the transactions within them.

After completing this process, you can usually obtain the following information:

* Transactions and transaction events related to the specified account
* Block entries

</details>

<details>

<summary><strong>What is the difference between Shred Stream and Geyser Stream?</strong></summary>

The core difference between the two lies in their data layer and ease of use. Shred Stream transmits lower-level raw shred data, resulting in shorter transmission chains and lower latency, making it suitable for bots, RPCs, and validators with extremely high timeliness requirements. However, the receiving party needs to perform reassembly and parsing themselves.&#x20;

[Geyser Stream](geyser-stream/), on the other hand, transmits structured account, slot, block, and transaction data. Clients can directly subscribe via gRPC, making integration simpler and more suitable for account monitoring, transaction analysis, and stable production environments.&#x20;

If you need earlier transaction signals, choose Shred Stream; if you need more complete and easily consumable real-time on-chain data, choose Geyser Stream.

</details>

### Price

The price is $50 / stream / day and $500 / stream / month. <a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=solana&#x26;serviceId=solana_shreds_stream&#x26;billing=day" class="button primary small">Subscribe</a>

### Instruction

1. Go to [https://www.blockrazor.io/](https://www.blockrazor.io/), click \[Register] in the upper right corner to complete the registration
2. Go to the [Pricing](https://blockrazor.io/#/pricing) page to complete the purchase
3.  Log in to the console, go to Solana - Shred Stream, click Edit, enter IP:Port or domain:Port, and select the region closest to your server.

    <figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Please ensure that the port is open. If your client is deployed on AWS or other cloud services, you should additionally configure inbound rules for the security group
# The steps to open the port are as follows
sudo ufw allow <port>/udp
sudo ufw reload
```
{% endcode %}

4. Complete the payment and return to \[Solana] - \[Shred], click \[Capture] to copy the command

<figure><img src="../../../.gitbook/assets/enode3.png" alt="" width="563"><figcaption></figcaption></figure>

6. Access server to run the command to view the shreds delivered from the relay of Shred Stream
