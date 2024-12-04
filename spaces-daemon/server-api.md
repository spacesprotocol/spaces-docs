# Server API

The daemon implements [JSON RPC](https://www.jsonrpc.org/specification), so you can use any RPC client to communicate with it or write your own. A basic request over HTTP would look like this (using [testnet4 ](configuration.md)endpoint)

<mark style="color:green;">`POST`</mark> `http://127.0.0.1:7224`

```json
{
    "jsonrpc": "2.0", 
    "method": "<methodname>", 
    "params": [ .. ],
    "id": 1
}
```

## Get Server Info

<mark style="color:green;">`getserverinfo`</mark> retrieve the current state of the spaces daemon

**Params** None

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getserverinfo
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getserverinfo","params":[],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
"chain": "testnet4",
"tip": {
  "hash": "0000000000000040297226f7046d72b63e159f6814009b9d9155331fd0ddec61",
  "height": 41730
}
```

## Get Space

<mark style="color:green;">`getspace`</mark> retrieve information about a space:

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>space</code></td><td>string</td><td>Canonical space name e.g. @bitcoin</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getspace @bitcoin
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getspace","params":["@bitcoin"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

{% tabs %}
{% tab title="Pre-auction" %}
<pre class="language-json"><code class="lang-json">
{
  "txid": "<a data-footnote-ref href="#user-content-fn-1">83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994</a>",
  "n": 1,
  "name": "@btc",
  "covenant": {
    "type": "bid",
    "burn_increment": 1000,
    "signature": "........",
    "total_burned": 1000,
    "claim_height": null
  },
  "value": 662,
  "script_pubkey": "512027453c85a747cdc411f3b27e1cc475c299f3bb18498316b7c56a977bb4e6b2dd"
}
</code></pre>
{% endtab %}

{% tab title="In Auction" %}
```json
{
  "txid": "ffe0d3a0d9f925efd486d3a3744e8968b41e64100249f55f6c3a85b8bfdcc8c1",
  "n": 51,
  "name": "@axis",
  "covenant": {
    "type": "bid",
    "burn_increment": 1400,
    "signature": "........",
    "total_burned": 1400,
    "claim_height": 56305
  },
  "value": 662,
  "script_pubkey": "512034c032c74d323646bb63930fd8a6b305957e2b46af207362f5d652fdc9dc2115"
}
```
{% endtab %}

{% tab title="Claimed" %}
```json
{
  "txid": "f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a",
  "n": 1,
  "name": "@bitcoin",
  "covenant": {
    "type": "transfer",
    "expire_height": 108439,
    "data": "68656c6c6f20776f726c64"
  },
  "value": 666,
  "script_pubkey": "5120611454515f9fe8c656f80c51de229dc5d9cba9a05d00486b43a1e9033ef6393b"
}
```
{% endtab %}
{% endtabs %}

## Get Space Owner

<mark style="color:green;">`getspaceowner`</mark> retrieves the outpoint of a space:

**Params**

| Name    | Type   | Description                        |
| ------- | ------ | ---------------------------------- |
| `space` | string | Canonical space name e.g. @bitcoin |

{% tabs %}
{% tab title="CLI" %}
```
only available via JSON-RPC
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getspaceowner","params":["@bitcoin"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response** Responds with an outpoint with `txid:vout` format.

```json
f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a:1
```

## Get Spaceout

<mark style="color:green;">`getspaceout`</mark> retrieves a _spaceout_ which are any UTXOs tracked by the spaces protocol not necessarily ones with a space.

**Params**

<table><thead><tr><th width="137">Name</th><th width="87">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>outpoint</code></td><td>string</td><td><p>An Outpoint which is a specific output within a transaction using <code>txid:vout</code> format e.g.</p><pre class="language-json"><code class="lang-json">f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a:1
</code></pre></td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getspaceout "f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a:1"
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getspaceout","params":["f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a:1"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

{% tabs %}
{% tab title="Space UTXO" %}
```json
{
  "n": 1,
  "name": "@bitcoin",
  "covenant": {
    "type": "transfer",
    "expire_height": 108439,
    "data": "68656c6c6f20776f726c64"
  },
  "value": 666,
  "script_pubkey": "5120611454515f9fe8c656f80c51de229dc5d9cba9a05d00486b43a1e9033ef6393b"
}
```
{% endtab %}

{% tab title="Other UTXOs" %}
```json
{
   "value": 662,
   "script_pubkey": "5120882cd5b0ef333be2efd7efd9bea0953f894677f1d5a638c8a3d13734e99d22e6"
}
```
{% endtab %}
{% endtabs %}

## Estimate bid

<mark style="color:green;">`estimatebid`</mark> estimates the required bid to make it into the [auctions phase](../getting-started/auctions.md#auctions) within the target block:

**Params**

<table><thead><tr><th width="208">Name</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>target</code></td><td>number</td><td>The target interval e.g. specify 0 for the coming rollout, 1 for the day after and so on.</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 estimatebid 0
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"estimatebid","params":[0],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response** Responds with an amount in Satoshis

```json
2000
```

## Get Rollout

<mark style="color:green;">`getrollout`</mark> get spaces rolling out into auctions for the given interval (in 144 block increments):

**Params**

<table><thead><tr><th width="208">Name</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>target</code></td><td>number</td><td>The target interval e.g. specify 0 for the coming rollout, 1 for the day after and so on.</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getrollout 0
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getrollout","params":[0],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response** An array of spaces expected to be in auctions within the given `target`

```json
[
  [
    "@bitcoin",
    1000
  ],
  [
    "@mytestspace",
    1000
  ],
```

## Get Block Meta

<mark style="color:green;">`getblockmeta`</mark> Retrieves all transactions relevant to spaces for the given block (requires [block indexing](configuration.md#options) to be enabled)

**Params**

| Name         | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| `block_hash` | string | The block hash as hex string |

{% tabs %}
{% tab title="CLI" %}
```
only available via JSON-RPC
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getblockmeta","params":["000000000157a35e916d9b6bfad6aee098ba7f0960fc0fe27c7f3e17bccec289"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "height": 56558,
  "tx_meta": [
    {
      "txid": "83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994",
      "spends": [],
      "creates": [
        {
          "n": 1,
          "value": 662,
          "script_pubkey": "512027453c85a747cdc411f3b27e1cc475c299f3bb18498316b7c56a977bb4e6b2dd"
        },
        {
          "n": 2,
          "value": 662,
          "script_pubkey": "5120c75ee481697d60b51e899875d9ba1308090dc8d45d786cfe7188bc3918da8a81"
        }
      ],
      "updates": []
    },
    {
      "txid": "d2c4179772bcde20e78fe4762d17d2192f771d1edfc467b68e4e56c4dd1e8deb",
      "spends": [
        {
          "n": 1
        }
      ],
      "creates": [],
      "updates": [
        {
          "type": "bid",
          "output": {
            "txid": "83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994",
            "n": 1,
            "name": "@btc",
            "covenant": {
              "type": "bid",
              "burn_increment": 1000,
              "signature": "05cd995322bbaa6a8d7d95b1a8e68c3c9fb68c679becfe7541ef6c5b7d4f97662091b8e48cc687845fa237ef6b2ac66bb8c2c2fdb7f92e0c608cbfb61e4308eb",
              "total_burned": 1000,
              "claim_height": null
            },
            "value": 662,
            "script_pubkey": "512027453c85a747cdc411f3b27e1cc475c299f3bb18498316b7c56a977bb4e6b2dd"
          }
        }
      ]
    }
  ]
}
```

[^1]: Checkout this outpoint here\
    [https://mempool.space/testnet4/tx/83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994](https://mempool.space/testnet4/tx/83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994)
