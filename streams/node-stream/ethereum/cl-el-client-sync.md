---
description: >-
  Explore the services, advantages, target users, price, endpoints and
  integration guide of the BlockRazor Ethereum CL/EL client synchronization.
metaLinks:
  canonical: cl-el-client-sync.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/node-stream/ethereum/cl-el-client-sync
---

# Ethereum CL/EL Client Sync

<details>

<summary><strong>What is CL/EL client sync?</strong></summary>

CL/EL client sync is a low-latency node synchronization capability designed by BlockRazor for Ethereum node architecture in the Node Stream scenario, used to help users synchronize their own Ethereum nodes with the latest world state faster.

Unlike BSC full node synchronization, which primarily targets a single EVM execution node, Ethereum adopted a CL/EL dual-client architecture after The Merge. Therefore, Ethereum's Node Stream not only enables EL to execute new blocks faster, but also allows CL to keep up with the correct head, safe/finalized blocks, and consensus layer messages more quickly.

</details>

<details>

<summary><strong>Why choose CL/EL client sync?</strong></summary>

For many high-frequency trading systems, searchers, quantitative strategies, and infrastructure teams, a core issue is whether the local Ethereum node can synchronize the latest head, execute the latest block, and update the world state quickly enough. Even if the strategy system, matching logic, or transaction construction itself is fast, if the chain head or state seen by the local node is lagging behind, subsequent strategy judgments, simulations, risk control, transaction replacements, and execution decisions will still be affected.

The value of CL/EL client synchronization is not just "helping nodes connect to the network," but also providing a lower latency and more stable synchronization entry point for production systems that rely on the state of local Ethereum nodes, reducing the impact of synchronization lag of local nodes in strategy judgment, trading simulation, and MEV scenarios on trading and analysis systems.

</details>

<details>

<summary><strong>Which users are suitable for CL/EL client sync</strong></summary>

* **Quant Team / Trading Bot / Searcher**\
  A quantitative and trading system that relies on the state of local nodes to make strategy judgments, prepare for transactions, or perform on-chain analysis.
* **Infra / Node Teams**\
  The engineering team is responsible for node deployment and status synchronization

If your goal is simply to obtain confirmed block data with low latency, Block Stream is usually sufficient.\
If your system needs its local nodes to synchronize with the latest blocks and world state as quickly as possible, then CL/EL client sync would be more appropriate.

</details>

### Price

<table><thead><tr><th width="177.73046875">Payment Method</th><th>Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td>$80 / client / day<br>$800 / client / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=ethereum&#x26;serviceId=ethereum_enode&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>$1250 / month<br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Relay IP

<table><thead><tr><th width="190">地區</th><th>Relay IP</th></tr></thead><tbody><tr><td>法蘭克福</td><td>64.130.47.75</td></tr><tr><td>東京</td><td>63.254.162.18</td></tr><tr><td>弗吉尼亞</td><td>208.91.105.204</td></tr></tbody></table>

### EL Client User Guide

#### Step 1: Purchase Node Stream

1. Go to [https://www.blockrazor.io/](https://www.blockrazor.io/https://www.blockrazor.io/), click on "Sign up" in the upper right corner, and complete the registration.
2. Log in to the console, go to \[Pricing] and complete the purchase.
3. Go to \[Services] - \[Streams] - \[Node Stream], and click \[Edit].
4. Select the region closest to the EL client, enter the EL client Enode that needs to be connected to the relay, and click \[Confirm] to complete the addition.
5. Return to the Node list and click "Copy Relay Addr".

#### Step 2:  Open ports to allow relay access

{% hint style="info" %}
If your EL client is deployed on AWS or other cloud services, you should additionally configure inbound rules for the security group.
{% endhint %}

1. Access your own EL client's server and execute commands to set up the firewall to allow Relay access.

```bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="63.254.162.18" port port="30303" protocol="tcp" accept'
```

* "source address" is the IP of Relay which can be acquired from [Relay IP](cl-el-client-sync.md#relay-ip)
* The port for the EL client to allow Relay access is generally set as the default, which is 30303. You can modify this based on your own node configuration

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

```bash
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

### CL Client User Guide

#### Step 1: Purchase Node Stream

1. Go to [https://www.blockrazor.io/](https://www.blockrazor.io/https://www.blockrazor.io/), click on "Sign up" in the upper right corner, and complete the registration.
2. Log in to the console, go to \[Pricing] and complete the purchase.
3. Go to \[Services] - \[Streams] - \[Node Stream], and click \[Edit].
4. Select the region closest to the CL client, enter your CL client ENR and click \[Confirm] to complete the addition.
5. Return to the Node list and click "Copy Relay Addr".

#### Step 2:  Open ports to allow relay access

{% hint style="info" %}
If your CL client is deployed on AWS or other cloud services, you should additionally configure inbound rules for the security group.
{% endhint %}

1. Access your own CL client's server and execute commands to set up the firewall to allow Relay access.

```bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="63.254.162.18" port port="9000" protocol="tcp" accept'

sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="63.254.162.18" port port="9000" protocol="udp" accept'
```

* "source address" is the IP of Relay which can be acquired from [Relay IP](cl-el-client-sync.md#relay-ip)
* "port" is the port that the CL client allows the relay to access. Users can modify it according to the default value based on the CL client type.

2. Reload the firewall configuration to make the changes take effect.

```bash
sudo firewall-cmd --reload
```

#### Step 3: Set Relay to TrustedNode

Obtain Relay Multiaddr `/ip4/<Relay_IP>/tcp/<Relay_P2P_PORT>/p2p/${Relay_Peer_ID}`  in step 1, configure the startup parameters and restart the CL client.

{% tabs %}
{% tab title="Lighthouse" %}
{% code overflow="wrap" %}
```bash
--boot-nodes <'Relay_Multiaddr','Bootnodes_ENR'> --trusted-peers "$Relay_Peer_ID"

## Since the boot-nodes parameter is overwritten and updated after a reboot, to ensure connection stability, please include the default bootnodes ENR parameter after Relay_Multiaddr. See reference https://github.com/sigp/lighthouse/blob/120c3c6dac9df8ee4d83f055919bd3488abae4f6/common/eth2_network_config/built_in_network_configs/mainnet/bootstrap_nodes.yaml#L16
```
{% endcode %}
{% endtab %}

{% tab title="Prysm" %}
```bash
--peer "$Relay_Multiaddr"
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
--netkey-file /path/to/netkey --direct-peer "$Relay_Multiaddr"
```
{% endtab %}

{% tab title="Teku" %}
```bash
--p2p-direct-peers "$Relay_Multiaddr"
```
{% endtab %}
{% endtabs %}

#### Step 4: Query connection status

1. Wait 10 minutes, access the node and execute the command, and check if the Relay's Peer ID appears in the peers list where `state=connected`.

```bash
Relay_Peer_ID="16Uiu2..."
REST_PORT="<CLIENT_REST_PORT>"

curl -s "http://127.0.0.1:${REST_PORT}/eth/v1/node/peers?state=connected" \
  | jq --arg id "$Relay_Peer_ID" '.data[] | select(.peer_id == $id) | {
      peer_id,
      state,
      direction,
      last_seen_p2p_address,
      enr
    }'
```

2. If there is output and you see `"state": "connected"`, it means that you have connected to the target node.
