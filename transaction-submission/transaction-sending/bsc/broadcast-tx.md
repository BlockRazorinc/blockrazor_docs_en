---
description: >-
  Use the BlockRazor BSC Broadcast Tx API. Review the API, scenarios, benchmark,
  price, rate limit, endpoint, and integration example.
metaLinks:
  canonical: broadcast-tx.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/bsc/broadcast-tx
---

# BSC Broadcast Tx API Reference

<details>

<summary><strong>What is Broadcast Tx</strong></summary>

Broadcast Tx is a fast transaction sending service provided by BlockRazor to help users send transactions with lower latency. It is part of the Fast ecosystem, but unlike the standard Fast model which requires attaching a tip to the transaction, Broadcast Tx does not require users to pay extra for a tip within the transaction, making it more suitable as a low-barrier, fast sending entry point.

Currently, Broadcast Tx offers two methods: `SendTx` and `SendTxs`, which are used to send single transactions and batch transactions.

It's important to note that while Broadcast Tx belongs to the Fast ecosystem, it is not equivalent to a private transmission channel with full transaction protection capabilities. Transactions sent via Broadcast Tx still enter the public propagation path and therefore _DO NOT_ have MEV protection capabilities.

</details>

<details>

<summary><strong>In what scenarios should you choose Broadcast Tx</strong></summary>

* No tips needed, lower barrier to entry\
  Unlike the standard Fast model, Broadcast Tx does not require adding tips to transactions, making it more suitable for users who want to quickly integrate but do not want to modify the transaction incentive structure.
* Suitable for scenarios where speed is a requirement but MEV protection is not currently emphasized\
  If your priority is to send transactions out as quickly as possible, rather than hiding transactions or mitigating risks like sandwiches and frontrunnings through private paths, then Broadcast Tx would be a more straightforward option.

</details>

### Benchmark

In our benchmark on transaction inclusion latency, we conducted multiple rounds of comparisons between BlockRazor and regular Nodes in four regions: Dublin, Frankfurt, Tokyo, and Virginia. The evaluation criteria consisted of two layers: first, comparing whether the transaction is included in the earlier block; second, if both transactions were included within the same block, we further compared the order of their transaction indices. The results are as follows:

<table><thead><tr><th width="187.37109375">Region</th><th width="511.00390625">BlockRazor Total Lead Rate</th></tr></thead><tbody><tr><td>Dublin</td><td><strong>88.7%</strong><br><strong>-</strong> Same block but better index: 82.9%<br>- Earlier block: 5.8%</td></tr><tr><td>Frankfurt</td><td><strong>85.4%</strong><br><strong>-</strong> Same block but better index: 81.3%<br>- Earlier block: 4.2%</td></tr><tr><td>Tokyo</td><td><strong>85.1%</strong><br><strong>-</strong> Same block but better index: 78.7%<br>- Earlier block: 6.4%</td></tr><tr><td>Virginia</td><td><strong>97.9%</strong><br><strong>-</strong> Same block but better index: 95.7%<br>- Earlier block: 2.1%</td></tr></tbody></table>

In terms of percentage results, BlockRazor maintained a higher overall lead across all regions. In the Dublin region, BlockRazor led by 88.7%, in Frankfurt by 85.4%, in Tokyo by 85.1%, and in Virginia by a remarkable 97.9%.

Looking further at the leading mechanism, BlockRazor's advantages are mainly reflected in two aspects: First, in most leading rounds, even when entering the same block as a regular Node, BlockRazor still obtains a higher transaction index; second, in some rounds, BlockRazor can even directly complete transaction inclusion one block ahead. This indicates that BlockRazor not only has a greater chance of entering earlier block windows, but also has a better chance of achieving a superior ranking position in the competition for the same block, thus forming a stable submission advantage.

### Price & Rate Limit

