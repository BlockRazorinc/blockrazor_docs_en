---
description: Explore Priority Fee & Tips for BlockRazor Solana Transaction Sending Mode.
metaLinks:
  canonical: priority-fee-and-tip.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/transaction-submission/transaction-sending/solana/priority-fee-and-tip
---

# Solana Transaction Sending Priority Fee & Tip

### **Priority** Fee <a href="#priority-fee-and-tip" id="priority-fee-and-tip"></a>

Priority Fee is an additional transaction fee charged by Solana on top of Base Fee (the minimum cost of sending a transaction, 5,000 lamports for each signature included in the transaction). Due to limited computing resources, Leader nodes order transactions mainly by transaction value when producing blocks. Transactions with higher Priority Fee have a higher probability of being included in the next block. The CU Price of Priority Fee is provided by [`getTransactionfee`](../../../streams/network-fee-stream/solana/get-transactionfee.md) and is recommended to be set at least 1,000,000 when conscructing transactions.

### Tip <a href="#priority-fee-and-tip" id="priority-fee-and-tip"></a>

When constructing a transaction, you need to add a instruction of Tip transfer into the transaction(preferably added at the front position) to further speed up the inclusion. BlockRazor does not charge service fees from Tips. The Tip transfer amount is at least 100,000 Lamports (0.0001 Sol) . It is recommended to set it to the value returned by [`getTransactionfee`](../../../streams/network-fee-stream/solana/get-transactionfee.md). The account to receive Tip is:

```
"GipkevdwsTrwf2PoGJZqcXXtjEP31DQbqM18QrFe5BDX",
"2TcMjZhDP2pjfYtBc6ZCrPEqvWYKZufptodQYMzhxxoG",
"FAYtM5hk3Dhon7ijDLbkSSsUqvxzhHCKoVCyHrn1Qtjf",
"DZVgBnByLL9MrR2fvZv1TdzXAUvED8EWaGPt3oehWKnm",
"Bym6AE2pEyH5mguycx3ja7ogMcByb9LYkDcHyrPDDzG5",
"Dxrxedq7FrDPfVU7fUADvSQADuzawsquYGmSrTafRRfH",
"4xmdnDV5xA59BydbUFPCBApTLZHn2L8vn6JRJHPYkWNp",
"8uCEKeEYEcrUkH9gKvRSc3ZjkJGsBtM5FGwZDU9pxtc",
"5mBQQcTxNwBgRuX4cab21hjrB116KvWLWinesEZTzy15",
"CSocujMUGBZ2Wb67FjB3VLFtdBTnnPEVmLmDD3FQbmHU",
"8EpzWhsr7uyZkfVD7MTHoWqupM14xngzMeUcCkWbY1UG",
"56XRik27PddNnR7JkHKqd5pWdMHeqje7TCm1gsdmvnTP",
"2DJNtR1tT7vfub7sDJ6v9NGmrzGVFUer5FvjhEcWEpvv",
"3AveZTS3PJo5qKEqHJci5bjW8neufriirr9mJXfsv4iH",
"9DC798ezxU2f2i8ggQ4vsqrsNxdDWJWAFg4cT1xLDR1X"
```

You can also retrieve the tip addresses dynamically to ensure your integration stays up to date if they change.

```bash
curl http://frankfurt.solana.blockrazor.xyz:443/getTipAccounts
```

{% hint style="info" %}
To avoid the degradation of performance due to address occupation, causing transaction delay, please rotate the Tip account when sending transactions.
{% endhint %}
