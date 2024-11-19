---
icon: hand-wave
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome

_Spaces_ are community identifiers; think of them as top-level domains such as ".com", ".org", and ".net". Every _Space_ is represented as a [Bitcoin UTXO](https://mirror.xyz/0xaFaBa30769374EA0F971300dE79c62Bf94B464d5/Yetu-6pZkbQCOpsBxswn\_7dGUZDxoBU8NrOQIZScwpg) (Unspent Transaction Output), and can operate millions of decentralized identities underneath called _subspaces_. There are only about 3600 community spaces released per year, acquired through permissionless auctions on the Bitcoin blockchain.

<figure><picture><source srcset=".gitbook/assets/handles-dark.png" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/handles-light.png" alt=""></picture><figcaption><p>alice, bob and mike are subspaces or identities within the communities @nostr, @bitcoin and @x</p></figcaption></figure>

## What makes spaces special?

Unlike _Bitcoin-themed_ naming systems such as ones based on RSK and STX, Spaces are Bitcoin UTXOs secured directly by the Bitcoin blockchain and have the following properties:

* Spaces are community identifiers, capped at \~3600 per year, acquired through permissionless auctions on the Bitcoin blockchain.
* Each _Space_ is a community that may operate a registry for issuing Bitcoin identities known as _subspaces_.
* Spaces AND subspaces (once allocated) are permissionless and as secure as Bitcoin itself! No bridges, slashing, or optimistic security.
* The spaces protocol is designed with zk-light clients in mind making these identities verifiable on resource constrained devices.
* Subspaces do not need to trust the community operator once allocated, and they can do 100% of their transactions on-chain if they wish.
* Minimal on-chain footprint.

## Spaces vs. ENS

For full verification, you can sync the entire spaces protocol with a pruned Bitcoin full node on an old Raspberry PI. Bitcoin is the most secure Proof-of-Work blockchain today, and it's relatively lightweight compared to running an Ethereum node. The majority of ENS integrations trust centralized third-parties defeating the whole purpose of decentralized identities.

## Jump right in

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Getting Started</strong></td><td>Download and install spaces</td><td><a href=".gitbook/assets/coversgetstarted.png">coversgetstarted.png</a></td><td></td><td><a href="getting-started/quickstart.md">quickstart.md</a></td></tr><tr><td><strong>Deep Dive</strong></td><td>Understand how the protocol works</td><td><a href=".gitbook/assets/coversdeepdive.png">coversdeepdive.png</a></td><td></td><td><a href="deep-dive/auctions.md">auctions.md</a></td></tr><tr><td><strong>Explorer</strong></td><td>Checkout the explorer</td><td><a href=".gitbook/assets/coversexplorer.png">coversexplorer.png</a></td><td></td><td><a href="https://explorer.spacesprotocol.org">https://explorer.spacesprotocol.org</a></td></tr></tbody></table>
