---
description: >-
  Learn the integration of 'Send Transaction in Binary' in BlockRazor Solana
  Transaction Sending mode. See endpoints, request example and request
  parameter.
metaLinks:
  canonical: send-in-binary.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/send-transaction/send-in-binary
---

# Solana Send Transaction in Binary

### Introduction <a href="#jie-shao" id="jie-shao"></a>

{% hint style="warning" %}
Solana's transaction sending service is not bound to the subscription plan, with rate limit default to 3 TPS. If you need to increase the TPS limit, please [contact](https://discord.com/invite/qqJuwRb8Nh) us and we will handle it as soon as possible.
{% endhint %}

`Send in Binary` is used to send signed transactions on Solana. Compared with the [Send Transaction](./), it allows signed transactions to be submitted in binary format instead of being converted to Base64 first. The benefits are that it removes one layer of encoding and decoding overhead, and because the payload is smaller, the transaction is less likely to be split into multiple packets during transmission, which can help reduce the resulting latency.

### Endpoint <a href="#xian-liu" id="xian-liu"></a>

* `POST /v2/sendBinaryTransaction`

### Request Example <a href="#jiao-yi-gou-jian-dai-ma-shi-li" id="jiao-yi-gou-jian-dai-ma-shi-li"></a>

{% tabs %}
{% tab title="CURL" %}
```javascript
curl --location 'http://frankfurt.solana.blockrazor.xyz:443/v2/sendBinaryTransaction?auth=<auth_token>' \
--data-binary @transaction.bin
```
{% endtab %}

