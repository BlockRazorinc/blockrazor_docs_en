---
description: >-
  This section introduces the services, advantages, target users, and access
  methods for BlockRazor BSC full node synchronization.
metaLinks:
  canonical: full-node-synchronization.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/node-stream/bsc/full-node-synchronization
---

# BSC Full Node Synchronization

<details>

<summary><strong>What is Full Node Synchronization</strong></summary>

Full Node Synchronization is a low-latency node synchronization service provided by BlockRazor under Node Stream, which helps users' own BSC full nodes synchronize to the latest blocks and world state faster.

</details>

<details>

<summary><strong>Why choose Full Node Synchronization</strong></summary>

For many high-frequency trading systems and infrastructure systems, "whether the local node can synchronize to the latest state fast enough" is a common problem. Even if the application logic itself is fast, subsequent strategy judgment, data analysis and trade execution calculation will still be affected by the lagging state.

The advantage of Full Node Synchronization is not just "helping nodes connect to the network," but that it provides a low-latency synchronization method more suitable for production environments for systems that rely on local node state. Unlike simply subscribing to block streams, full node synchronization does not continuously push a set of structured data to users. Instead, it allows users' own full nodes to establish P2P connections directly with BlockRazor's high-performance network nodes, leveraging BlockRazor's [BEF](../../../core-technology/blockchain-edge-fabric.md) to receive the latest blocks and state updates from high-quality nodes more quickly.

</details>

<details>

<summary><strong>Which users are suitable for Full Node Synchronization</strong></summary>

* **Quant Team / Trading Bot / Searcher**\
  A quantitative and trading system that relies on the state of local nodes to make strategy judgments, prepare for transactions, or perform on-chain analysis.
* **Infra / Node Teams**\
  The engineering team is responsible for node deployment and status synchronization

If your goal is simply to obtain confirmed block data with low latency, Block Stream is usually sufficient.\
If your system needs its local nodes to synchronize with the latest blocks and world state as quickly as possible, then full node synchronization would be more appropriate.

</details>

### Benchmark

We compared nodes connected to BlockRazor Relay with nodes not connected to Relay in four regions: Dublin, Frankfurt, Tokyo, and Virginia. The evaluation method was based on the block reception logs of nodes, comparing the time difference between the two nodes receiving the same block to verify the improvement effect of full node synchronization on the local full node synchronization speed.

<table><thead><tr><th width="117.09765625">Region</th><th width="264.55859375">Relay-Connected Node Lead Rate</th><th>Avg Lead</th><th>P90 Lead</th></tr></thead><tbody><tr><td>Dublin</td><td>98.92%</td><td>32 ms</td><td>66 ms</td></tr><tr><td>Frankfurt</td><td>98.92%</td><td>44 ms</td><td>63 ms</td></tr><tr><td>Tokyo</td><td>99.51%</td><td>113 ms</td><td>654 ms</td></tr><tr><td>Virginia</td><td>98.64%</td><td>33 ms</td><td>98 ms</td></tr></tbody></table>

The results show that nodes connected to the Relay exhibit a consistent advantage across all four regions. Based on matched block samples, the leading percentage of nodes connected to the Relay in Dublin, Frankfurt, Tokyo, and Virginia reached 98.92%, 98.92%, 99.51%, and 98.64%. This indicates that in the vast majority of comparable samples, nodes connected to the BlockRazor Relay synchronize to new blocks earlier, thus obtaining the latest on-chain state more quickly.

In terms of lead time, the average lead time of nodes connected to the relay in the four regions is approximately 32ms, 44ms, 113ms, and 33ms, respectively; in the P90 dimension, the lead time reaches 66ms, 63ms, 654ms, and 98ms, respectively. This indicates that full-node synchronization not only improves the lead probability but also maintains a considerable synchronization advantage in higher quantile scenarios.

### Price

<table><thead><tr><th width="173.8984375">Payment Method</th><th width="295.85546875">Price</th><th width="254.1796875">Action</th></tr></thead><tbody><tr><td>Personalized</td><td>$80 / enode / day<br>$800 / enode / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_enode&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>$1250 / month<br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Relay IP

