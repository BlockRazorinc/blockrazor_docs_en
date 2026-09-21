---
description: >-
  Explore the BlockRazor Base Get FlashBlockTransaction service, advantage,
  endpoint, price, and integration guide.
metaLinks:
  canonical: get-flashblocktransaction.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/base/get-flashblocktransaction
---

# Base Get FlashBlockTransaction Integration Guide

<details>

<summary><strong>What is Base Get FlashBlockTransaction</strong></summary>

`Get FlashBlockTransaction` is a real-time FlashBlock transaction data stream provided by BlockRazor for Base, used to retrieve transaction data from FlashBlocks on the Base with lower latency. This interface supports both gRPC and WebSocket protocols, making it suitable for transaction systems, monitoring systems, and real-time processing systems that are more sensitive to data arrival time.

On Base, FlashBlock can be understood as a "sub-block" data stream that occurs before the formal block is formed, typically pushed continuously at a frequency of about 200ms. Compared to the standard block time of about 2 seconds, FlashBlock can provide transaction-related feedback much earlier, making it more suitable for low-latency scenarios that require quick on-chain awareness of changes.

For trading bots, quantitative strategies, real-time monitoring platforms, and front-end trading applications, waiting for blocks often means longer response times. The value of  `Get FlashBlockTransaction`  is to help the system receive transaction and block change signals earlier, before the block arrives, thus buying more time for subsequent judgment and response.

</details>

<details>

<summary><strong>What is the difference between <code>Get FlashBlockTransaction</code> and <code>Get FlashBlockStream</code></strong></summary>

Both `Get FlashBlockStream` and `Get FlashBlockTransaction` subscribe to FlashBlock data, but the key difference lies in the returned data format. Get FlashBlockStream returns binary data, which needs to be parsed into structured data after acquisition; while Get FlashBlockTransaction directly returns structured transaction data.

</details>

### Price

The price is $25 / stream / day and $250 / stream / month. <a href="https://blockrazor.io/#/login?redirect=pricing&#x26;purchaseMode=personalized&#x26;chain=base&#x26;serviceId=base_flashblock&#x26;billing=day" class="button primary small">Subscribe</a>

{% hint style="info" %}
The number of data streams that can be subscribed to is calculated on a shared basis across all regions. For example, if you purchase one stream, you can only subscribe in one region; you will not be able to subscribe in other regions.
{% endhint %}

### Endpoint

{% tabs %}
{% tab title="gRPC" %}
<table><thead><tr><th width="136.83203125">Region</th><th>Endpoint</th></tr></thead><tbody><tr><td>Frankfurt</td><td>frankfurt.grpc.base.blockrazor.xyz:80</td></tr><tr><td>Virginia</td><td>virginia.grpc.base.blockrazor.xyz:80</td></tr><tr><td>Tokyo</td><td>tokyo.grpc.base.blockrazor.xyz:80</td></tr></tbody></table>
{% endtab %}

{% tab title="WebSocket" %}
<table><thead><tr><th width="139.7421875">地區</th><th>端點</th></tr></thead><tbody><tr><td>法蘭克福</td><td>ws://frankfurt.base.blockrazor.xyz:81/ws</td></tr><tr><td>弗吉尼亞</td><td>ws://virginia.base.blockrazor.xyz:81/ws</td></tr><tr><td>日本</td><td>ws://tokyo.base.blockrazor.xyz:81/ws</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Request Example

{% tabs %}
{% tab title="CLI" %}
{% code overflow="wrap" %}
```javascript
wscat -H "Authorization: <AUTH_HEADER>" \
  -c ws://frankfurt.base.blockrazor.xyz:81/ws \
  --wait 1000 \
  --execute '{"jsonrpc": "2.0", "id": 1, "method": "subscribe_FlashTransaction", "params": []}'
```
{% endcode %}
{% endtab %}

