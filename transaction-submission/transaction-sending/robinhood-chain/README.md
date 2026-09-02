---
description: >-
  This page introduces the price and benchmark of Robinhood Chain Transaction
  Sending
metaLinks:
  canonical: ./
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/robinhood-chain
---

# Robinhood Chain Transaction Sending

### What is Robinhood Chain <a href="#what-is-robinhood-chain" id="what-is-robinhood-chain"></a>

Robinhood Chain is a Ethereum Layer 2 built on Arbitrum, optimized for tokenized real-world assets including equities and ETFs, enabling 24/7 onchain trading and self-custody.

### Price

<table><thead><tr><th width="216.2578125">User Type</th><th width="237.40234375">Limit</th><th>Price</th></tr></thead><tbody><tr><td>New registered users</td><td><code>eth_sendRawTransaction</code><br>1 Tx / 5s<br><br><code>eth_sendBatch</code><br>1 Tx / 5s</td><td>Free</td></tr><tr><td>Paid users</td><td><code>eth_sendRawTransaction</code><br>10 Txs / 1s<br><br>eth_sendBatch<br>10 Tx / 1s</td><td>$100 / day<br>$1000 /  month<br><br><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=robinhood&#x26;serviceId=robinhood_rpc_send_tx&#x26;billing=day" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Benchmark

We deployed test clients across AWS regions in Frankfurt, Ohio, and Japan, and sent 50 pairs of transactions with identical nonces to both the corresponding regional BlockRazor endpoints and the official RPC endpoints. Within each transaction pair, all parameters were kept exactly the same except for the recipient address, which was used to distinguish between the two submission channels.

Performance was evaluated by comparing the ratio of transactions that were ultimately included through each channel. A higher on-chain inclusion rate indicates faster transaction propagation and execution performance. The benchmark results are shown below.

<table><thead><tr><th width="174.3359375">Region</th><th>BlockRazor Inclusion Rate</th><th>Robinhood Inclusion Rate</th></tr></thead><tbody><tr><td>Frankfurt</td><td>88%</td><td>12%</td></tr><tr><td>Ohio</td><td>50%</td><td>50%</td></tr><tr><td>Japan</td><td>96%</td><td>4%</td></tr></tbody></table>