{% tab title="Go" %}
```go
package main
import (
	"bytes"
	"context"
	"fmt"
	"io"
	"math/rand"
	"net/http"
	"time"
	"github.com/gagliardetto/solana-go"
	"github.com/gagliardetto/solana-go/programs/system"
	"github.com/gagliardetto/solana-go/rpc"
)
const (
	httpEndpoint = "http://frankfurt.solana.blockrazor.xyz:443/v2/sendBinaryTransaction?auth=<auth_token>"
	mainNetRPC   = ""
	privateKey   = ""
	publicKey    = ""
	amount       = 200_000
	tipAmount    = 1_000_000
)
var tipAccounts = []string{
	"Gywj98ophM7GmkDdaWs4isqZnDdFCW7B46TXmKfvyqSm",
	"FjmZZrFvhnqqb9ThCuMVnENaM3JGVuGWNyCAxRJcFpg9",
	"6No2i3aawzHsjtThw81iq1EXPJN6rh8eSJCLaYZfKDTG",
	"A9cWowVAiHe9pJfKAj3TJiN9VpbzMUq6E4kEvf5mUT22",
	"68Pwb4jS7eZATjDfhmTXgRJjCiZmw1L7Huy4HNpnxJ3o",
	"4ABhJh5rZPjv63RBJBuyWzBK3g9gWMUQdTZP2kiW31V9",
	"B2M4NG5eyZp5SBQrSdtemzk5TqVuaWGQnowGaCBt8GyM",
	"5jA59cXMKQqZAVdtopv8q3yyw9SYfiE3vUCbt7p8MfVf",
	"5YktoWygr1Bp9wiS1xtMtUki1PeYuuzuCF98tqwYxf61",
	"295Avbam4qGShBYK7E9H5Ldew4B3WyJGmgmXfiWdeeyV",
	"EDi4rSy2LZgKJX74mbLTFk4mxoTgT6F7HxxzG2HBAFyK",
	"BnGKHAC386n4Qmv9xtpBVbRaUTKixjBe3oagkPFKtoy6",
	"Dd7K2Fp7AtoN8xCghKDRmyqr5U169t48Tw5fEd3wT9mq",
	"AP6qExwrbRgBAVaehg4b5xHENX815sMabtBzUzVB4v8S",
}
var httpClient = &http.Client{
	Timeout: 10 * time.Second,
}
func main() {
	if err := sendTx(); err != nil {
		fmt.Printf("send tx failed: %v\n", err)
	}
}
func sendTx() error {
	account, err := solana.WalletFromPrivateKeyBase58(privateKey)
	if err != nil {
		return err
	}
	receivePub := solana.MustPublicKeyFromBase58(publicKey)
	tipPub := solana.MustPublicKeyFromBase58(tipAccounts[rand.Intn(len(tipAccounts))])
	rpcClient := rpc.New(mainNetRPC)
	blockhash, err := rpcClient.GetLatestBlockhash(context.TODO(), rpc.CommitmentFinalized)
	if err != nil {
		return fmt.Errorf("[get blockhash] %v", err)
	}
	transferIx := system.NewTransferInstruction(amount, account.PublicKey(), receivePub).Build()
	tipIx := system.NewTransferInstruction(tipAmount, account.PublicKey(), tipPub).Build()
	tx, err := solana.NewTransaction(
		[]solana.Instruction{tipIx, transferIx},
		blockhash.Value.Blockhash,
		solana.TransactionPayer(account.PublicKey()),
	)
	if err != nil {
		return fmt.Errorf("build tx error: %v", err)
	}
	_, err = tx.Sign(func(key solana.PublicKey) *solana.PrivateKey {
		if account.PublicKey().Equals(key) {
			return &account.PrivateKey
		}
		return nil
	})
	if err != nil {
		return fmt.Errorf("sign tx error: %v", err)
	}
	binTx, err := tx.MarshalBinary()
	if err != nil {
		return fmt.Errorf("marshal tx error: %v", err)
	}
	req, err := http.NewRequest("POST", httpEndpoint, bytes.NewReader(binTx))
	if err != nil {
		return err
	}
	req.Header.Set("Content-Type", "application/octet-stream")
	resp, err := httpClient.Do(req)
	if err != nil {
		return fmt.Errorf("send http error: %v", err)
	}
	defer resp.Body.Close()
	bodyBytes, _ := io.ReadAll(resp.Body)
	fmt.Printf("[send tx] response: %s\n", string(bodyBytes))
	return nil
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use bincode;
use rand::Rng;
use reqwest::header::{HeaderMap, HeaderValue, CONTENT_TYPE};
use solana_client::rpc_client::RpcClient;
use solana_sdk::{
    commitment_config::CommitmentConfig,
    pubkey::Pubkey,
    signature::{Keypair, Signer},
    transaction::Transaction,
};
use solana_system_interface::instruction::transfer;
use std::str::FromStr;
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let http_endpoint = "http://frankfurt.solana.blockrazor.xyz:443/v2/sendBinaryTransaction?auth=<auth_token>";
    let mainnetrpc = "";
    let privatekey = "";
    let publickey = "";
    let amount: u64 = 200_000;
    let tipamount: u64 = 1_000_000;
    let tip_accounts = [
        "Gywj98ophM7GmkDdaWs4isqZnDdFCW7B46TXmKfvyqSm",
        "FjmZZrFvhnqqb9ThCuMVnENaM3JGVuGWNyCAxRJcFpg9",
        "6No2i3aawzHsjtThw81iq1EXPJN6rh8eSJCLaYZfKDTG",
        "A9cWowVAiHe9pJfKAj3TJiN9VpbzMUq6E4kEvf5mUT22",
        "68Pwb4jS7eZATjDfhmTXgRJjCiZmw1L7Huy4HNpnxJ3o",
        "4ABhJh5rZPjv63RBJBuyWzBK3g9gWMUQdTZP2kiW31V9",
        "B2M4NG5eyZp5SBQrSdtemzk5TqVuaWGQnowGaCBt8GyM",
        "5jA59cXMKQqZAVdtopv8q3yyw9SYfiE3vUCbt7p8MfVf",
        "5YktoWygr1Bp9wiS1xtMtUki1PeYuuzuCF98tqwYxf61",
        "295Avbam4qGShBYK7E9H5Ldew4B3WyJGmgmXfiWdeeyV",
        "EDi4rSy2LZgKJX74mbLTFk4mxoTgT6F7HxxzG2HBAFyK",
        "BnGKHAC386n4Qmv9xtpBVbRaUTKixjBe3oagkPFKtoy6",
        "Dd7K2Fp7AtoN8xCghKDRmyqr5U169t48Tw5fEd3wT9mq",
        "AP6qExwrbRgBAVaehg4b5xHENX815sMabtBzUzVB4v8S",
    ];
    let client = reqwest::Client::new();
    send_transaction(
        &client,
        mainnetrpc,
        privatekey,
        publickey,
        &tip_accounts,
        tipamount,
        amount,
        http_endpoint,
    )
    .await?;
    Ok(())
}
async fn send_transaction(
    client: &reqwest::Client,
    mainnetrpc: &str,
    privatekey: &str,
    publickey: &str,
    tip_accounts: &[&str],
    tipamount: u64,
    amount: u64,
    http_endpoint: &str,
) -> Result<(), Box<dyn std::error::Error>> {
    let from = Keypair::from_base58_string(privatekey);
    let receiver = Pubkey::from_str(publickey)?;
    let tip = Pubkey::from_str(tip_accounts[rand::thread_rng().gen_range(0..tip_accounts.len())])?;
    let rpcurl = String::from(mainnetrpc);
    let connection = RpcClient::new_with_commitment(rpcurl, CommitmentConfig::confirmed());
    let recent_blockhash = connection
        .get_latest_blockhash()
        .expect("Failed to get recent blockhash.");
    let ix_tip = transfer(&from.pubkey(), &tip, tipamount);
    let ix_main = transfer(&from.pubkey(), &receiver, amount);
    let tx = Transaction::new_signed_with_payer(
        &[ix_tip, ix_main],
        Some(&from.pubkey()),
        &[&from],
        recent_blockhash,
    );
    let serialized = bincode::serialize(&tx)?;
    let mut headers = HeaderMap::new();
    headers.insert(CONTENT_TYPE, HeaderValue::from_static("application/octet-stream"));
    let res = client
        .post(http_endpoint)
        .headers(headers)
        .body(serialized)
        .send()
        .await?;
    let text = res.text().await?;
    println!("[send tx] response: {}", text);
    Ok(())
}
```
{% endtab %}

