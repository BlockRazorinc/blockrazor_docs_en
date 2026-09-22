---
description: >-
  Follow the setup steps, verify requests in your project, get transactions
  protected and earn backrun refund.
metaLinks:
  canonical: integration.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/bsc/integration
---

# Integrate BSC RPC into Project

{% hint style="info" %}
BlockRazor RPC is free to all users. No auth needed.
{% endhint %}

### Endpoint

<table><thead><tr><th width="203.8515625">Endpoint type</th><th width="483.47265625">URL</th></tr></thead><tbody><tr><td>General RPC</td><td>https://bsc.blockrazor.xyz</td></tr><tr><td>Project Default RPC</td><td>https://bsc.blockrazor.xyz/&#x3C;rpc_id></td></tr><tr><td>Project Custom RPC</td><td>https://&#x3C;custom_domain>.bsc.blockrazor.xyz</td></tr></tbody></table>

### How to integrate RPC into your project

#### 1. Configure RPC

1. Register and log in to the portal at [blockrazor.io](https://blockrazor.io/).
2. Under the RPC module, click on the RPC page to view the configuration information of your RPC.
3. Click on **Update** to enter the configuration update page, and adjust the parameters according to your needs. The meaning of the parameters is shown in the following table.

<table><thead><tr><th width="181">Parameters</th><th>Meaning</th></tr></thead><tbody><tr><td>Default RPC URL</td><td>Each account automatically generates 1 Ethereum RPC and 1 BSC RPC by default. The Default RPC URL is automatically generated and cannot be modified.</td></tr><tr><td>Custom RPC URL</td><td>The third-level domain is allowed to be modified, and the Custom RPC URL can be promoted to the project's end-users through websites or docs, guiding them to add custom RPC within their wallets.</td></tr><tr><td>Hint</td><td>The system defaults to sharing the transaction's hash and logs with the Searcher. The more fields shared, the greater the possibility of obtaining refunds, please proceed with caution after evaluating the need for disclosure of transaction data.</td></tr><tr><td>Refund Address</td><td>The default refund address is tx.origin, which means the refund will be returned to the sender of the transaction. It can be modified to a fixed refund address (EOA).</td></tr><tr><td>Revert Protection</td><td>revert protection is enabled by default; if a transaction is detected to revert, it will not be included in the block. To ensure fast inclusion in a block, it is recommended to set priority fee (Ethereum) when sending transactions.</td></tr></tbody></table>

4. Click **Confirm**, the system will update the RPC configuration in real time.

#### 2. Integrate RPC

1. Find the configuration file or code: Open the project workspace and locate the file or code segment that configures the RPC node in the DApp project. This could be a configuration file such as .env, config.js, truffle-config.js, etc., or it could be hardcoded directly in the code.
2. Modify the RPC URL: Change the RPC URL in the configuration file or code to the Scutum RPC URL.
3. Test the connection: After making the change, run the DApp or the corresponding test script locally to ensure that the new RPC URL works properly. You can use methods like `web3.eth.net.isListening()` or `ethers.provider.pollingInterval` to check if the connection is successful.
4. Deploy the update: If the test passes, you can deploy the changes to the production environment.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// import Web3
const Web3 = require('web3');

// Create a Web3 instance and connect to the RPC.
const web3 = new Web3('https://ethereum-rpc.publicnode.com'); // You can replace the RPC URL with the Scutum RPC URL here.

// check the connection
web3.eth.net.isListening()
  .then((listening) => {
    console.log('Web3 connected: ', listening);
  })
  .catch((err) => {
    console.error('Web3 connection error: ', err);
  });
```
{% endtab %}
{% endtabs %}

#### 3. Query Transactions

1. Log in to [blockrazor.io](https://blockrazor.io).
2. Under the Scutum module, click on **Refunds** to view the refund, and click on **Transactions** to view the transactions submitted to dedicated RPC.

