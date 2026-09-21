---
description: >-
  Introduce how to integrate BlockRazor Base RPC’s `eth_sendRawTransaction`
  method.
metaLinks:
  canonical: eth_sendrawtransaction.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/base/eth_sendrawtransaction
---

# Base eth\_sendRawTransaction

<details>

<summary><strong>What is Base Transaction Sending</strong></summary>

Base RPC is a transaction sending interface provided by BlockRazor for Base, used to send signed raw transactions. Currently, it only supports the `eth_sendRawTransaction` method and provides both gRPC and HTTPS.

</details>

<details>

<summary><strong>Why choose BlockRazor Base RPC?</strong></summary>

For Wallets and DEXs, directly connecting to the official Base Sequencer can meet basic sending requirements, but for businesses serving global users and focusing on cross-regional performance and production environment stability, the sending path itself still has room for further optimization. For a detailed comparison of BlockRazor Base RPC and the official sending service, please refer to the [Benchmark](https://blockrazor.io/blog/20250922basebenchmark/).

**Global multi-point deployment**

Compared to directly connecting to the Base Sequencer, BlockRazor Base RPC provides sending entry points in multiple core regions, making it suitable for wallets and DEXs to choose the nearest endpoint based on their service deployment location. For businesses serving global users, sending entry points closer to business servers and core user regions typically help shorten request paths and improve transaction sending efficiency.

**Intercontinental express lines**

BlockRazor Base RPC's optimization focuses not only on ingress node deployment but also on data transmission links between regional relays. Compared to inter-regional forwarding methods that rely entirely on ordinary public network routing, intercontinental express lines can significantly reduce the impact of routing fluctuations, congestion, and link jitter in inter-relay transmission, improving the stability and consistency of transaction requests throughout the overall transmission routing.

</details>

### Price

<table><thead><tr><th width="216.2578125">User Type</th><th width="140.359375">Limit</th><th>Price</th></tr></thead><tbody><tr><td>New registered users</td><td>1 Tx / 5s</td><td>Free</td></tr><tr><td>Paid users</td><td>5 Txs / 1s</td><td>$100 / day<br>$1000 /  month<br><br><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=base&#x26;serviceId=base_rpc_send_tx&#x26;billing=day" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Endpoint

{% tabs %}
{% tab title="gRPC" %}
<table><thead><tr><th width="167.0078125">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Frankfurt</td><td>frankfurt.grpc.base.blockrazor.xyz:80</td></tr><tr><td>Virginia</td><td>virginia.grpc.base.blockrazor.xyz:80</td></tr><tr><td>Tokyo</td><td>tokyo.grpc.base.blockrazor.xyz:80</td></tr></tbody></table>
{% endtab %}

{% tab title="HTTPS" %}
<table><thead><tr><th width="122.00390625">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Frankfurt</td><td>https://frankfurt-base.blockrazor.io:10101/eth_sendRawTransaction</td></tr><tr><td>Virginia</td><td>https://virginia-base.blockrazor.io:10101/eth_sendRawTransaction</td></tr><tr><td>Tokyo</td><td>https://tokyo-base.blockrazor.io:10101/eth_sendRawTransaction</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Request Parameter

<table><thead><tr><th width="106.7421875">Parameter</th><th width="111.046875">Mandatory</th><th width="121.6796875">Format</th><th>Example</th><th>Remark</th></tr></thead><tbody><tr><td>rawTransaction</td><td>mandatory</td><td>string[hex]</td><td>"0xd46e8dd67c5d32be8d24c6b0afe7c5c3f4e9c3b2dae18d0c6b0cf5c8f3e8b2c1"</td><td>signed raw transaction</td></tr></tbody></table>

### Request Example

{% tabs %}
{% tab title="gRPC" %}
Access the example [here](https://github.com/BlockRazorinc/base-api-client-go/blob/c4bec3d65e55ffb0da07253fa78aefe1b1c07e33/main.go#L150)

```go
// sendTransactions is an example function for sending a transaction.
// It's designed to reuse an existing gRPC client connection to avoid connection latency.
func sendTransactions(client basepb.BaseApiClient, authToken string, rawTxString string) {
	log.Println("[SendTx] Sending transaction...")
	ctx, cancel := context.WithTimeout(context.Background(), 3*time.Second)
	defer cancel()

	// Add the authentication token to the outgoing request's metadata.
	md := metadata.Pairs("authorization", authToken)
	ctx = metadata.NewOutgoingContext(ctx, md)

	req := &basepb.SendTransactionRequest{
		RawTransaction: rawTxString,
	}

	res, err := client.SendTransaction(ctx, req)
	if err != nil {
		log.Printf("[SendTx] Failed to send transaction: %v", err)
	} else {
		log.Printf("[SendTx] Transaction sent successfully. Hash: %s", res.GetTxHash())
	}
}
```
{% endtab %}

{% tab title="HTTPS" %}
```shellscript
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer [TOKEN]" \
  -d '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf8668206bf831e988e825208940d287d6c8e5f8ad086f90d1933de060fdc578c2d808082422ea082d586bdef05fd6af532ca2943740f092cc5275731f175c0e0538a3e6a9b3c0ba060d2454b3b7679cc6c31ec70d6a8d926dcc40cf8cd4442776e018bd54787befb"
    ],
    "id": 67
  }' \
https://virginia-base.blockrazor.io:10101/eth_sendRawTransaction
```
{% endtab %}
{% endtabs %}

#### [proto](https://github.com/BlockRazorinc/base-api-client-go/blob/main/proto/BaseApi.proto)

```go
syntax = "proto3";

option go_package = "./basepb";


import "google/protobuf/wrappers.proto";

message BaseBlock {
  string parent_hash = 1;
  string fee_recipient = 2;
  bytes state_root = 3;
  bytes receipts_root = 4;
  bytes logs_bloom = 5;
  bytes prev_randao = 6;
  uint64 block_number = 7;
  uint64 gas_limit = 8;
  uint64 gas_used = 9;
  uint64 timestamp = 10;
  bytes extra_data = 11;
  repeated uint64 base_fee_per_gas = 12;
  string block_hash = 13;
  repeated bytes transactions = 14;

  repeated Withdrawal withdrawals = 15;
  google.protobuf.UInt64Value blob_gas_used = 16;
  google.protobuf.UInt64Value excess_blob_gas = 17;
  google.protobuf.BytesValue withdrawals_root = 18;
}

message Withdrawal {
  uint64 index = 1;
  uint64 validator = 2;
  bytes address = 3;
  uint64 amount = 4;
}

message GetRawFlashBlocksStreamRequest {
}

message GetBlockStreamRequest {
}

message SendTransactionRequest {
  string rawTransaction = 1;
}

message SendTransactionResponse {
  string txHash = 1;
}

message FlashBlockStrRequest {
}

message RawFlashBlockStrResponse {
  bytes message = 1;
}

service BaseApi {
  rpc SendTransaction(SendTransactionRequest) returns (SendTransactionResponse);
  rpc GetBlockStream(GetBlockStreamRequest) returns (stream BaseBlock);
  rpc GetRawFlashBlockStream(GetRawFlashBlocksStreamRequest) returns (stream RawFlashBlockStrResponse);
}
```

### Response

**Normal**

```go
res: txHash:"0xaf430540d20eae2448947ffb254b03180b82333ef0c56bd526f7047489c195b5"
```

**Abnormal**

```go
res: txHash:"Unknown TxHash"
```



