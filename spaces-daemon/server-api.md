# Server API

The daemon implements [JSON RPC](https://www.jsonrpc.org/specification), so you can use any RPC client to communicate with it or write your own. A basic request over HTTP would look like this (using [testnet4 ](configuration.md)endpoint)

<mark style="color:green;">`POST`</mark> `http://127.0.0.1:7225`

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
space-cli getserverinfo
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getserverinfo","params":[],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "chain": "mainnet",
  "tip": {
    "hash": "0000000000000000000210254eedf4507d3df7bf258ed4bcedfcbb8d02df58c6",
    "height": 883338
  }
}
```

## Get Space

<mark style="color:green;">`getspace`</mark> retrieve information about a space:

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>space</code></td><td>string</td><td>Canonical space name e.g. @bitcoin</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli getspace @bitcoin
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
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
  "txid": "83e11748b0eb15b4ee26f35d3bffe13d35d66d055de4a42a41a06d646879f994",
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
  "script_pubkey": "........"
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
  "script_pubkey": "........"
}
```
{% endtab %}

{% tab title="Claimed" %}
```json
{
  "txid": "3ac9772d3451bd0b2a7cfa09c3eecebdbfb54bbdf90f322e7e7fa6dee3bc0def",
  "n": 1,
  "name": "@bitcoin",
  "covenant": {
    "type": "transfer",
    "expire_height": 925977,
    "data": null
  },
  "value": 666,
  "script_pubkey": "........"
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
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getspaceowner","params":["@bitcoin"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response** Responds with an outpoint with `txid:vout` format.

```json
"3ac9772d3451bd0b2a7cfa09c3eecebdbfb54bbdf90f322e7e7fa6dee3bc0def:1"
```

## Get Spaceout

<mark style="color:green;">`getspaceout`</mark> retrieves a _spaceout_ which are any UTXOs tracked by the spaces protocol not necessarily ones with a space.

**Params**

<table><thead><tr><th width="137">Name</th><th width="87">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>outpoint</code></td><td>string</td><td><p>An Outpoint which is a specific output within a transaction using <code>txid:vout</code> format e.g.</p><pre class="language-json"><code class="lang-json">3ac9772d3451bd0b2a7cfa09c3eecebdbfb54bbdf90f322e7e7fa6dee3bc0def:1
</code></pre></td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli getspaceout "3ac9772d3451bd0b2a7cfa09c3eecebdbfb54bbdf90f322e7e7fa6dee3bc0def:1"
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getspaceout","params":["3ac9772d3451bd0b2a7cfa09c3eecebdbfb54bbdf90f322e7e7fa6dee3bc0def:1"],"id":1}'
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
    "expire_height": 925977,
    "data": null
  },
  "value": 666,
  "script_pubkey": "........"
}
```
{% endtab %}

{% tab title="Other UTXOs" %}
```json
{
  "n": 0,
  "value": 662,
  "script_pubkey": "........"
}
```
{% endtab %}
{% endtabs %}

## Estimate bid

