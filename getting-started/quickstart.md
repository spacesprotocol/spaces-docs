---
description: Setup spaces on mainnet
---

# Quickstart

### Install Bitcoin Core

It can be installed from the official [download page](https://bitcoincore.org/en/download/).

Configure RPC authentication in `bitcoin.conf`:

```
rpcuser=<your-username>
rpcpassword=<your-password>
```

### Install Spaces Daemon

`spaces` is a tiny layer that connects to Bitcoin Core over RPC and scans transactions relevant to the protocol.&#x20;

1. Download the latest binary from [releases](https://github.com/spacesprotocol/spaces/releases) (must be v0.0.4 or higher for mainnet)
2. Verify installation

```
spaced --version
space-cli --version
```

### Run Spaces

1. Ensure Bitcoin Core is fully synced
2. Start Spaces daemon

```sh
spaced --bitcoin-rpc-user <your-rpc-user> --bitcoin-rpc-password <your-rpc-password>
```

### Create a wallet

Create the default wallet and get an address to receive coins

```sh
space-cli createwallet
space-cli getnewaddress
```

Send some BTC to the address you get and check your balance

```
space-cli balance
```

### Auction process <a href="#opening-an-auction" id="opening-an-auction"></a>

In short, top level spaces are community identifiers limited to \~3600 spaces a year. Every day, the top 10 highest-bid spaces advance from pre-auctions to auctions [learn more](understanding-auctions.md).

### Opening an auction <a href="#opening-an-auction" id="opening-an-auction"></a>

You can check the [explorer](https://explorer.spacesprotocol.org) for currently open auctions . For example to open an auction for the space `@bitcion`

```bash
space-cli open bitcoin
```

You will get a similar output to this

<pre class="language-json"><code class="lang-json">[
  {
    "txid": "<a data-footnote-ref href="#user-content-fn-1">1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc</a>",
    "tags": ["bidouts", "commitment"]
  },
  {
    "txid": "<a data-footnote-ref href="#user-content-fn-2">792a2fd221e4715d6c4f330fb46309d6f6a7ed4fd0f9c50471b77e643f9885d2</a>",
    "tags": ["open"]
  }
]
</code></pre>

### Placing a bid <a href="#placing-a-bid" id="placing-a-bid"></a>

Find one of the spaces [currently in auction](https://explorer.spacesprotocol.org/) and place a bid (amount is in sats)

```bash
space-cli bid nostr 1500
```

### Check status of a space <a href="#placing-a-bid" id="placing-a-bid"></a>

```bash
space-cli getspace btc
```

You will get something like this

<pre class="language-json"><code class="lang-json">{
  "outpoint": "<a data-footnote-ref href="#user-content-fn-3">1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc:1</a>",
  "value": 662,
  "script_pubkey": "51202a7267b047254ad41e87458b902c286434e3764ffd2f2fdb46a9c8fafa6135e3",
  "name": "@btc",
  "covenant": {
    "type": "bid",
    "burn_increment": 1000,
    "signature": "........",
    "total_burned": 1000,
    "claim_height": null
  }
}
</code></pre>

You can use `listspaces` command to see all space outputs you own including outputs that are actively in auction.

The `bid` covenant indicates spending this output requires either another bid spend or a registration spend if the claim height is reached [learn more](../deep-dive/auctions.md#covenants).

### Claiming a space <a href="#placing-a-bid" id="placing-a-bid"></a>

If you're currently winning and the space entered the claim period, you can register it!

```sh
space-cli register bitcoin
```

You will get something like this

<pre class="language-json"><code class="lang-json">{
  "outpoint": "<a data-footnote-ref href="#user-content-fn-4">b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc:1</a>",
  "value": 662,
  "script_pubkey": "5120882cd5b0ef333be2efd7efd9bea0953f894677f1d5a638c8a3d13734e99d22e6",
  "name": "@bitcoin",
  "covenant": {
    "type": "transfer",
    "expire_height": 93050,
    "data": null,
  }
}
</code></pre>

You may also watch the status of auctions on the [explorer](https://explorer.spacesprotocol.org)

[^1]: Checkout this transaction here[https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5ad](https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc)

[^2]: Checkout this transaction here

    [https://mempool.space/testnet4/tx/792a2fd221e4715d6c4f330fb46309d6f6a7ed4fd0f9c50471b77e643f9885d2](https://mempool.space/testnet4/tx/792a2fd221e4715d6c4f330fb46309d6f6a7ed4fd0f9c50471b77e643f9885d2)

[^3]: Checkout this outpoint here\
    [https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc#vout=](https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc#vout=1)

[^4]: Check this outpoint here

    [https://mempool.space/testnet4/tx/b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc#vout=1](https://mempool.space/testnet4/tx/b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc#vout=1)