<table><thead><tr><th width="137.21484375">Payment Method</th><th width="256.375">Limit</th><th width="198.984375">Price</th><th width="107.0546875">Action</th></tr></thead><tbody><tr><td>Free</td><td><p><code>SendTx</code></p><ul><li>TPS：10 Txs / 5s</li><li>Daily Tx Limit：10</li></ul></td><td>Free</td><td>-</td></tr><tr><td>Personalized</td><td><p><code>SendTx</code></p><ul><li>TPS：100 Txs / 5s</li><li>Daily Tx Limit：100000</li></ul><p><code>SendTxs</code></p><ul><li>BPS：4 batches / 1s</li><li>Txs per Batch：10</li></ul></td><td>$50 / day<br>$500 / month</td><td><br><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=bsc&#x26;serviceId=bsc_fast_tx&#x26;billing=day" class="button primary small">Subscribe</a></td></tr><tr><td>Package</td><td><p><code>SendTx</code></p><ul><li>TPS：100 Txs / 5s</li><li>Daily Tx Limit：100000</li></ul><p><code>SendTxs</code></p><ul><li>BPS：4 batches / 1s</li><li>Txs per Batch：10</li></ul></td><td>$1250 / month<br>packaged with 9 other services. </td><td><a href="https://blockrazor.io/#/portal/pricing?redirect=pricing&#x26;purchaseMode=package&#x26;billing=month" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Endpoint

<table><thead><tr><th width="160">Region</th><th>Relay Address</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75:50051</td></tr><tr><td>Tokyo</td><td>63.254.162.18:50051</td></tr><tr><td>Dublin</td><td>141.98.217.82:50051</td></tr><tr><td>Virginia</td><td>208.91.105.204:50051</td></tr></tbody></table>

### SendTx

#### Request Parameter

<table><thead><tr><th width="156">Parameters</th><th width="125">Mandatory</th><th width="93">Format</th><th>Example</th><th>Description</th></tr></thead><tbody><tr><td>Transaction</td><td>Mandatory</td><td>String</td><td>"0xf8……8219"</td><td>signed raw tx</td></tr></tbody></table>

#### Request Example