<table><thead><tr><th width="160">Region</th><th>Relay IP</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75</td></tr><tr><td>Tokyo</td><td>63.254.162.18</td></tr><tr><td>Dublin</td><td>141.98.217.82</td></tr><tr><td>Virginia</td><td>208.91.105.204</td></tr></tbody></table>

### Instruction

#### Step 1: Purchase Node Stream

1. Go to [https://www.blockrazor.io](https://www.blockrazor.io/), click \[Register] in the upper right corner to complete the registration
2. Log in to the console, go to \[Pricing] - Node Stream, and complete the purchase.
3.  Go to \[Services] - \[Streams] - \[Node Stream], and click \[Edit].

    <figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>
4.  Select the region where Relay is located (it is recommended to select the region closest to the geographical location of your Geth node), enter the Enode of the Ethereum client that needs to be connected, and click \[Confirm] to complete the addition.

    <figure><img src="../../../.gitbook/assets/image (79).png" alt="" width="375"><figcaption></figcaption></figure>
5. Return to the Enode list and click \[Copy Relay Enode]

#### Step 2:  Open ports to allow relay access

{% hint style="info" %}
If your Ethereum client is deployed on AWS or other cloud services, you should additionally configure inbound rules for the security group.
{% endhint %}

1. Access your own Ethereum client's server and execute commands to set up the firewall to allow Relay access.

```
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="63.254.162.18" port port="30311" protocol="tcp" accept'
```

* "source address" is the IP of Relay which can be acquired from [Relay IP](full-node-synchronization.md#relay-ip)
* The port for the Ethereum client to allow Relay access is generally set as the default, which is 30311. You can modify this based on your own node configuration.

2. Reload the firewall configuration to make the changes take effect.

```
sudo firewall-cmd --reload
```

#### Step 3: Set the Relay to a TrustedNode (taking a Geth node as an example)

To ensure a continuous connection between the Geth node and the Relay, it is recommended to add the Relay Enode to the Geth node's config.toml file.

1. In the config.toml file, locate the TrustedNodes field in Node.P2P and add the Relay Enode obtained in step 1.

```scheme
[Node.P2P]
TrustedNodes = ["enode://b5b4e5aa8d8f4568af755af6da0d4642b6475d8d87c3470632bdecab8f54e4e2936ec8ae0d6f34cff8b052235e81a281912c17dfcdbf40d6d3c281b78ada4134"]
```

2. Restart the Geth node, specifying config.toml as the starting command: `--config config.toml`&#x20;

#### Step 4: Check the connection status（Enable the admin namespace in the Geth node as an example）

1. Wait for 10 minutes, access the Geth node, execute the curl command to check the connection status

```json
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"admin_peers","params":[],"id":1}' http://localhost:8545
```

2. In the returned data, query the Relay Enode address (which can be copied from the Portal). If the address is found, it proves that the connection is successful.

```json
[
    {
        "enode": "enode://9ddacbcca0dc1d1b112d470552acc795fce5c3e9f50983fcd5cee7b47289914295acaef3163bea819bcc967461978425def13595deb7de4063295c40e593f320@52.205.173.134:53754",
        "id": "8be29a75ac2cf81e3aa37ccc119630a9dfc43c88d7b5200398a466f5ef9097c4",
        "name": "Geth/v1.4.5/linux-amd64/go1.21.7",
        "caps": [
            "eth/68"
        ],
        "network": {
            "localAddress": "127.0.0.1:30311",
            "remoteAddress": "52.205.173.134:53754",
            "inbound": true,
            "trusted": false,
            "static": false
        },
        "protocols": {
            "eth": {
                "version": 68
            }
        }
    }
]
```

{% hint style="info" %}
If the connection status is found to be abnormal after inquiry, it may be due to network communication problems between nodes. Please go to [Discord](https://discord.com/invite/qqJuwRb8Nh) to contact us.
{% endhint %}

