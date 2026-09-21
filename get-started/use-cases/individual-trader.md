---
description: >-
  See how to add BlockRazor RPC to your wallet and compare different rpc mode
  including default mode, full privacy mode and max backrun mode
metaLinks:
  canonical: individual-trader.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/use-cases/individual-trader
---

# BlockRazor RPC for Individual Traders

For users who frequently trade on DEXs, there is a high probability that their trades will execute at the maximum slippage limit, resulting in "invisible" financial losses. Individual traders can now [add](individual-trader.md#how-to-add-rpc-to-my-wallet) the BlockRazor RPC to their wallets with a single click. From then on, all initiated swap transactions will benefit from the deep protection of the BlockRazor RPC, reducing slippage losses while offering the opportunity to receive real-time transaction rebates. Currently, BlockRazor supports RPC integration for individual traders on Ethereum and BSC.

Advanced users can choose to [add different modes of the RPC](individual-trader.md#how-to-add-custom-rpc-to-wallet) according to your needs. If you have any questions, please contact us on [Discord](https://discord.com/invite/qqJuwRb8Nh).

### Add RPC to your wallet with one click

{% hint style="info" %}
Currently, only Metamask and OKX wallets support one-click addition. The following steps take adding BSC RPC to OKX wallet as an example.
{% endhint %}

{% stepper %}
{% step %}
Go to [http://blockrazor.io/rpc/?chain=bsc](http://127.0.0.1:4186/rpc/?chain=bsc)
{% endstep %}

{% step %}
Select RPC, click "Connect wallet to add", select your wallet to complete the linking.

<figure><img src="../../.gitbook/assets/image (85).png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
Click "Add this RPC" to Approve its addition.

<figure><img src="../../.gitbook/assets/image (81).png" alt="" width="350"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
RPC added successfully

{% hint style="info" %}
For Metamask wallets, you need to manually switch networks after successfully adding them via RPC.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (86).png" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

### Manually add RPC to wallet

{% stepper %}
{% step %}
Open OKX Wallet, open the network.

<figure><img src="../../.gitbook/assets/image (83).png" alt="" width="351"><figcaption></figcaption></figure>


{% endstep %}

{% step %}
點擊 Add custom network，輸入[網絡信息](individual-trader.md#tong-yong-rpc-wang-luo-xin-xi)Click "Add custom network" and enter the [network information](individual-trader.md#network-information).

<figure><img src="../../.gitbook/assets/image (84).png" alt="" width="346"><figcaption></figcaption></figure>


{% endstep %}

{% step %}
Click Save to complete adding the network.
{% endstep %}
{% endstepper %}

### Network Information

<table><thead><tr><th width="200"></th><th>Ethereum</th><th>BSC</th></tr></thead><tbody><tr><td>Network Name</td><td>Ethereum Mainnet</td><td>BNB Smart Chain Mainnet</td></tr><tr><td>RPC URL</td><td>https://eth.blockrazor.xyz</td><td>https://bsc.blockrazor.xyz</td></tr><tr><td>Chain ID</td><td>1</td><td>56</td></tr><tr><td>Currency Symbol</td><td>ETH</td><td>BNB</td></tr><tr><td>Block Explorer URL</td><td><a href="https://etherscan.io">https://etherscan.io</a></td><td><a href="https://bscscan.com">https://bscscan.com</a></td></tr></tbody></table>

### Comparison of RPC

<table data-search="false"><thead><tr><th width="146"></th><th>default</th><th>fullprivacy</th><th>maxbackrun</th></tr></thead><tbody><tr><td>BSC</td><td>https://bsc.blockrazor.xyz</td><td>https://bsc.blockrazor.xyz/fullprivacy</td><td>https://bsc.blockrazor.xyz/maxbackrun</td></tr><tr><td>Ethereum</td><td>https://eth.blockrazor.xyz</td><td>https://eth.blockrazor.xyz/fullprivacy</td><td>https://eth.blockrazor.xyz/maxbackrun</td></tr><tr><td>MEV Protection</td><td>Protected</td><td>Protected</td><td>Protected</td></tr><tr><td>Transaction Privacy</td><td>Minimal Disclosure</td><td>Full Privacy</td><td>Maximum Disclosure</td></tr><tr><td>Refund Probability</td><td>Moderate</td><td>No refunds</td><td>High</td></tr><tr><td>Refunds</td><td>Supported</td><td>No refunds</td><td>Supported</td></tr><tr><td>Revert Protection</td><td>Not Protected</td><td>Protected</td><td>Protected</td></tr></tbody></table>

**default**

* In default mode, transactions submitted to MEV Protect RPC General for Wallet User only disclose necessary transaction data (hash, logs & state) to Searcher to win refund opportunities while protecting transaction privacy to the greatest extent. At the same time, in order to ensure the speed of transactions being included in blocks, transactions are not under revert protection  (which is consistent with the default RPC of wallet).&#x20;

**fullprivacy**

* In fullprivacy mode, transactions submitted to MEV Protect RPC General for Wallet User will not disclose any transaction data, and Scutum will directly forward the transaction to the mainstream builders. Transactions in this mode are under revert protection. In order to ensure the speed of inclusion in blocks, it is recommended to set priority fee (Ethereum) when sending transactions.

**maxbackrun**

* In the maxbackrun mode, transactions submitted to MEV Protect RPC General for Wallet User will disclose necessary transaction data (hash, to, calldata, functionSelector, logs & state) under the premise of MEV protection to maximize the possibility of obtaining refunds. Transactions are under revert protection. In order to ensure the speed of inclusion in blocks, it is recommended to set priority fee (Ethereum) when sending transactions.