<mark style="color:green;">`estimatebid`</mark> estimates the required bid to make it into the [auctions phase](../getting-started/auctions.md#auctions) for the given interval (in 144 block increments)::

**Params**

<table><thead><tr><th width="208">Name</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>target</code></td><td>number</td><td>The target interval e.g. specify 0 for the coming rollout, 1 for the day after and so on.</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli estimatebid 0
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
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
space-cli getrollout 0
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
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
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"getblockmeta","params":["00000000003a037a98a512ca38147af1d3a21d858596d24f83d65fbf2f6d5131"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "height": 55879,
  "tx_meta": [
    {
      "txid": "394ebef5203c4306c3f7a6d1a5c2058bac130deb24c1d7725094b31d1ff54ea4",
      "spends": [
        {
          "n": 0
        }
      ],
      "creates": [
        {
          "n": 0,
          "value": 662,
          "script_pubkey": "........"
        }
      ],
      "updates": []
    },
    {
      "txid": "f811529d79c9fc808c240a1b5087ba19610c4177a01ffa8047c3cc143cf3eb1a",
      "spends": [
        {
          "n": 0
        },
        {
          "n": 1
        },
        {
          "n": 2
        },
        {
          "n": 3
        }
      ],
      "creates": [
        {
          "n": 1,
          "name": "@bitcoin",
          "covenant": {
            "type": "transfer",
            "expire_height": 108439,
            "data": "68656c6c6f20776f726c64"
          },
          "value": 666,
          "script_pubkey": "........"
        }
      ],
      "updates": []
    }
  ]
}
```

## Get Transaction Meta

<mark style="color:green;">`gettxmeta`</mark> Retrieves all transactions relevant to spaces for the given block (requires `-txindex` to be enabled)

**Params**

| Name    | Type   | Description                        |
| ------- | ------ | ---------------------------------- |
| `txid`  | string | The transaction id as a hex string |

{% tabs %}
{% tab title="CLI" %}
```
only available via JSON-RPC
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"gettxmeta","params":["394ebef5203c4306c3f7a6d1a5c2058bac130deb24c1d7725094b31d1ff54ea4"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "txid": "394ebef5203c4306c3f7a6d1a5c2058bac130deb24c1d7725094b31d1ff54ea4",
  "spends": [
    {
      "n": 0
    }
  ],
  "creates": [
    {
      "n": 0,
      "value": 662,
      "script_pubkey": "........"
    }
  ],
  "updates": []
}
```

## Check package

<mark style="color:green;">`checkpackage`</mark> Simulates the transactions being applied to the current blockchain state

**Params**

| Name         | Type             | Description                     |
| ------------ | ---------------- | ------------------------------- |
| `txs`        | array of strings | The transactions as hex strings |

{% tabs %}
{% tab title="CLI" %}
```
only available via JSON-RPC
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7225 \
     -H "Content-Type: application/json" \
     -d '{
        "method": "checkpackage",
        "jsonrpc": "2.0",
        "params": [["0100000000010129176a1bb5fc5dd710e1b450149542587b9654a1cc67a6bcb187b66164ffb1170000000000fdffffff0296020000000000002251209b5695ccc8a72f544d73d211e7c9fa94bd55790f6a2edfd77fe593ea0314c668cfee052a01000000225120324220a672fbadd3e25c3e732d9f3c0f5827acb5ce4b148a69511409735ef7b1014055a6b387d3671f0282a12a45eda1d5021f64c2d2649b5f2b493f0707513c36c3df77bd6ffb8e351609d3f6b8a3c620411d079d43b0e554f74e0109d12a8d109016c96f67","01000000000101823c0fa865bfd69ba958e14e5d02f279196b68d3c3836835af63089cf3491d710000000000fdffffff04980200000000000022512061225aeea7aca1e4db303c05c470eae17f8a0dba30755414ec67eb34e2351bb796020000000000002251203892e1fbdbdb5f5b747368acaf39494a8e5d093b570a633f5e6d3df8a9a39b56960200000000000022512080399b859701298a79a3ac1bec8bd1c504a899eb358a6daba0c40a1e8d3a43df4be9052a0100000022512061225aeea7aca1e4db303c05c470eae17f8a0dba30755414ec67eb34e2351bb70140766c4817ef799c80dca7ad61cc41c398d40813ae486a90861deb8fc385f317775129efa204154fd60bfa7856c5d233aeeeae66d4402e75b87fb3eb04a455087216c96f67,"01000000000102042dced1d38f424b1dba3d8341b4ff1e0366bdd6f5aa941af201761377fbc0370000000000fdffffff82fc2357effd1cf761fb3edd3036be22c7e19de78cf7e04b609247878ecc1b120000000000fdffffff01e803000000000000436a4101ea364f18ab229f237c94c1444be888e0bff73ee72701bf249ff8c690ae505984df6e2347006fed51086844017c82afb9455077ca0a9cb4041fb9d4573550177d0140e4b4d300baa8e3c645c9eb93bc1f8a5782413b526fa55e2a875143d1657e8832287298ff08e0faef079516d5be02b6ed8dcdfdff4b6fae67c0084b2618295bec0340479ba59533ea6a15100185725271d2e6bd2b3f5ff75cab38a967cdf20b382acab93aaa210a1fb6bb52eff28d6e72e26dec410962fce4a9281bc249172ea96af22d09dededede01033131317520f986f35f836d5e17bef8d72440f679ee93d38cafc00bf7db497eeeefc968375dac21c0f986f35f836d5e17bef8d72440f679ee93d38cafc00bf7db497eeeefc968375d8f000000"]],
        "id": 1
      }'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
[
  {
    "txid": "e318d853699b3f6c734d19ccb8a04b0428252fd8ef0e650c23fd07edc9a1e438",
    "spends": [
      {
        "n": 1
      }
    ],
    "creates": [
      {
        "n": 0,
        "value": 2,
        "script_pubkey": "........"
      },
      {
        "n": 1,
        "value": 1662,
        "script_pubkey": "........"
      }
    ],
    "updates": [
      {
        "type": "revoke",
        "reason": "bid_psbt_output_spent",
        "output": {
          "txid": "2a6fb4db5cd40ddccb4d49ada1caa78ea313ae6bae8b47ca12fd6f45a589ba75",
          "n": 1,
          "name": "@space",
          "covenant": {
            "type": "bid",
            "burn_increment": 1000,
            "signature": "........",
            "total_burned": 1000,
            "claim_height": null
          },
          "value": 662,
          "script_pubkey": "........"
        }
      }
    ]
  }
]
```
