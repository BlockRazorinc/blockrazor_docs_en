---
description: >-
  Explore the services, application scenarios, benchmark data, price, endpoint,
  and integration methods of BlockRazor BSC NewBlocks stream.
metaLinks:
  canonical: newblocks.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/bsc/newblocks
---

# BSC NewBlocks Stream Integration Guide

<details>

<summary><strong>What is BSC NewBlocks</strong></summary>

NewBlocks is a high-performance block stream service provided by BlockRazor, used for low-latency subscription to the latest blocks and confirmed transactions. NewBlocks helps users receive the latest block content earlier, and integrates the block header, transaction list, and next validator info into their monitoring or strategy systems with lower latency.

NewBlocks distributes the latest block data based on [BEF](../../../core-technology/blockchain-edge-fabric.md). When a block is generated in the network and begins to propagate, BlockRazor receives the block in multiple core areas as early as possible and then forwards it to subscribers through low-latency links, shortening the time for users to receive block data.

</details>

<details>

<summary><strong>Scenarios of BSC NewBlocks</strong></summary>

* Confirmed Transaction Monitoring: Receives confirmed transactions from the latest block in real time, used to monitor target addresses, popular contracts, or abnormal transaction behavior.
* Block-level data analysis: Obtaining block headers, transaction lists, and next validator info for block research, node observation, and network state analysis.
* Strategy data input: Serving as the confirmed data source for the trading system, it works in conjunction with capabilities such as Public Mempool and Transaction Submission to build a more complete monitoring and execution chain.

</details>

### Benchmark

In our benchmark on the latency of receiving new blocks, we compared BlockRazor with a regular Node in four regions: Dublin, Frankfurt, Tokyo, and Virginia. The evaluation was based on the time difference for clients receiving the same block from BlockRazor and node in the comparable sample.

<table><thead><tr><th width="131.58203125">Region</th><th width="190.0703125">BlockRazor Lead Rate</th><th>Avg Lead</th><th>P50 Lead</th><th>P90 Lead</th></tr></thead><tbody><tr><td>Dublin</td><td><strong>100.0%</strong></td><td>60.4 ms</td><td>50.5 ms</td><td>90.7 ms</td></tr><tr><td>Frankfurt</td><td><strong>100.0%</strong></td><td>61.5 ms</td><td>54.7 ms</td><td>92.3 ms</td></tr><tr><td>Tokyo</td><td><strong>100.0%</strong></td><td>239.1 ms</td><td>145.7 ms</td><td>622.9 ms</td></tr><tr><td>Virginia</td><td><strong>99.5%</strong></td><td>69.0 ms</td><td>58.3 ms</td><td>121.6 ms</td></tr></tbody></table>

The results show that BlockRazor maintained a consistent lead across all four regions. In the Frankfurt, Tokyo, and  Dublin regions, BlockRazor achieved a 100% lead rate; in the Virginia region, the lead rate reached 99.5%. In terms of lead magnitude, BlockRazor's average lead time across different regions was approximately 60.4 ms, 61.5 ms, 239.1 ms, and 69.0ms; under the P90 dimension, the lead magnitudes were 90.7 ms, 92.3 ms, 622.9ms, and 121.6ms, respectively.

This indicates that BlockRazor is able to receive block data more consistently ahead of ordinary nodes when new blocks arrive.

### Price

<table><thead><tr><th width="167.296875">Payment Method</th><th>Price</th><th>Action</th></tr></thead><tbody><tr><td>Personalized</td><td>$50 / stream / day<br>$500 / stream / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_block_stream&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td>$1250 / month <br>bundled with other essential services for more value</td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

{% hint style="info" %}
The number of data streams that can be subscribed to is calculated on a shared basis across all regions. For example, if you purchase one stream, you can only subscribe in one region; you will not be able to subscribe in other regions.
{% endhint %}

### Endpoint

<table><thead><tr><th width="160">Region</th><th>Relay Address</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75:50051</td></tr><tr><td>Tokyo</td><td>63.254.162.18:50051</td></tr><tr><td>Dublin</td><td>141.98.217.82:50051</td></tr><tr><td>Virginia</td><td>208.91.105.204:50051</td></tr></tbody></table>

### Request Parameters

<table><thead><tr><th width="164">Parameters</th><th width="117">Mandatory</th><th width="102">Format</th><th width="94">Example</th><th>Description</th></tr></thead><tbody><tr><td>NodeValidation</td><td>Mandatory</td><td>boolean</td><td>false</td><td>This field currently only supports being set to <code>false</code>, and the relay will push all new blocks (unchecked) with lower latency.</td></tr></tbody></table>

### Request Example

[https://github.com/BlockRazorinc/relay\_example](https://github.com/BlockRazorinc/relay_example)

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"context"
	"fmt"

	// directory of the generated code using the provided relay.proto file
	pb "github.com/BlockRazorinc/relay_example/protobuf"
	"google.golang.org/grpc"
)

// auth will be used to verify the credential
type Authentication struct {
	apiKey string
}

