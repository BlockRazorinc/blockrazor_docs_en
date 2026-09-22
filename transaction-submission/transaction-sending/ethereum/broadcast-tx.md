---
description: >-
  Use the BlockRazor Ethereum Broadcast Tx API. Review the API, scenarios,
  price, rate limit, endpoint, and integration example.
metaLinks:
  canonical: broadcast-tx.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/ethereum/broadcast-tx
---

# Ethereum Broadcast Tx API Reference

<details>

<summary><strong>What is Broadcast Tx</strong></summary>

Broadcast Tx is a fast transaction sending service provided by BlockRazor to help users send transactions with lower latency. It is part of the Fast ecosystem, but unlike the standard Fast model which requires attaching a tip to the transaction, Broadcast Tx does not require users to pay extra for a tip within the transaction, making it more suitable as a low-barrier, fast sending entry point.

Currently, Broadcast Tx offers methods `SendTx` which are used to send single transactions.

It's important to note that while Broadcast Tx belongs to the Fast ecosystem, it is not equivalent to a private transmission channel with full transaction protection capabilities. Transactions sent via Broadcast Tx still enter the public propagation path and therefore _DO NOT_ have MEV protection capabilities.

</details>

<details>

<summary><strong>In what scenarios should you choose Broadcast Tx</strong></summary>

* No tips needed, lower barrier to entry\
  Unlike the standard Fast model, Broadcast Tx does not require adding tips to transactions, making it more suitable for users who want to quickly integrate but do not want to modify the transaction incentive structure.
* Suitable for scenarios where speed is a requirement but MEV protection is not currently emphasized\
  If your priority is to send transactions out as quickly as possible, rather than hiding transactions or mitigating risks like sandwiches and frontrunnings through private paths, then Broadcast Tx would be a more straightforward option.

</details>

### Price & Rate Limit

<table><thead><tr><th width="148.92578125">Payment Method</th><th width="241.35546875">Limit</th><th width="197.57421875">Price</th><th>Action</th></tr></thead><tbody><tr><td>Free</td><td><ul><li>TPS：10 Txs / 5s</li><li>Daily Tx Limit：10</li></ul></td><td>Free</td><td>-</td></tr><tr><td>Personalized</td><td><ul><li>TPS：100 Txs / 5s</li><li>Daily Tx Limit：100000</li></ul></td><td>$50 / day<br>$500 / month</td><td><a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=ethereum&#x26;serviceId=ethereum_fast_tx&#x26;billing=day" class="button primary small">Subscribe</a></td></tr></tbody></table>

### Endpoint

<table><thead><tr><th width="160">Region</th><th>Relay Address</th></tr></thead><tbody><tr><td>Frankfurt</td><td>64.130.47.75:50061</td></tr><tr><td>Tokyo</td><td>63.254.162.18:50061</td></tr><tr><td>Virginia</td><td>208.91.105.204:50061</td></tr></tbody></table>

### SendTx

#### Request Parameter

<table><thead><tr><th width="156">Parameters</th><th width="125">Mandatory</th><th width="93">Format</th><th>Example</th><th>Description</th></tr></thead><tbody><tr><td>Transaction</td><td>Mandatory</td><td>String</td><td>"0xf8……8219"</td><td>signed raw tx</td></tr></tbody></table>

#### Request Example

[https://github.com/BlockRazorinc/eth\_relay\_example](https://github.com/BlockRazorinc/eth_relay_example)

{% tabs %}
{% tab title="gRPC" %}
```go
package main

import (
	"context"
	"fmt"
	"math/big"
	"time"

	// directory of the generated code using the provided relay.proto file
	pb "github.com/BlockRazorinc/eth_relay_example/protobuf"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/core/types"
	"github.com/ethereum/go-ethereum/crypto"
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

	// use the Relay client connection interface.
	client := pb.NewRelayClient(conn)

	// create context
	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()

	// replace with your address
	fromPrivateAddress := "42b565......44d05c"
	toPublicAddress := "0x4321......3f1c66"

	// replace with your transaction data
	nonce := uint64(1)
	toAddress := common.HexToAddress(toPublicAddress)
	var data []byte
	gasLimit := uint64(21000)
	value := big.NewInt(0)
	chainID := big.NewInt(1)
	gasTipCap := big.NewInt(2_000_000_000)
	gasFeeCap := big.NewInt(30_000_000_000)

	// create new EIP-1559 transaction
	tx := types.NewTx(&types.DynamicFeeTx{
		ChainID:   chainID,
		Nonce:     nonce,
		GasTipCap: gasTipCap,
		GasFeeCap: gasFeeCap,
		Gas:       gasLimit,
		To:        &toAddress,
		Value:     value,
		Data:      data,
	})

	privateKey, err := crypto.HexToECDSA(fromPrivateAddress)
	if err != nil {
		fmt.Println("fail to casting private key to ECDSA")
		return
	}

	// sign transaction by private key
	signedTx, err := types.SignTx(tx, types.LatestSignerForChainID(chainID), privateKey)
	if err != nil {
		fmt.Println("fail to sign transaction")
		return
	}

	// marshal signed transaction to raw binary bytes
	rawTx, err := signedTx.MarshalBinary()
	if err != nil {
		fmt.Println("fail to marshal signed transaction")
		return
	}

	// send raw tx by BlockRazor
	res, err := client.SendTx(ctx, &pb.SendTxRequest{RawTx: rawTx})
	if err != nil {
		fmt.Println("failed to send raw tx: ", err)
		return
	}
	fmt.Println("raw tx sent by BlockRazor, tx hash is ", res.TxHash)
}


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

### **Proto**

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