{% tab title="JS" %}
```javascript
const axios = require("axios");
const web3 = require("@solana/web3.js");
const bs58 = require("bs58");
const http = require("http");
const https = require("https");
const httpEndpoint = "http://frankfurt.solana.blockrazor.xyz:443/v2/sendBinaryTransaction?auth=<auth_token>";
const mainNetRPC = "";
const privateKey = "";
const publicKey = "";
const amount = 200_000;
const tipAmount = 1_000_000;
const tipAccounts = [
  "Gywj98ophM7GmkDdaWs4isqZnDdFCW7B46TXmKfvyqSm",
  "FjmZZrFvhnqqb9ThCuMVnENaM3JGVuGWNyCAxRJcFpg9",
  "6No2i3aawzHsjtThw81iq1EXPJN6rh8eSJCLaYZfKDTG",
  "A9cWowVAiHe9pJfKAj3TJiN9VpbzMUq6E4kEvf5mUT22",
  "68Pwb4jS7eZATjDfhmTXgRJjCiZmw1L7Huy4HNpnxJ3o",
  "4ABhJh5rZPjv63RBJBuyWzBK3g9gWMUQdTZP2kiW31V9",
  "B2M4NG5eyZp5SBQrSdtemzk5TqVuaWGQnowGaCBt8GyM",
  "5jA59cXMKQqZAVdtopv8q3yyw9SYfiE3vUCbt7p8MfVf",
  "5YktoWygr1Bp9wiS1xtMtUki1PeYuuzuCF98tqwYxf61",
  "295Avbam4qGShBYK7E9H5Ldew4B3WyJGmgmXfiWdeeyV",
  "EDi4rSy2LZgKJX74mbLTFk4mxoTgT6F7HxxzG2HBAFyK",
  "BnGKHAC386n4Qmv9xtpBVbRaUTKixjBe3oagkPFKtoy6",
  "Dd7K2Fp7AtoN8xCghKDRmyqr5U169t48Tw5fEd3wT9mq",
  "AP6qExwrbRgBAVaehg4b5xHENX815sMabtBzUzVB4v8S",
];
const httpClient = axios.create({
  timeout: 10000,
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),
});
async function sendTx() {
  const senderPrivateKey = new Uint8Array(bs58.decode(privateKey));
  const senderKeypair = web3.Keypair.fromSecretKey(senderPrivateKey);
  const receiver = new web3.PublicKey(publicKey);
  const tipAccount = new web3.PublicKey(
    tipAccounts[Math.floor(Math.random() * tipAccounts.length)]
  );
  const connection = new web3.Connection(mainNetRPC);
  const { blockhash } = await connection.getLatestBlockhash("finalized");
  const tx = new web3.Transaction()
    .add(
      web3.SystemProgram.transfer({
        fromPubkey: senderKeypair.publicKey,
        toPubkey: tipAccount,
        lamports: tipAmount,
      })
    )
    .add(
      web3.SystemProgram.transfer({
        fromPubkey: senderKeypair.publicKey,
        toPubkey: receiver,
        lamports: amount,
      })
    );
  tx.recentBlockhash = blockhash;
  tx.feePayer = senderKeypair.publicKey;
  tx.sign(senderKeypair);
  const serialized = tx.serialize();
  try {
    const res = await httpClient.post(httpEndpoint, Buffer.from(serialized), {
      headers: {
        "Content-Type": "application/octet-stream",
      },
      responseType: "text",
    });
    console.log("[send tx] response:", res.data);
  } catch (err) {
    console.error("SendTx failed:", err.response?.data || err.message);
  }
}
sendTx().catch(console.error);
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note:**

* **the `auth` and `request parameter`  are compulsory to be added in URI params, e.g.,** `http://frankfurt.solana.blockrazor.xyz:443/v2/sendBinaryTransaction?auth=<auth_token>&mode=fast&revertProtection=true`
{% endhint %}

