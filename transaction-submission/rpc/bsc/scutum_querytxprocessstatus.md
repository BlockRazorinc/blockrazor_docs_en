---
description: >-
  Use the BlockRazor BSC RPC Transaction Trace API. Review the endpoint,
  parameters, request format, response fields, and integration example.
metaLinks:
  canonical: scutum_querytxprocessstatus.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/rpc/bsc/scutum_querytxprocessstatus
---

# BSC RPC Transaction Trace API

`scutum_queryTxProcessStatus` is used to query the real-time process status of transactions sent to BlockRazor RPC. It is currently available on BSC.

### Request Example

```json
curl -X POST -H "Content-Type: application/json" --data'{
	"id": 1,
	"jsonrpc": "2.0",
	"method": "scutum_queryTxProcessStatus",
	"params": ["0xf84a……e54284"]
}'<ETH_NODE_URL>
```

### Response Example

**Normal**

```json
{
  "jsonrpc": "2.0",
  "result": "{"msg\":\"tx is included on chain\",\"status\":\"included\"}",
  "id": "1"
} // the tx is executed on chain, success or reverted
```

```json
{
  "jsonrpc": "2.0",
  "result": "{"msg\":\"tx is expired and discarded\",\"status\":\"expired\"}",
  "id": "1"
} // 100 blocks has passed since submission of the tx
```

```json
{
  "jsonrpc": "2.0",
  "result": "{"msg\":\"nonce too high\",\"status\":\"pending\"}",
  "id": "1"
} // the tx is queued since the nonce is too high
```

```json
{
  "jsonrpc": "2.0",
  "result": "{"msg\":\"tx is pending\",\"status\":\"pending\"}",
  "id": "1"
} // the tx is being processed by Scutum
```

```json
{
  "jsonrpc": "2.0",
  "result": "{"msg\":\"simulation error: xxxxxxx\",\"status\":\"failed\"}",
  "id": "1"
} // the tx failed to be processed by Scutum due to the simulation error
```

**Abnomal**

```json
{
  "jsonrpc": "2.0",
  "error": "{\"code\":-32000,\"message\":\"tx not found\"}",
  "id": "1"
}  //the tx has not been sent to Scutum or has exceeded the Scutum processing time limit
```