[https://github.com/BlockRazorinc/relay\_example](https://github.com/BlockRazorinc/relay_example)

{% tabs %}
{% tab title="gRPC" %}
```go
package main

import (
	"context"
	"encoding/hex"
	"fmt"
	"math/big"

	// directory of the generated code using the provided relay.proto file
	pb "github.com/BlockRazorinc/relay_example/protobuf"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/core/types"
	"github.com/ethereum/go-ethereum/crypto"
	"github.com/ethereum/go-ethereum/rlp"
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

	// use the Gateway client connection interface.
	client := pb.NewGatewayClient(conn)

	// create context
	ctx := context.Background()

	// replace with your address
	from_private_address := "42b565……44d05c"
	to_public_address := "0x4321……3f1c66"

	// replace with your transaction data
	nonce := uint64(1)
	toAddress := common.HexToAddress(to_public_address)
	var data []byte
	gasPrice := big.NewInt(1e9)
	gasLimit := uint64(22000)
	value := big.NewInt(0)
	chainid := types.NewEIP155Signer(big.NewInt(56))

	// create new transaction
	tx := types.NewTransaction(nonce, toAddress, value, gasLimit, gasPrice, data)

	privateKey, err := crypto.HexToECDSA(from_private_address)
	if err != nil {
		fmt.Println("fail to casting private key to ECDSA")
		return
	}

	// sign transaction by private key
	signedTx, err := types.SignTx(tx, chainid, privateKey)
	if err != nil {
		fmt.Println("fail to sign transaction")
		return
	}

	// use rlp to encode your transaction
	body, _ := rlp.EncodeToBytes([]types.Transaction{*signedTx})

	// encode []byte to string
	encode_tx := hex.EncodeToString(body)

	// send raw tx by BlockRazor
	res, err := client.SendTx(ctx, &pb.SendTxRequest{Transaction: encode_tx})

	if err != nil {
		fmt.Println("failed to send raw tx: ", err)
		return
	} else {
		fmt.Println("raw tx sent by BlockRazor, tx hash is ", res.TxHash)
	}

}
```
{% endtab %}

{% tab title="HTTP" %}
```bash
curl <Endpoint_URL> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":["0x02f8……869514"],"id":1}'
```
{% endtab %}
{% endtabs %}

#### Response Example

**Success**

```json
{
 "tx_hash":"0x2944……b2188f"
}
```

**Error**

```
rpc error: code = Unknown desc = invalid transaction format
```

### SendTxs

#### Request Parameter

<table><thead><tr><th>Parameters</th><th width="124">Mandatory</th><th width="131">Format</th><th>Example</th><th>Description</th></tr></thead><tbody><tr><td>Transactions</td><td>Mandatory</td><td>Array[String]</td><td>["0xf8……8219", "0xcb……2ac0"]</td><td>signed raw txs</td></tr></tbody></table>

#### Request Example

[https://github.com/BlockRazorinc/relay\_example](https://github.com/BlockRazorinc/relay_example)

```go
package main

import (
	"context"
	"encoding/hex"
	"fmt"
	"math/big"

	// directory of the generated code using the provided relay.proto file
	pb "github.com/BlockRazorinc/relay_example/protobuf"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/core/types"
	"github.com/ethereum/go-ethereum/crypto"
	"github.com/ethereum/go-ethereum/rlp"
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

	// create context
	ctx := context.Background()

	// replace with your address
	from_private_address1 := "6c0456……8b8003"
	from_private_address2 := "42b565……44d05c"
	to_public_address := "0x4321……3f1c66"

	// replace with your transaction data
	nonce1 := uint64(1)
	nonce2 := uint64(1)
	toAddress := common.HexToAddress(to_public_address)
	var data []byte
	gasPrice := big.NewInt(1e9)
	gasLimit := uint64(22000)
	value := big.NewInt(0)
	chainid := types.NewEIP155Signer(big.NewInt(56))

	// create new transaction
	tx1 := types.NewTransaction(nonce1, toAddress, value, gasLimit, gasPrice, data)
	tx2 := types.NewTransaction(nonce2, toAddress, value, gasLimit, gasPrice, data)

	privateKey1, err := crypto.HexToECDSA(from_private_address1)
	if err != nil {
		fmt.Println("fail to casting private key to ECDSA")
		return
	}

	privateKey2, err := crypto.HexToECDSA(from_private_address2)
	if err != nil {
		fmt.Println("fail to casting private key to ECDSA")
		return
	}

	// sign transaction by private key
	signedTx1, err := types.SignTx(tx1, chainid, privateKey1)
	if err != nil {
		fmt.Println("fail to sign transaction")
		return
	}

	signedTx2, err := types.SignTx(tx2, chainid, privateKey2)
	if err != nil {
		fmt.Println("fail to sign transaction")
		return
	}

	// use rlp to encode your transaction
	body, _ := rlp.EncodeToBytes([]types.Transaction{*signedTx1, *signedTx2})

	// encode []byte to string
	encode_txs := hex.EncodeToString(body)

	// send raw tx batch by BlockRazor
	res, err := client.SendTxs(ctx, &pb.SendTxsRequest{Transactions: encode_txs})

	if err != nil {
		fmt.Println("failed to send raw tx batch: ", err)
		return
	} else {
		fmt.Println("raw tx batch sent by BlockRazor, tx hashes are ", res.TxHashs)
	}

}
```

#### Response Example

**Success**

```json
{
 "tx_hash":["0xdb95……584813", "0x887d……7aba19"]
}
```

**Error**

```
rpc error: code = Unknown desc = invalid transaction format
```

### **Proto**

The code of `relay.proto` is as follows:

```
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
