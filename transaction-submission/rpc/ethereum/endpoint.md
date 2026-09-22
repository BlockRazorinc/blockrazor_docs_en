---
description: >-
  Find general endpoints, dedicated endpoints, and regional endpoints of
  BlockRazor Ethereum RPC for low-latency integration.
metaLinks:
  canonical: endpoint.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/ethereum-rpc/endpoint
---

# Ethereum RPC Endpoint

### General Endpoints

<table data-search="false"><thead><tr><th width="127.3359375"></th><th width="186">default</th><th width="196">fullprivacy</th><th>maxbackrun</th></tr></thead><tbody><tr><td>URL</td><td>https://eth.blockrazor.xyz</td><td>https://eth.blockrazor.xyz/fullprivacy</td><td>https://eth.blockrazor.xyz/maxbackrun</td></tr><tr><td>MEV Protection</td><td>Enabled</td><td>Enabled</td><td>Enabled</td></tr><tr><td>Transaction Privacy</td><td>Minimal disclosure</td><td>Full privacy</td><td>Maximum disclosure</td></tr><tr><td>Refund Potential</td><td>Medium</td><td>No refund</td><td>High</td></tr><tr><td>Refund Percentage</td><td>Supported</td><td>No refund</td><td>Supported</td></tr><tr><td>Revert Protection</td><td>Disabled</td><td>Enabled</td><td>Enabled</td></tr></tbody></table>

<details>

<summary><strong>default Mode</strong></summary>

In `default` mode, transactions submitted through Ethereum RPC disclose only the necessary transaction data to Searchers (`hash`, `logs`, and state changes). This provides opportunities for refunds while protecting transaction privacy as much as possible.

To prioritize fast block inclusion, revert protection is not enabled in this mode. Users are advised to set an appropriate transaction Priority Fee to improve inclusion speed.

</details>

<details>

<summary><strong>fullprivacy Mode</strong></summary>

In `fullprivacy` mode, transactions submitted through Ethereum RPC do not disclose any transaction data. Ethereum RPC forwards the transactions directly to mainstream Builders.

Because no transaction data is disclosed, transactions in this mode do not generate refunds, and no refund percentage needs to be configured. Revert protection is enabled in this mode.

</details>

<details>

<summary><strong>maxbackrun Mode</strong></summary>

In `maxbackrun` mode, transactions submitted through Ethereum RPC disclose the transaction data required to maximize refund opportunities while maintaining privacy protection. The disclosed fields include `hash`, `to`, `calldata`, `functionSelector`, `logs`, and state changes.

Revert protection is enabled in this mode. Users are advised to set an appropriate transaction Priority Fee to improve inclusion speed.

</details>

### Dedicated Endpoints

A dedicated endpoint is a private transaction channel provided by BlockRazor for an individual user or project. Users can configure dedicated endpoints in the BlockRazor console, including customizing the endpoint URL for easier identification and configuring transaction disclosure parameters and refund recipient addresses.

After adding a dedicated endpoint to a wallet or integrating it into a project, users can view its transactions and refund records in the BlockRazor console.

<table><thead><tr><th width="148.8359375">Endpoint</th><th width="551.25390625">Example URL</th></tr></thead><tbody><tr><td>Dedicated Endpoint URL</td><td>https://eth.blockrazor.xyz/&#x3C;rpc_id></td></tr><tr><td>Custom Endpoint URL</td><td>https://&#x3C;custom_content>.eth.blockrazor.xyz</td></tr></tbody></table>

### Regional Endpoints

<table><thead><tr><th width="148.99609375">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Tokyo</td><td>https://jp-ethscutum.blockrazor.xyz</td></tr><tr><td>New York</td><td>https://us-ethscutum.blockrazor.xyz</td></tr></tbody></table>

Regional endpoints are suitable for projects that are highly sensitive to transaction latency and whose transaction sources are concentrated in specific regions.
