# Making Bids

A bid transaction carries a Bid PSBT and consumes a previous Bid PSBT (if any)

| Inputs/Outputs                               | Notes                              |
| -------------------------------------------- | ---------------------------------- |
| Carried bid psbt input/output pair           | Offers a new UTXO for the next bid |
| Re-constructed refund psbt Input/Output pair | Spends auctioned UTXO              |

In scenarios where multiple bids are placed simultaneously on the same auctioned space output from the re-constructed PSBT, only one bid will succeed in burning the coins.

**Open Transactions**

{% hint style="warning" %}
When opening an auction for a space, avoid placing large initial bids. If two auctions for the same space are opened simultaneously, only the first to be included in a block will establish the auction process for that space. To mitigate potential losses, the Spaces wallet currently defaults to 1000 sats for initial bids.
{% endhint %}

An open transaction is exactly the same as a bid transaction except it includes a [space script](interactive-blocks.md) which reveals the name in the witness.

```
<sname>
OP_OPEN
```
