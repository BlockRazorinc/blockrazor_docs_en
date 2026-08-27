---
description: >-
  This section introduces the services, application scenarios, and access
  methods of the BlockRazor Ethereum Public Mempool.
metaLinks:
  canonical: public-mempool.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/public-mempool/ethereum/public-mempool
---

# Ethereum Public Mempool

### What is Ethereum Public Mempool

Public Mempool is a high-performance pending transaction stream based on [BEF](../../../core-technology/blockchain-edge-fabric.md), used for low-latency subscription to unconfirmed transactions in public propagation.

In the EVM network, transactions typically propagate through the mempool before entering a block. The core value of the Public Mempool is to help users obtain publicly available pending transaction signals earlier and integrate these signals into their strategy systems with lower latency. It is commonly used to monitor public trading activity, track Smart Money behavior, identify new opportunities, and provide faster signal input for strategies such as backrun, copy trading, and sniping. For systems that rely on pending signals to drive trading decisions, seeing transactions earlier often means:

* Entering the strategy judgment process earlier
* More time to complete calculations and risk control
* Higher probability of obtaining a better execution position in competitive scenarios

### Scenarios of Ethereum Public Mempool

* Pending Transaction Monitoring: Real-time monitoring of publicly distributed pending transactions to identify active addresses, popular contracts, or unusual transaction behavior.
* Smart Money Tracking: Track transaction activity at target addresses early on, providing signals for copy trading or strategy following.
* Backrun Discovery: Identifying publicly trades that may trigger backrun opportunities, allowing more time for subsequent strategy evaluation and trade submission.
* Sniping Opportunities: Capture the first signals in the open market as early as possible when new pools launch, liquidity injections occur, or target trades emerge.
* Real-time data input for strategies: Serving as a real-time input source for the trading system, it can be used in conjunction with capabilities such as Block Stream, Node Stream, RPC, or Block Builder to build a more complete monitoring and execution project.

### Price

<table><thead><tr><th width="167.296875">Payment Method</th><th>Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td>$30 / stream / day<br>$300 / stream / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=ethereum&#x26;serviceId=ethereum_public_mempool&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>$1250 / month<br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

{% hint style="info" %}
The number of data streams that can be subscribed to is calculated on a shared basis across all regions. For example, if you purchase one stream, you can only subscribe in one region; you will not be able to subscribe in other regions.
{% endhint %}

### Endpoint

<table><thead><tr><th width="160">Region</th><th>Relay Address</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75:50061</td></tr><tr><td>Tokyo</td><td>63.254.162.18:50061</td></tr><tr><td>Virginia</td><td>208.91.105.204:50061</td></tr></tbody></table>

### Request Example

[https://github.com/BlockRazorinc/eth\_relay\_example](https://github.com/BlockRazorinc/eth_relay_examplehttps:/github.com/BlockRazorinc/eth_relay_example)

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"context"
	"encoding/hex"
	"fmt"

	// directory of the generated code using the provided relay.proto file
	pb "github.com/BlockRazorinc/eth_relay_example/protobuf"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
)

// auth will be used to verify the credential
type Authentication struct {
	apiKey string
}

func (a *Authentication) GetRequestMetadata(context.Context, ...string) (map[string]string, error) {
	return map[string]string{"apikey": a.apiKey}, nil
}

func (a *Authentication) RequireTransportSecurity() bool {
	return false
}

