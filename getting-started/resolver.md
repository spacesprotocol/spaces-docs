---
cover: ../.gitbook/assets/Fabric bg (1).svg
coverY: 0
---

# Name resolution

The Spaces protocol functions primarily as a Bitcoin certificate authority, minimizing on-chain footprint. Its trustless nature makes it an ideal foundation for Peer-to-Peer (P2P) protocols that leverage spaces as trust anchors, enabling off-chain record storage and other use cases.&#x20;

### Fabric

[Fabric](https://github.com/spacesprotocol/fabric) is a trustless, distributed DNS resolver for spaces that enables Bitcoin-signed zone file publication on a permissionless DHT without on-chain storage. You can run a Fabric node to host records or pay others to "pin" your signed zone file.\


### Bitcoin Wallet Naming

The Spaces protocol currently offers experimental support for resolving spaces to payment addresses. You can experiment with this feature using the spaces wallet:

```
space-cli send 1000 --to @bob
```

This command sends 1000 satoshis to the owner of the Space `@bob`.

### Upcoming changes

We're in the process of formalizing the format for space records checkout [SIP-02.](https://github.com/spacesprotocol/sips/blob/main/sip-0002.mediawiki) You will be able to resolve spaces to [silent payment addresses](https://silentpayments.xyz/), [Nostr](https://nostr.com), [onion services](https://community.torproject.org/onion-services/) and more.

Join us on [Telegram](https://t.me/spacesprotocol) to contribute or discuss ideas and stay up to date!
