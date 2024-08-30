---
description: You can try the Bitcoin Spaces protocol on testnet4 🎉
---

# Installation

### Install Bitcoin Core

{% hint style="info" %}
Bitcoin Core version 28+ is required for testnet4 and it's not yet released at the time of this writing. Check the [website](https://bitcoin.org/en/bitcoin-core/) to see if it's released otherwise follow instructions below to compile from source.
{% endhint %}

The following instructions is for compiling Bitcoin core from source to access `testnet4`

{% tabs %}
{% tab title="MacOS" %}
Setup environment xcode & brew

```sh
xcode-select --install

brew install automake libtool boost pkg-config libevent llvm
```

Compile Bitcoin core

```sh
git clone https://github.com/bitcoin/bitcoin.git && cd bitcoin
./autogen.sh
CC=$(brew --prefix llvm)/bin/clang CXX=$(brew --prefix llvm)/bin/clang++ ./configure --with-gui=no
make
make install # optional
```

If you need further help, check the main [guide](https://github.com/bitcoin/bitcoin/blob/master/doc/build-osx.md)
{% endtab %}

{% tab title="Linux" %}
### Ubuntu/debian

Install dependencies&#x20;

```sh
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3 libevent-dev libboost-dev  libsqlite3-dev
```

Compile

```sh
git clone https://github.com/bitcoin/bitcoin.git && cd bitcoin
./autogen.sh
./configure
make # use "-j N" for N parallel jobs
make install # optional
```

If you need further help or use a different distro, check the main [guide](https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md)
{% endtab %}
{% endtabs %}

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

Hooray installation is done! 🎉&#x20;
