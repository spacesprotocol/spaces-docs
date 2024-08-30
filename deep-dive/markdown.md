# Bid PSBT

{% hint style="info" %}
If you would like to manually construct bid transactions, you can use the Spaces Wallet CLI to get the bid contract signature for the auctioned space output, total burned and other details:\
\
$ `space-cli getspace myspacename`
{% endhint %}

The bid PSBT spends an _auctioned_ space output and refunds the previous bidder's burned coins. Bid PSBTs must follow the exact structure defined here otherwise it would not be possible to re-construct it from the compressed form:

| Field         | Value |
| ------------- | ----- |
| **Version**   | 2     |
| **Lock Time** | 0     |

**Input**

| Field           | Value                  |
| --------------- | ---------------------- |
| Previous Output | `prev_txid:prev_index` |
| Sequence        | `0xFFFFFFFD`           |
| Sighash type    | `SINGLE\|ANYONECANPAY` |

**Output**

| Field         | Value                       |
| ------------- | --------------------------- |
| Script Pubkey | `prev_script_pubkey`        |
| Value         | `total_burned + prev_value` |

Where:

* `prev_txid:prev_index` refers to the transaction ID and output index of the previous auctioned space UTXO.
* `total_burned` is the total amount of coins burned during the auction until this point.
* `prev_value` is the value of the previous output (`prev_txid:prev_index`)
* `prev_script_pubkey` the exact p2tr script pubkey of (`prev_txid:prev_index`)



**Compressing Bid PSBT to 65 bytes**



<figure><picture><source srcset="../.gitbook/assets/cpsbt-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/cpsbt-light.png" alt=""></picture><figcaption><p><br>In this example, the Space UTXO would be identified with the outpoint <code>#f9395e:6</code></p></figcaption></figure>



To fit the PSBT into a single OP\_RETURN (80-byte limit), we:

1. Omit known details (tx version, input sequence, lock time).
2. Use transactions with at least two unspent outputs for auctions (these outputs must be [tracked](tracking-utxos.md) by the protocol).
3. Encode information in this format:

| **Inputs**                              | **Outputs**                                                                                                                |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Previous Output: `prev_txid:prev_index` | <p>Script Pubkey: <code>OP_RETURN OP_PUSHBYTES_65 prev_index_2:64-byte sig</code><br>Value: <code>bid_increment</code></p> |

Where:

* `prev_txid`: Transaction ID containing the UTXO to be auctioned
* `prev_index`: Any output index from `prev_txid` (references the transaction)
* `prev_index_2`: Index of the output to be auctioned (1 byte)
* `64-byte sig`: Schnorr signature
* `bid_increment`: The current bid increment which equals the bid amount minus the total burned in prior bids and must be greater than zero

Total size: 65 bytes (1 byte + 64 bytes), fitting within the OP\_RETURN limit.



