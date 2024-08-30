# Tracking UTXOs

The Spaces protocol tracks relevant Unspent Transaction Outputs (UTXOs) instead of monitoring the entire Bitcoin UTXO set. This approach allows clients like [spaced](https://github.com/buffrr/spaces-docs/blob/main/deep-dive/broken-reference/README.md) to operate without relying on the full Bitcoin UTXO set database, enabling them to sync using Simplified Payment Verification (SPV). This selective tracking also enables faster sync since the protocol does not need to check the witness of every input in every transaction looking for [space script](interactive-blocks.md)[s](interactive-blocks.md). Additionally, it enables building a smaller more lightweight version of [Utreexo](https://eprint.iacr.org/2019/611) for the zk-light clients.

The protocol automatically tracks any space outputs. However, for transactions creating [auctioned outputs](markdown.md) and/or committing to P2TR [space scripts](interactive-blocks.md), you need to indicate that such transactions are creating outputs relevant to the protocol:

* The transaction must use a special locktime ending with `222`
* Within such transactions, any outputs with a value ending in `2` are tracked.

While this may occasionally result in false positives, these are removed from the tracked set once they're spent.