func (a *Authentication) GetRequestMetadata(context.Context, ...string) (map[string]string, error) {
	return map[string]string{"apiKey": a.apiKey}, nil
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
	conn, err := grpc.Dial(blzrelayEndPoint, grpc.WithInsecure(), grpc.WithPerRPCCredentials(&auth), grpc.WithWriteBufferSize(0), grpc.WithInitialConnWindowSize(128*1024))
	if err != nil {
		fmt.Println("error: ", err)
		return
	}

	// use the Gateway client connection interface
	client := pb.NewGatewayClient(conn)

	// create context and defer cancel of context
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	// create a subscription using the stream-specific method and request
	stream, err := client.NewBlocks(ctx, &pb.BlocksRequest{NodeValidation: false})
	if err != nil {
		fmt.Println("failed to subscribe new block: ", err)
		return
	}

	for {

		reply, err := stream.Recv()
		if err != nil {
			fmt.Println("stream receive error: ", err)
		}

		fmt.Println("recieve new block, block hash is ", reply.Hash)
	}
}
```
{% endtab %}
{% endtabs %}

#### Proto

The code of `relay.proto` is as follows:

```go
syntax = "proto3";

package blockchain;

option go_package = "/Users/code/relay/grpcServer"; 
service Gateway {
  rpc SendTx (SendTxRequest) returns (SendTxReply) {}
  rpc SendTxs (SendTxsRequest) returns (SendTxsReply) {}
  rpc NewTxs (TxsRequest) returns (stream TxsReply){}
  rpc NewBlocks (BlocksRequest) returns (stream BlocksReply){}
}

message TxsRequest{
  bool node_validation = 1;
}

message Tx{
  bytes from = 1;
  int64 timestamp = 2;
  bytes raw_tx = 3;
}

message TxsReply{
   Tx tx = 1;
}

message BlocksRequest{
  bool node_validation = 1;
}

message BlockHeader{
  string parent_hash = 1;
  string sha3_uncles = 2;
  string miner = 3;
  string state_root = 4;
  string transactions_root = 5;
  string receipts_root = 6;
  string logs_bloom = 7;
  string difficulty = 8;
  string number = 9;
  uint64 gas_limit = 10;
  uint64 gas_used = 11;
  uint64 timestamp = 12;
  bytes extra_data = 13;
  string mix_hash = 14;
  uint64 nonce = 15;
  uint64 base_fee_per_gas = 16;
  string withdrawals_root = 17;
  uint64 blob_gas_used = 18;
  uint64 excess_blob_gas = 19;
  string parent_beacon_block_root = 20;
  string requests_hash = 21;
}

message NextValidator{
  string block_height = 1;
  string coinbase = 2;
}

message BlocksReply{
  string hash = 1;
  BlockHeader header = 2;
  repeated NextValidator nextValidator = 3;
  repeated Tx txs = 4;
}

message Transaction {
  string content = 1;
}

message Transactions {
  repeated Transaction transactions = 1;
}

message SendTxRequest {
  string transaction = 1;
}

message SendTxsRequest {
  string transactions = 1;
}

message SendTxReply {
  string tx_hash = 1;
}