func main() {

	// BlockRazor relay endpoint address
	blzrelayEndPoint := "ip:port"

	// auth will be used to verify the credential
	auth := Authentication{
		"your auth token",
	}

	// open gRPC connection to BlockRazor relay
	var err error
	conn, err := grpc.NewClient(blzrelayEndPoint, grpc.WithTransportCredentials(insecure.NewCredentials()), grpc.WithPerRPCCredentials(&auth), grpc.WithWriteBufferSize(0), grpc.WithInitialConnWindowSize(128*1024))
	if err != nil {
		fmt.Println("error: ", err)
		return
	}
	defer conn.Close()

	// use the Relay client connection interface
	client := pb.NewRelayClient(conn)

	// create context and defer cancel of context
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	// create a subscription using the stream-specific method and request
	stream, err := client.NewTxs(ctx, &pb.NewTxsRequest{IncludeRawTx: true})
	if err != nil {
		fmt.Println("failed to subscribe new tx: ", err)
		return
	}

	for {
		reply, err := stream.Recv()
		if err != nil {
			fmt.Println("stream receive error: ", err)
			return
		}

		fmt.Println("receive new tx, tx hash is ", reply.TxHash, " source is ", reply.Source)
		if len(reply.RawTx) > 0 {
			fmt.Println("raw tx is 0x" + hex.EncodeToString(reply.RawTx))
		}
	}
}
```
{% endtab %}
{% endtabs %}

```go

service Relay {
  rpc SendTx(SendTxRequest) returns (SendTxReply);
  rpc NewTxs(NewTxsRequest) returns (stream NewTx);
  rpc NewBlocks(NewBlocksRequest) returns (stream NewBlock);
}

message SendTxRequest {
  bytes raw_tx = 1;
}

message SendTxReply {
  string tx_hash = 1;
}

message NewTxsRequest {
  bool include_raw_tx = 1;
}

message NewTx {
  string tx_hash = 1;
  bytes raw_tx = 2;
  int64 first_seen_unix_ns = 3;
  string source = 4;
}

message NewBlocksRequest {
  bool parsed_txs = 1;
}

message NewBlock {
  string hash = 1;
  BlockHeader header = 2;
  repeated BlockTransaction transactions = 3;
  repeated Withdrawal withdrawals = 4;
}

message BlockHeader {
  string parent_hash = 1;
  string sha3_uncles = 2;
  string miner = 3;
  string state_root = 4;
  string transactions_root = 5;
  string receipts_root = 6;
  string logs_bloom = 7;
  string difficulty = 8;
  string number = 9;
  string gas_limit = 10;
  string gas_used = 11;
  string timestamp = 12;
  string extra_data = 13;
  string mix_hash = 14;
  string nonce = 15;
  string base_fee_per_gas = 16;
  string withdrawals_root = 17;
  string blob_gas_used = 18;
  string excess_blob_gas = 19;
  string parent_beacon_block_root = 20;
}

message BlockTransaction {
  bytes raw_tx = 1;
  bytes from = 2;
}

message Withdrawal {
  string address = 1;
  string amount = 2;
  string index = 3;
  string validator_index = 4;
}
```

### Response Example

**Success**

```json
{
  "tx":[
     {
        "raw_tx":"+QH0gjOthDuaygCDBrbAlKoP7P6dEOH8IzwtDAw9whDVeHKRhwFrzEHpAAC5AYTVQ9H9AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAmsrt4HBPm7jWohanh7XmbX9S/gRLrth87fXF3H2gC0FAAAAAAAAAAAAAAAAbsa1rd5IJ6lr43ixr1+LWmT/OhgAAAAAAAAAAAAAAABV05gyb5kFn/d1SFJGmZAnsxl5VQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABVkLNFR0SQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGZ7qkBM09jlPtkQprbOV2bITVAfdbvzTltwBYjUJu6OIzF3aAAAAAAAAAAAAAAAAHqXLqcmW4qO1ZEAZXn2nYI/dKV1AAAAAAAAAAAAAAAAVdOYMm+ZBZ/3dUhSRpmQJ7MZeVUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAASvCnY7scAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABme6pAgZOgy2LsKlIqPeeM7d520T3eAwIVk9O+vY4wT+zifYp0GGOgTY7Z5J3zs/YCj1HvVXOZF9Q2rj5x421GBG9CrKmxVGo="
     }
   ]
}
```

**Error**

```
rpc error: code = Unknown desc = data streams have exceeded its max limit [5]
```