### Request Parameter

<table><thead><tr><th width="111.44921875">Parameters</th><th width="115.421875">Mandatory</th><th width="109.98046875">Example</th><th>Description</th></tr></thead><tbody><tr><td>transaction</td><td>Mandatory</td><td>transaction.bin</td><td>Fully signed transactions, binary format</td></tr><tr><td>mode</td><td>Optional</td><td>"fast"<br>"sandwichMitigation"</td><td>BlockRazor offers two modes: Fast and SandwichMitigation, with Fast as the default.<br><br>In fast mode, transactions are sent based on globally distributed high-performance network and high-quality SWQoS, reaching the Leader node with the lowest latency.<br><br>In sandwichMitigation mode, BlockRazoz will route transactions to the trusted SWQoS and skip the slot of the blacklisted Leader (dynamically identified by the BlockRazor sandwich monitoring mechanism). In this mode, <strong>DO NOT</strong> send transactions using durable nonce, as it will cause the sandwich protection to become ineffective.</td></tr><tr><td>safeWindow</td><td>Optional</td><td>3</td><td>safeWindow is used to determine the timing of transaction sending in sandwichMitigation mode and represents the number of consecutive slots of  whitelist validators. For example, if it is set to 3, the transaction will only be sent when 3 consecutive slots from the current slot belong to whitelist validators.<br><br>The range of safeWindow is 3-13. The larger the number, the better the effect of mitigating the sandwich attack, but it may have a certain impact on the rate of inclusion. If not set, the default is 3.</td></tr><tr><td>revertProtection</td><td>Optional</td><td>false</td><td>The default value is false. If set to true, the transaction will not fail on chain, but the speed of inclusion will be affected and there is a possibility that it cannot be included. Please choose to enable it carefully according to actual needs.</td></tr></tbody></table>

### Response

<table><thead><tr><th width="141.20703125">Status Code</th><th width="201.421875">Message</th><th>Meaning</th></tr></thead><tbody><tr><td>200</td><td>OK</td><td>The request is normal</td></tr><tr><td>400</td><td>BadRequest</td><td>Invalid parameter</td></tr><tr><td>403</td><td>Forbidden</td><td>Request denied, as the authentication (auth) is empty, invalid, or expired.</td></tr><tr><td>500</td><td>InternalServerError</td><td>The server encountered an unexpected condition that prevented it from fulfilling the request</td></tr></tbody></table>
