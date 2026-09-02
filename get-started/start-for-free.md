---
description: >-
  BlockRazor offers new registered users multi-mode transaction sending modes on
  Solana, BSC, Robinhood Chain, Etherem, and Base for free.
metaLinks:
  canonical: start-for-free.md
  alternates:
    - https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/get-started/start-for-free
---

# Start for Free

### RPC

<table><thead><tr><th width="125.16015625">Chain</th><th>Methods</th><th>Limit</th></tr></thead><tbody><tr><td>BSC</td><td><ul><li><a href="../transaction-submission/rpc/bsc/eth_sendrawtransaction.md"><code>eth_sendRawTransaction</code></a></li><li><a href="../transaction-submission/rpc/bsc/eth_sendbundle.md"><code>eth_sendBundle</code></a></li><li>Other JSON RPC methods</li></ul></td><td>-</td></tr><tr><td>Ethereum</td><td><ul><li><a href="../transaction-submission/rpc/ethereum/eth_sendrawtransaction.md"><code>eth_sendRawTransaction</code></a></li><li><a href="../transaction-submission/rpc/ethereum/eth_sendbundle.md"><code>eth_sendBundle</code></a></li><li>Other JSON RPC methods</li></ul></td><td>-</td></tr></tbody></table>

### Transaction Sending

<table><thead><tr><th width="169.95703125">Chain</th><th width="306.46484375">Methods</th><th width="227.57421875">Limit</th></tr></thead><tbody><tr><td>Solana</td><td><ul><li><a href="../transaction-submission/transaction-sending/solana/send-transaction/"><code>Send Transaction</code></a> </li><li><a href="../transaction-submission/transaction-sending/solana/send-bundle/"><code>Send Bundle</code></a></li><li><a href="../transaction-submission/transaction-sending/solana/send-batch/"><code>Send Batch</code></a></li></ul></td><td>-</td></tr><tr><td>Robinhood Chain</td><td><ul><li><a href="../transaction-submission/transaction-sending/robinhood-chain/eth_sendrawtransaction/"><code>eth_sendRawTransaction</code></a></li></ul></td><td><ul><li>1 Tx / 5s</li></ul></td></tr><tr><td>BSC</td><td><ul><li><a href="../transaction-submission/transaction-sending/bsc/broadcast-tx.md"><code>Broadcast Tx</code></a></li></ul></td><td><ul><li>TPS：10 Txs / 5s</li><li>Daily Tx Limit：10</li></ul></td></tr><tr><td>Ethereum</td><td><ul><li><a href="../transaction-submission/transaction-sending/ethereum/broadcast-tx.md"><code>Broadcast Tx</code></a></li></ul></td><td><ul><li>TPS：10 Txs / 5s</li><li>Daily Tx Limit：10</li></ul></td></tr><tr><td>Base</td><td><ul><li><a href="../transaction-submission/transaction-sending/base/eth_sendrawtransaction.md"><code>eth_sendRawTransaction</code></a></li></ul></td><td><ul><li>1 Tx / 5s</li></ul></td></tr><tr><td>Base</td><td><ul><li><a href="../transaction-submission/transaction-sending/base/eth_sendrawtransaction-tip.md"><code>eth_sendRawTransaction(tip)</code></a></li></ul></td><td><ul><li>Default to 10 TPS</li></ul></td></tr></tbody></table>

### Block Builder

<table><thead><tr><th width="125.484375">Chain</th><th width="288.10546875">Methods</th><th>Limit</th></tr></thead><tbody><tr><td>BSC</td><td><ul><li><a href="../transaction-submission/block-builder/send-bundle.md"><code>eth_sendBundle</code></a></li><li><a href="../transaction-submission/block-builder/send-privatetransaction.md"><code>eth_sendPrivateTransaction</code></a></li></ul></td><td>-</td></tr></tbody></table>
