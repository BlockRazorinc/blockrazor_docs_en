---
description: >-
  Explore the services, application scenarios, benchmark data, price, endpoint,
  and integration methods of BlockRazor Ethereum NewBlocks stream.
metaLinks:
  canonical: newblocks.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/ethereum/newblocks
---

# Ethereum Block Stream

<details>

<summary><strong>What is Ethereum NewBlocks</strong></summary>

NewBlocks is a high-performance block stream service provided by BlockRazor, used for low-latency subscription to the latest blocks and confirmed transactions. NewBlocks helps users receive the latest block content earlier, and integrates the block header, transaction list, and next validator info into their monitoring or strategy systems with lower latency.

NewBlocks distributes the latest block data based on [BEF](../../../core-technology/blockchain-edge-fabric.md). When a block is generated in the network and begins to propagate, BlockRazor receives the block in multiple core areas as early as possible and then forwards it to subscribers through low-latency links, shortening the time for users to receive block data.

</details>

<details>

<summary><strong>Scenarios of Ethereum NewBlocks</strong></summary>

* Confirmed Transaction Monitoring: Receives confirmed transactions from the latest block in real time, used to monitor target addresses, popular contracts, or abnormal transaction behavior.
* Block-level data analysis: Obtaining block headers, transaction lists, and next validator info for block research, node observation, and network state analysis.
* Strategy data input: Serving as the confirmed data source for the trading system, it works in conjunction with capabilities such as Public Mempool and Transaction Submission to build a more complete monitoring and execution chain.

</details>

### Price

<table><thead><tr><th width="167.296875">Payment Method</th><th>Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td>$50 / stream / day<br>$500 / stream / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=ethereum&#x26;serviceId=ethereum_block_stream&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>$1250 / month <br>packaged with 9 other services.</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

{% hint style="info" %}
The number of data streams that can be subscribed to is calculated on a shared basis across all regions. For example, if you purchase one stream, you can only subscribe in one region; you will not be able to subscribe in other regions.
{% endhint %}

### Endpoint

<table><thead><tr><th width="160">Region</th><th>Relay Address</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75:50061</td></tr><tr><td>Tokyo</td><td>63.254.162.18:50061</td></tr><tr><td>Virginia</td><td>208.91.105.204:50061</td></tr></tbody></table>

### Request Example

[https://github.com/BlockRazorinc/eth\_relay\_example](https://github.com/BlockRazorinc/eth_relay_example)

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"context"
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
	stream, err := client.NewBlocks(ctx, &pb.NewBlocksRequest{ParsedTxs: true})
	if err != nil {
		fmt.Println("failed to subscribe new block: ", err)
		return
	}

	for {
		reply, err := stream.Recv()
		if err != nil {
			fmt.Println("stream receive error: ", err)
			return
		}

		header := reply.GetHeader()
		fmt.Println("receive new block, block hash is ", reply.Hash, " number is ", header.GetNumber(), " txs are ", len(reply.Transactions))
	}
}
```
{% endtab %}
{% endtabs %}

#### Proto

The code of `relay.proto` is as follows:

```go
syntax = "proto3";

package relay.v1;

option go_package = "github.com/BlockRazorinc/eth_relay_example/protobuf";

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
  "hash": "0xaf1f...4116",
  "header": {
    "parentHash": "0x4603...cfee",
    "sha3Uncles": "0x1dcc...9347",
    "miner": "0x0e33b1c214463062753ad849a28e54667e0c87c2",
    "stateRoot": "0x4c26...1c96",
    "transactionsRoot": "0xb6b8...731e",
    "receiptsRoot": "0xd9ae...75ea",
    "logsBloom": "0x263c...59fdb",
    "difficulty": "0x0",
    "number": "0x1845661",
    "gasLimit": "0x3938700",
    "gasUsed": "0x14c4d26",
    "timestamp": "0x6a475117",
    "extraData": "0x",
    "mixHash": "0xb048...3154",
    "nonce": "0x0000000000000000",
    "baseFeePerGas": "71769385",
    "withdrawalsRoot": "0xe5de...f2f2",
    "blobGasUsed": "0x20000",
    "excessBlobGas": "0xa8d8c53",
    "parentBeaconBlockRoot": "0x0319...7a00"
  },
  "transactions": [
    {
      "rawTx": "<base64 encoded raw transaction>",
      "from": "<base64 encoded sender>"
    }
  ],
  "withdrawals": [
    {
      "address": "0x1135fa96848f34bff9d003f4c1699ae97418de29",
      "amount": "0xda361b",
      "index": "0x80678fb",
      "validatorIndex": "0x1b1fc1"
    }
  ]
}
```

**Error**

```
rpc error: code = Unknown desc = data streams have exceeded its max limit [5]
```