message SendTxsReply {
  repeated string tx_hashs = 1;
}
```

### Response Example

**Success**

```json
{
	"hash": "0xe4a85aaa8cf4c85c4abf59c06b744ae680941e7ffba351fd4c166f0264e860de",
	"header": {
		"parent_hash": "0x0105992a6c305b0bbfbf7b4eebbbc92c4dca1bb2a18c1f93c551afc2c73c0668",
		"sha3_uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
		"miner": "0x9f1b7FAE54BE07F4FEE34Eb1aaCb39A1F7B6FC92",
		"state_root": "0xce2a7e35a643e5840e510df9eea8ba9177dd62fe743439ef159d7d9e783afd28",
		"transactions_root": "0x63c952adbe28ef60d8e723acec299ca62a915b51d05b060cb6fe97bb28814ab3",
		"receipts_root": "0xd3232a5308a69591dda132bd4ce1c1dbd5968ed9feee5f1385a5e7ad1b10527b",
		"logs_bloom": "8477149424673580296520836103527692456522284192111573121045681184670018737794069194486118998020237483301054022136573687133364882576385281526094331485603954025070865625599001965016471857986082361337851986790329901990002923474000909032395056767065187980257808021700240863783968353544053840361314138235944346718686049569138609472702141575196319415411746602993576034439427711191239874622732325947862558346553652593648034520192193214092215911630192819139737571343595880627533215841996399050061528845101804940660169259391681335118771766679784629118424769368884717750281985023526825099358014255162966726322082309693440030370",
		"difficulty": "2",
		"number": "40370748",
		"gas_limit": 139997863,
		"gas_used": 12180159,
		"timestamp": 1720674742,
		"extra_data": "2IMBBAqEZ2V0aIhnbzEuMjEuNYVsaW51eAAAAEj6ewX4tYMf//+4YKyfFdynrjSSLQKJj9FdhVSwaDFtnAdMnqNzKeVm2agkWE79uSpjZ2Wbrxt2eCFSvBRuPLIhONzRYpQgx/DeH/Xrhsz/A+EZw4BFaOVV3ch76i1QdJy1CADUqoc/XEaBHvhMhAJoAjqg3GksrNaxsaC5pxx8JS0YFpA6OQh7ofh7pTmra2Ve7lOEAmgCO6ABBZkqbDBbC7+/e07ru8ksTcobsqGMH5PFUa/CxzwGaIBHk8oYx40FGg4hnpMSQsTiOEAObH2DV3ytkUGOdEk4KENXgVf0/xsCA56w2r/VIqP7ux7HCD9vrh7H7fjxdU3xAA==",
		"mix_hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
		"noncpe":0,
		"base_fee_per_gas":0,
		"withdrawals_root":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
		"blob_gas_used": 131072,
		"excess_blob_gas":0,
		"parent_beacon_block_root":""
	
	},
	"nextValidator": [{
		"block_height": "40370749",
		"coinbase": "0xbdcc079bbb23c1d9a6f36aa31309676c258abac7"
	}, {
		"block_height": "40370750",
		"coinbase": "0xc2d534f079444e6e7ff9dabb3fd8a26c607932c8"
	}],
	"txs": [{
		"raw_tx": "+K6DzSWShQEqBfIAgwEwY5R2021E3EWV6NLrOtdF8XXtoTQoT4C4RKkFnLsAAAAAAAAAAAAAAAAFWjs3lXv70zRb7Zlo5+jdVtZwZgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGBV8GygaywAAgZOguQQg9l/3+bmldXAcm9lsSsgxwq8m+wqcXh/KwbKJ7E+gAS8g6ZuII8KJWFBFpVzTcyptzqfro00WDKR3oi1ly7g="
	}, {
		"raw_tx": "+QVPgxUyKIUBKgXyAIMHoSCUE3kk18NoFuDcrwFuthfMLJLAV4KAuQTkyYB1OQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAyAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAQEBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAoAAAAAAAAAAAAAAAGQptag/qdv1AH5JjIvJ4fgACqZ4AQoECA4FDA0HAQsPAgAJBgMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw9aP+EAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADD2B6d4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMPYNaEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw9jajbAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADD2a1DgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMPZrUOAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw9qNqnAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADD2o2qcAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMPajapwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw9u7fAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADD3ePNwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMPiGlgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw+IaWAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADD4n5xAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMPqOuWAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw+o8GuAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAbIQBTEAMmDL3iUlt3D7Hrj6ACjQhWYvzwIOV06fd5+CxHWRbBH4sYJEbXQ/y6+cimsuGJSFGXoSkH5dZf7RDlpIA584nEMYIrJUVhB3PQZWCiY951T9xxDCPwvuqG40jEYJR68n788tw7y1nUnZwIJVWAoko+4Z/immcv2IcTzZ53J2RNPMiqAlb21gAwDfKQtEF7HpWEwn422R1b9VDfEey+3vxMAzZcisrwfRh49shbMLJpz4CnFt5FyAS9eI9AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABhy0jMwJkMmVvzRghVWVS2LwKSffduFZtzpO03R2sucDE1hqfyr8bpMWOG7oHhhdknlcQ/+leSGQf9X8GF1g7MBIWrGp/Ri03p4GYSY3Rj6RaN+SWlHEPySIll217WvNyHf3dtjfz9ic4gXWSc4l+vP5fXff/59EgoqDMhgEOP/iOfCAJsJt5Uisq/0Cpz/+2fzV4KknIckM8W1VsH8S34kLRauuV1UKaY7AU0K0BIA1dr8HMkOm/Ci+fDyuSFJY94GToC30fN+eoJ0+scIcdj9XpEmHUxWAT6TtSwk1zVm14HyboHCzcmyYjRIcHxPeMrJiwXSKXkgCTggJw9V0ETOLYuZL"
	}, {
		"raw_tx": "+K6DzSWThQEqBfIAgwEw4JRV05gyb5kFn/d1SFJGmZAnsxl5VYC4RKkFnLsAAAAAAAAAAAAAAADt2wDQgCLygqAXnLTDVpf1DvOxOAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALA2UyUvg7vwAgZOgReRi2miA5qA+ERtjgmiOmqwmtCDhvtj0TFKrtUAuQWegP7xd7Gj89uQM03KMcfceKvAKkxwwajcJB1M8xHQlR4g="
	}, {
		"raw_tx": "+QFrBYTodUcAgwQ7j5Td7QSbsOJzaTkPQPjEolIpjjUwjoC5AQS39rSuAAAAAAAAAAAAAAAAVdOYMm+ZBZ/3dUhSRpmQJ7MZeVUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHP5UVLi+/8gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAcWBJiX8HPgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAisdqUcyVDZgi1ouD/hrZezLNWA0AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIGUoDDbxwrT1VjxX+u9C70/BMIgFYEATNA/P4NaAIu8AYupoFgRK+X8L9sYxgVNJobVpne44NbM/wQTYLLDGqanoAeH"
	}]
}
```

**Error**

```
rpc error: code = Unknown desc = data streams have exceeded its max limit [5]
```

