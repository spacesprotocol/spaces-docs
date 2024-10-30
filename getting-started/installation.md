---
description: You can try the Bitcoin Spaces protocol on testnet4 🎉
---

# Installation

### Install Bitcoin Core
Bitcoin Core of version 28+ is required. It can be installed from the official [download page](https://bitcoincore.org/en/download/).

### Install Spaces Daemon

`spaced` is a tiny layer that connects to Bitcoin Core over RPC and scans transactions relevant to the protocol. Make sure you have [Rust](https://www.rust-lang.org/tools/install) installed before proceeding.

```sh
git clone https://github.com/spacesprotocol/spaced && cd spaced
cargo install --path node --locked
```

Make sure it's in your path

```sh
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify installation

```
spaced --version
space-cli --version
```

### Configure & Sync Bitcoin Core

```sh
mkdir $HOME/bitcoin-testnet4

# Create a configuration file with RPC credentials
echo "rpcuser=testnet4" > $HOME/bitcoin-testnet4/bitcoin.conf
echo "rpcpassword=testnet4" >> $HOME/bitcoin-testnet4/bitcoin.conf

# Start Bitcoin Core specifying testnet4 network
bitcoind -testnet4 -datadir=$HOME/bitcoin-testnet4
```

### Run Spaces

```sh
spaced --chain testnet4 --bitcoin-rpc-user testnet4 --bitcoin-rpc-password testnet4
```

Hooray installation is done! 🎉
