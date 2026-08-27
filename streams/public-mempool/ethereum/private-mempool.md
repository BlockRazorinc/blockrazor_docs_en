---
hidden: true
noIndex: true
---

# Private Mempool

### Introduction

The Ethereum Private Mempool delivers private transaction stream data, which is pushed based on the SSE protocol. Transactions within the data stream are uniformly desensitized, disclosing only transaction fields authorized for release.

Private Mempool can be applied to a variety of scenarios, including backrunning, copy trading, and sniping.

To avoid data interruption due to network fluctuations, it is recommended to establish a reconnection mechanism.

### Endpoint

{% hint style="info" %}
Please keep the domain for subscribing to bundles consistent with the domain for sending bundles. For example, if you subscribe to `https://jp-ethscutum.blockrazor.xyz/stream`, send bundles to `https://jp-ethscutum.blockrazor.xyz`.
{% endhint %}

{% hint style="info" %}
Private data streams vary across regions. It is recommended to subscribe to both endpoints simultaneously.
{% endhint %}

<table><thead><tr><th width="129.359375">地區</th><th>端點</th></tr></thead><tbody><tr><td>Tokyo</td><td>https://eth-bscscutum.blockrazor.xyz/stream</td></tr><tr><td>New York</td><td>https://eth-bscscutum.blockrazor.xyz/stream</td></tr></tbody></table>

### Authentication

{% hint style="info" %}
To protect private transactions,  Ethereum Private Mempool is restricted to designated Searchers. Please [contact](https://discord.com/invite/qqJuwRb8Nh) us if you need to subscribe to the Private Mempool
{% endhint %}

```json
curl -X GET \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token>" \
    --data '{}' \
    https://eth.blockrazor.xyz/stream
```

```
https://eth.blockrazor.xyz/stream?token=<token>
```

The \<token> in the example must be obtained after registering for BlockRazor. The steps are as follows:

1. Go to [https://www.blockrazor.io](https://www.blockrazor.io/) and click on \[Register] in the upper right corner of the webpage, the system will redirect you to the registration page.
2. On the registration page, enter your email and password, then click \[Register], the system will send an account activation email to your mailbox.
3. Go to your mailbox, check the account activation email, and click on the account activation link.
4. After completing the account activation, proceed to log in, check your account information, and copy the auth token.

### Data Stream Structure

```json
{
    "result": [
        {
            "chainId": "0x1",
            "to": "0x6215589d293fdf52886484f46f0d6a11c76b4a7e",
            "value": "0x4fefa17b724000",
            "data": "0x",
            "nonce": "0x10",
            "type": "0x2",
            "hash": "0x5f08dd372fce1a44dda27bed60ca036acb4979fad6ca37b9c388e351a870fe4c",
            "from": "0xcb1588f3f7e92a1278c68a6aed4bdcbc68534b29"
        },
        {
            "chainId": "0x1",
            "to": "0x8765589d293fdf52886484f46f0d6a11c76b4a7e",
            "value": "0x0",
            "data": "0x07ed2379000000000000000000000000990636ecb3ff04d33d92e970d3d588bf5cd8d086000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec70000000000000000000000005c697fee285b513711a816018dbb34dc0cfc4875000000000000000000000000990636ecb3ff04d33d92e970d3d588bf5cd8d0860000000000000000000000007718bc9e46745e94602c43a4a4903488647c4839000000000000000000000000000000000000000000000000000000002faf08000000000000000000000000000000000000000000000001771036abd0ec4309a00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000012000000000000000000000000000000000000000000000000000000000000003e90000000000000000000003cb00039d00033a00032000030600030000004e00a0744c8c09dac17f958d2ee523a2206206994597c13d831ec790cbe4bdd538d6e9b379bff5fe72c3d67a521de50000000000000000000000000000000000000000000000000000000000249f0000a0c9e75c4800000000000000000000000000000000bb803e8000000000000000000000000000000000000000000000000000027a00022300a007e5c0d20000000000000000000000000000000000000000000000000001ff0001b05120bbcb91440523216e2b87052a99f69c604a7b6e00dac17f958d2ee523a2206206994597c13d831ec700847fc9d4ad000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000bc35d29000000000000000000000000990636ecb3ff04d33d92e970d3d588bf5cd8d0860000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000014000000000000000000000000000000000000000000000000000000000000001600000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002a000000000000000000000000000000000000000000000000000de6e2040c77f52ee63c1e501e0554a476a092703abdb3ef35c80e0d76d32939fa0b86991c6218b36c1d19d4a2e9eb0ce3606eb4802a0000000000000000000000000000000000000000000000000029b43aa6973f98048c95033000000000000000000000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7000064000001000000206b4be0b94041c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2d0e30db00020d6bdbf78c02aaa39b223fe8d0a0e5c4f27ead9083c756cc202a00000000000000000000000000000000000000000000001771036abd0ec4309a0ee63c1e58061192eb6ca9fe34a3ccc5f4cd4bf6fefb77a037fc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2111111125421ca6dc452d289314280a0f8842a650020d6bdbf785c697fee285b513711a816018dbb34dc0cfc4875111111125421ca6dc452d289314280a0f8842a650000000000000000000000000000000000000000000000b59f19d6",
            "nonce": "0x11",
            "type": "0x2",
            "hash": "0x3456dd372fce1a44dda27bed60ca036acb4979fad6ca37b9c388e351a870fe4c",
            "from": "0xcb1588f3f7e92a1278c68a6aed4bdcbc68534b29"
        }
    ]
}
```



