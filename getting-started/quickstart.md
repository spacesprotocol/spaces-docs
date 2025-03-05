---
description: Setup spaces on mainnet
cover: ../.gitbook/assets/Fabric bg (1).svg
coverY: 0
---

# Quick start

### Install Bitcoin Core

1. Install Bitcoin Core from [bitcoin.org/en/download](https://bitcoin.org/en/download).
2. Edit `bitcoin.conf` in `~/Library/Application Support/Bitcoin/` (macOS) or `~/.bitcoin/` (Linux).
3. Add these lines to `bitcoin.conf`to configure RPC auth:

```
rpcuser=<your-username>
rpcpassword=<your-password>
```

### Install Spaces

`spaced` is a tiny layer that connects to Bitcoin Core over RPC and scans transactions relevant to the protocol.&#x20;

1. Install the latest version:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://install.spacesprotocol.org | sh
```

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

In short, top level spaces are community identifiers limited to \~3600 spaces a year. Every day, the top 10 highest-bid spaces advance from pre-auctions to auctions [learn more](auctions.md).

### Opening an auction <a href="#opening-an-auction" id="opening-an-auction"></a>

You can check the [explorer](https://explorer.spacesprotocol.org) for currently open auctions . For example to open an auction for the space `@bitcion`

```bash
space-cli open bitcoin
```

### Placing a bid <a href="#placing-a-bid" id="placing-a-bid"></a>

Find one of the spaces [currently in auction](https://explorer.spacesprotocol.org/) and place a bid (amount is in sats)

```bash
space-cli bid nostr 1500
```

### Check status of your Spaces <a href="#placing-a-bid" id="placing-a-bid"></a>

```
space-cli listspaces
```

This will list all spaces you own and spaces currently in auction.

### Claiming a space <a href="#placing-a-bid" id="placing-a-bid"></a>

If you're currently winning and the space entered the claim period, you can register it!

```sh
space-cli register bitcoin
```

You may also watch the status of auctions on the [explorer](https://explorer.spacesprotocol.org)

### Publish records <a href="#placing-a-bid" id="placing-a-bid"></a>

Once you register a space, you can publish records over the [Fabric DHT](fabric-dht.md) without adding any on-chain bloat!