{% tab title="Websocket" %}
{% code overflow="wrap" %}
```go
type FlashTransactionWebSocketResponse struct {
    JsonRPC string                           `json:"jsonrpc"`
    Result  *basepb.NewFlashblockTransaction `json:"result"`
}

func GetWebSocketFlashTransactionStream(authToken string) {
    header := http.Header{}
    header.Set("Authorization", authToken)

    // Dial the WebSocket server.
    dialer := websocket.DefaultDialer
    conn, resp, err := dialer.Dial(websocketAddr, header)
    if err != nil {
       if resp != nil {
          log.Fatalf("[WebSocket] Dial failed: %v (HTTP status: %s)", err, resp.Status)
       }
       log.Fatalf("[WebSocket] Dial failed: %v", err)
    }
    defer conn.Close()
    log.Printf("[WebSocket] Successfully connected to %s", websocketAddr)

    // Prepare the JSON-RPC subscription request.
    req := map[string]interface{}{
       "jsonrpc": "2.0",
       "method":  "subscribe_FlashTransaction",
       "params":  []interface{}{},
       "id":      1,
    }
    reqB, _ := json.Marshal(req)
    if err := conn.WriteMessage(websocket.TextMessage, reqB); err != nil {
       log.Fatalf("[WebSocket] Failed to send subscription request: %v", err)
    }
    log.Printf("[WebSocket] Subscription request sent: %s", string(reqB))

    for {
       _, msg, err := conn.ReadMessage()
       if err != nil {
          log.Printf("[WebSocket] read error: %v", err)
          return
       }

       var outer FlashTransactionWebSocketResponse
       if err := json.Unmarshal(msg, &outer); err != nil {
          log.Printf("[WebSocket] unmarshal outer message failed: %v, raw=%s", err, string(msg))
          continue
       }

       if outer.Result == nil {
          log.Printf("[WebSocket] Received message without a valid result field. Raw data: %s", string(msg))
          continue
       }

       tx := outer.Result

       b, err := protojson.Marshal(tx)
       if err != nil {
          continue
       }

       jsonString := string(b)
       printPretty(jsonString)
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="gRPC" %}
{% code overflow="wrap" %}
```go
func GetFlashTransactionStream(authToken string) {
    log.Printf("[FlashTransactionStream] Attempting to connect to gRPC server at %s...", grpcAddr)

    // Establish a connection to the gRPC server with a timeout.
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()
    conn, err := grpc.DialContext(ctx, grpcAddr,
       grpc.WithTransportCredentials(insecure.NewCredentials()))
    if err != nil {
       log.Printf("[FlashTransactionStream] Failed to connect to gRPC server: %v", err)
       return
    }
    defer conn.Close()

    log.Println("[FlashTransactionStream] Successfully connected to gRPC server.")
    client := basepb.NewBaseApiClient(conn)

    // Create a new context with authentication metadata for the stream subscription.
    streamCtx := metadata.NewOutgoingContext(context.Background(), metadata.Pairs("authorization", authToken))
    stream, err := client.GetNewFlashblockTransactionsStream(streamCtx, &basepb.GetNewFlashblockTransactionsStreamRequest{})
    if err != nil {
       log.Printf("[FlashTransactionStream] Failed to subscribe to stream: %v", err)
       return
    }

    log.Println("[FlashTransactionStream] Subscription successful. Waiting for new txs...")

    // Loop indefinitely to receive messages from the stream.
    for {
       tx, err := stream.Recv()
       if err != nil {
          if err == io.EOF {
             log.Println("[FlashTransactionStream] Stream closed by the server (EOF).")
          } else {
             log.Printf("[FlashTransactionStream] An error occurred while receiving data: %v", err)
          }
          break
       }

       log.Printf("=> [FlashTransactionStream] Received new tx: BlockNumber=%s, txHash=%s, TransactionIndex=%s",
          tx.GetBlockNumber(),
          tx.GetHash(),
          tx.GetTransactionIndex(),
       )
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Proto" %}
{% code overflow="wrap" %}
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

message GetNewFlashblockTransactionsStreamRequest {
}

message NewFlashblockTransactionAccessListEntry {
  string address = 1;
  repeated string storage_keys = 2;
}

message NewFlashblockTransactionLog {
  string address = 1;
  repeated string topics = 2;
  string data = 3;
  string block_hash = 4;
  string block_number = 5;
  string block_timestamp = 6;
  string transaction_hash = 7;
  string transaction_index = 8;
  string log_index = 9;
  bool removed = 10;
}

message NewFlashblockTransaction {
  string type = 1;
  string chain_id = 2;
  string nonce = 3;
  string gas = 4;
  string max_fee_per_gas = 5;
  string max_priority_fee_per_gas = 6;
  google.protobuf.StringValue to = 7;
  string value = 8;
  repeated NewFlashblockTransactionAccessListEntry access_list = 9;
  string input = 10;
  string r = 11;
  string s = 12;
  string y_parity = 13;
  string v = 14;
  string hash = 15;
  google.protobuf.StringValue block_hash = 16;
  string block_number = 17;
  string transaction_index = 18;
  string block_timestamp = 19;
  string from = 20;
  string gas_price = 21;
  repeated NewFlashblockTransactionLog logs = 22;
  string gas_used = 23;
  string status = 24;
  string cumulative_gas_used = 25;
  google.protobuf.StringValue contract_address = 26;
  string logs_bloom = 27;
}


service BaseApi {
  rpc SendTransaction(SendTransactionRequest) returns (SendTransactionResponse);
  rpc GetBlockStream(GetBlockStreamRequest) returns (stream BaseBlock);
  rpc GetRawFlashBlockStream(GetRawFlashBlocksStreamRequest) returns (stream RawFlashBlockStrResponse);
  rpc GetNewFlashblockTransactionsStream(GetNewFlashblockTransactionsStreamRequest) returns (stream NewFlashblockTransaction);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

**Normal**

{% tabs %}
{% tab title="Websocket" %}
{% code overflow="wrap" %}
```javascript
{"jsonrpc":"2.0","result":{"type":"0x2","chain_id":"0x2105","nonce":"0x57f5bd","gas":"0x1d4c0","max_fee_per_gas":"0x1dcf6155","max_priority_fee_per_gas":"0x0","to":{"value":"0xec0e36a6060339694c618ffffcc9ec7da21cb0cc"},"value":"0x0","input":"0xd67704ad0000000000000000000000000000000000000000000000000000000365682750","r":"0x452d04d6fa2805151f35a6372c84ae6ed1afeb3fa7e681bfe12eb86fec4856f1","s":"0x73abdc6f5066c211b940746c7d507947c4a3ff3e9a007388a173de0063bc627e","y_parity":"0x1","v":"0x1","hash":"0xb2008689261342c043d1c29a1986bfc307d2add6e1b8b05bd095460b2762c27c","block_number":"0x2e31a2d","transaction_index":"0xb1","block_timestamp":"0x6a50913d","from":"0xabbac9becc5b171842ae47703dfa6640b23c9710","gas_price":"0x4c4b40","gas_used":"0x67a4","status":"0x1","cumulative_gas_used":"0x273580b","logs_bloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"},"id":null}
```
{% endcode %}
{% endtab %}

{% tab title="gRPC" %}
{% code overflow="wrap" %}
```python
type:"0x2"  chain_id:"0x2105"  nonce:"0x1bdf78"  gas:"0x3d0900"  max_fee_per_gas:"0x1012aca"  max_priority_fee_per_gas:"0x4a0fca"  to:{value:"0x1722b0b1a1ff934b82d07d7ab540d60d94648cbd"}  value:"0x0"  input:"0xb802000d8b00010000640001609a9cffe20cb54633a58d2ddaa0afd9d283192ecaa77e89243131d31901a07a0001000bb800c8609a9cffe20cb54633a58d2dd9a250114b47885fe27d9c9734a21378a456f267"  r:"0x67fe6c98362d0184052a69ea2422bc00dfaddeb8ab5224a11881fee82a1087a6"  s:"0x20d7a774e39d864017ac7174857dc65c2468d6bd82741302a7abffedbd2630e6"  y_parity:"0x1"  v:"0x1"  hash:"0x224eff2fee593c016e271911e68795f6b88a3d8867c1c650678c2169f7a28a17"  block_number:"0x2cfe45c"  transaction_index:"0xe"  block_timestamp:"0x6a2a259b"  from:"0xfea6e8b9d60ccfc36498f56bf316b63e2812047b"  gas_price:"0x965b0a"  gas_used:"0xa25f"  status:"0x1"  cumulative_gas_used:"0x2f2ca6"  logs_bloom:"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
```
{% endcode %}
{% endtab %}
{% endtabs %}

**Abnormal**

{% tabs %}
{% tab title="Websocket" %}
{% code overflow="wrap" %}
```json
2026/07/10 11:27:30 [WebSocket] Subscription request sent: {"id":1,"jsonrpc":"2.0","method":"subscribe_FlashTransaction","params":[]}
2026/07/10 11:27:31 [WebSocket] Received message without a valid result field. Raw data: {"jsonrpc":"2.0","error":{"code":-32000,"message":"invalid authentication credentials. please ensure your auth token is correct and try again"},"id":null}
2026/07/10 11:27:31 [WebSocket] read error: websocket: close 1006 (abnormal closure): unexpected EOF
```
{% endcode %}
{% endtab %}

{% tab title="gRPC" %}
{% code overflow="wrap" %}
```
[FlashTransactionStream] An error occurred while receiving data: rpc error: code = FailedPrecondition desc = invalid authentication credentials. please ensure your auth token is correct and try again
```
{% endcode %}
{% endtab %}
{% endtabs %}

