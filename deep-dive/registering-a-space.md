# Registering a Space

{% hint style="warning" %}
Attempting to register a space before the claim period will invalidate the auction causing the current holder to lose their bid without successfully registering the space.
{% endhint %}

Once a space enters the claim period, the current space output holder i.e the winner can safely register it. It's important to note that auctions remain open indefinitely until the winner claims. The registration process itself is straightforward: a register transaction is identical to a [transfer](moving-spaces.md) and requires no additional metadata.

### How does the protocol distinguish bid spends from claim spends?

Since bidding is still possible in the claim period, the protocol determines if a spend of a space output is a bid spend if the following conditions are met:

* The transaction version, lock time, input sequence for the space output spend, ... etc match the [Bid PSBT](bid-psbt.md) format.
* It's a P2TR key spend with a signature matching exactly the one carried out in the PSBT.

In all other cases it will be interpreted as a claim.
