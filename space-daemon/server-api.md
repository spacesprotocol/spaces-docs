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

**Response**

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

**Example Response**

{% tabs %}
{% tab title="Pre-auction" %}
<pre class="language-json"><code class="lang-json">{
  "outpoint": "<a data-footnote-ref href="#user-content-fn-1">1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc:1</a>",
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
{% endtab %}

{% tab title="In Auction" %}
```json
{
  "outpoint": "e2931d35c561120572716b065ade80ab1faf5e3cd06866554902467e96cda5fa:1",
  "value": 662,
  "script_pubkey": "51200a88c8c50ce0aa50107de7803ebb3818f34d8123c9750ba04c5fb9456bddd3ee",
  "name": "@nostr",
  "covenant": {
    "type": "bid",
    "burn_increment": 2,
    "signature": "069e91e235fe4ef5e0095f7ca99bf5ddd7f968c7dd6508c4c2aa839c3f9fc1be7c1b3b139a7a8f5761fe2ab52739055589ad7c162e213edd0f3cc3b4fcf47b45",
    "total_burned": 2001,
    "claim_height": 41747
  }
}
```
{% endtab %}

{% tab title="Claimed" %}
```json
{
  "outpoint": "b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc:1",
  "value": 662,
  "script_pubkey": "5120882cd5b0ef333be2efd7efd9bea0953f894677f1d5a638c8a3d13734e99d22e6",
  "name": "@bitcoin",
  "covenant": {
    "type": "transfer",
    "expire_height": 93050,
    "data": null
  }
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

**Example Response**\
Responds with an outpoint with `txid:vout` format.

```json
b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc:1
```



## Get Spaceout

<mark style="color:green;">`getspaceout`</mark> retrieves a _spaceout_ which are any UTXOs tracked by the spaces protocol not necessarily ones with a space.

**Params**

<table><thead><tr><th width="137">Name</th><th width="87">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>outpoint</code></td><td>string</td><td><p>An Outpoint which is a specific output within a transaction using <code>txid:vout</code> format e.g. </p><pre class="language-json"><code class="lang-json">b2819258b2416314a36e8f66840ebb5682e2600a07c28a04b4e27fe0b51b46fc:1
</code></pre></td></tr></tbody></table>

**Example Response**

{% tabs %}
{% tab title="Space UTXO" %}
<pre class="language-json"><code class="lang-json"><strong>{ 
</strong><strong>  "value": 662,
</strong>  "script_pubkey": "5120882cd5b0ef333be2efd7efd9bea0953f894677f1d5a638c8a3d13734e99d22e6",
  "name": "@bitcoin",
  "covenant": {
     "type": "transfer",
     "expire_height": 93050,
     "data": null
  }
}
</code></pre>
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

<mark style="color:green;">`estimatebid`</mark> estimates the required bid to make it into the [auctions phase](../getting-started/publish-your-docs.md#auctions) within the target block: &#x20;

**Params**

| Name     | Type   | Description              |
| -------- | ------ | ------------------------ |
| `target` | number | The target rollout block |

**Example Response**\
Responds with an amount in Satoshis

```json
2000
```



## Get Rollout

<mark style="color:green;">`getrollout`</mark> get spaces rolling out into auctions for the given interval (in 144 block increments): &#x20;

**Params**

<table><thead><tr><th width="208">Name</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>target</code></td><td>number</td><td>The target interval e.g. specify 0 for the coming rollout, 1 for the day after and so on.</td></tr></tbody></table>

**Example Response**\
An array of spaces expected to be in auctions within the given `target`

```json
[
  {
    "outpoint": "1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc:1",
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
]
```



## Get Block Data

<mark style="color:green;">`getblockdata`</mark> Retrieves all transactions relevant to spaces for the given block (requires [block indexing](configuration.md#options) to be enabled)

**Params**

| Name         | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| `block_hash` | string | The block hash as hex string |

**Example Response**

```json
{
  "tx_data": [
    {
      "version": 1,
      "txid": "4f9911d6b2bd0f54ea1f9eb44eafea048c426b4efdffba319cb5f6163b9cba9f",
      "lock_time": 1723928222,
      "vin": [
        {
          "previous_output": "08a00afe02963139d16e5b307a7c2420db538438eeb7aa5662ccfe16a383c0ea:1",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "30440220026dde59fba65d48e4d052f007798ab52bcce8baf566dd15a5a23f6631cf58be0220686a7224f361485f9972d90839e8afa7e3030fd65805b4cac08112674380805001",
            "03ff927e6c7e73d7eb53f0818273b7e7e4e24a9aad6fa83f8af6fca2ee113a965f"
          ]
        }
      ],
      "vout": [
        {
          "value": 660,
          "script_pubkey": "512042f2adecff4509d816a2d306711ef61671d82b7ea4597cbee2e684df1c39e6d8"
        },
        {
          "value": 662,
          "script_pubkey": "51206ce9d7dee41bdeecd2274afff909789f319a91ab844210cf8b05a3ebb35d857f"
        },
        {
          "value": 662,
          "script_pubkey": "5120da0a691c51bb08da5b3883dbc797c0ac59ee6d2067afc0beb8a1ceb763c1f64d"
        },
        {
          "value": 497777,
          "script_pubkey": "001497e006e72b4078d94316cb63d53c66d8d04dc0b9"
        }
      ],
      "vmetaout": []
    },
    {
      "version": 1,
      "txid": "e4dbb2725eb2597658e51d9921e1fdbee0efe78f2a7e3241866de1fd0623d456",
      "lock_time": 40044,
      "vin": [
        {
          "previous_output": "4f9911d6b2bd0f54ea1f9eb44eafea048c426b4efdffba319cb5f6163b9cba9f:0",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "9330a8fd300e185507d9470b16bad8a65470b81885b696a03b3ec89c9788bd0aabe110147327f7639f7d30afd47ce5fd04cf3d090cef94fa25cb23a0d812e3ad"
          ]
        },
        {
          "previous_output": "4f9911d6b2bd0f54ea1f9eb44eafea048c426b4efdffba319cb5f6163b9cba9f:2",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "b57a62fa827848a01d12b6ca32acbb7c466db498fa53bcf20712d56582e768f0bedba844bff2b767d791b3b13473e52fbb79df894f210924a9f9cfc2fc9c6d1d",
            "0edededede090006046d6f6f6e00017520d0f01daf760b384a8f90c160055581361e2ae535e490d7865d20dd6f0a0aea46ac",
            "c1d0f01daf760b384a8f90c160055581361e2ae535e490d7865d20dd6f0a0aea46"
          ],
          "script_error": null
        }
      ],
      "vout": [
        {
          "value": 1000,
          "script_pubkey": "6a4101372b0c46d5ba47ee810af6dd58d1dd09512fb429b60d57c711673cd922c7a7d3d22808d320a125c3bb1666f9415395bbe0a4a4c884dcbfd254958d254c0d8c50"
        }
      ],
      "vmetaout": [
        {
          "outpoint": "4f9911d6b2bd0f54ea1f9eb44eafea048c426b4efdffba319cb5f6163b9cba9f:1",
          "value": 662,
          "script_pubkey": "51206ce9d7dee41bdeecd2274afff909789f319a91ab844210cf8b05a3ebb35d857f",
          "name": "@moon",
          "covenant": {
            "type": "bid",
            "burn_increment": 1000,
            "signature": "372b0c46d5ba47ee810af6dd58d1dd09512fb429b60d57c711673cd922c7a7d3d22808d320a125c3bb1666f9415395bbe0a4a4c884dcbfd254958d254c0d8c50",
            "total_burned": 1000,
            "claim_height": null
          }
        }
      ]
    },
    {
      "version": 1,
      "txid": "c23851a97c9ef620e57366cafa7985a343464cb78736771db10be5492ab62c03",
      "lock_time": 40044,
      "vin": [
        {
          "previous_output": "a4f2099d8e06f1e399a86f3023fe5d29c694f53191e5b5613d688cd17eb73ee7:0",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "df49506ad63d5522e4c108a6704cd15b23297aeb4ed0e0029704c4edaef1faf5f935086c77287fa8ec9fd57fcb546bffb8a496e25a4c11d550dac10a6ea307b3"
          ]
        },
        {
          "previous_output": "a4f2099d8e06f1e399a86f3023fe5d29c694f53191e5b5613d688cd17eb73ee7:2",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "0c2c137b295342e975b282f448e001373b4e7197bd433a03f7529053e6babe22ccd5834fbf6636655f8cb173ed2b89ea3d6447b454f794eaaff9fc9e266cb6b7",
            "10dededede0b00080673617475726e0001752047f1a47fca5aace0bae4cf72671d2d7c0c35d3434b45625c3d723b374122aa22ac",
            "c147f1a47fca5aace0bae4cf72671d2d7c0c35d3434b45625c3d723b374122aa22"
          ],
          "script_error": null
        }
      ],
      "vout": [
        {
          "value": 1000,
          "script_pubkey": "6a41015fea5f6e50f077255789cab1accbf4879d632061365bcc00858c54c986c7ec40ef80c714de22f47796e46ad629eb197cd8216d2e0a9c6f9cb4712dcb875a5900"
        }
      ],
      "vmetaout": [
        {
          "outpoint": "a4f2099d8e06f1e399a86f3023fe5d29c694f53191e5b5613d688cd17eb73ee7:1",
          "value": 662,
          "script_pubkey": "512006339baacbbba04d28fa7dc742754912a2186b20f85ca98818c4cbc39addfa6f",
          "name": "@saturn",
          "covenant": {
            "type": "bid",
            "burn_increment": 1000,
            "signature": "5fea5f6e50f077255789cab1accbf4879d632061365bcc00858c54c986c7ec40ef80c714de22f47796e46ad629eb197cd8216d2e0a9c6f9cb4712dcb875a5900",
            "total_burned": 1000,
            "claim_height": null
          }
        }
      ]
    },
    {
      "version": 1,
      "txid": "b6ee7332208cf958ce20c78e4c631d10cfed480cf2a427ff165a153ca8247732",
      "lock_time": 40044,
      "vin": [
        {
          "previous_output": "304a39bcf373de3e595c00537c4a88b636c4843d247b8ea81e304604ce31d5bd:0",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "075f2222c88a7f1fb76e452b7d37726fcf991d544efaef6cfe5b5ea458b2fba4588ad856c0d118f48f5cf23fd19011e8f190972387b3aeef09c8ea0d86963808"
          ]
        },
        {
          "previous_output": "304a39bcf373de3e595c00537c4a88b636c4843d247b8ea81e304604ce31d5bd:2",
          "script_sig": "",
          "sequence": 4294967293,
          "witness": [
            "00ac17d413f71cd4bd4cdc170ec8f99e5108d674148b17b0c3e6b369a7716bf6e02c7726b8b2932a5405acb216666c3005994bab430f47f3c23460489e0dc6ee",
            "0fdededede0a000705636f6d657400017520b698d4bce6e35441e5d44f10a772b3b8f354c464bf1926ee4c9984fd88caaee1ac",
            "c1b698d4bce6e35441e5d44f10a772b3b8f354c464bf1926ee4c9984fd88caaee1"
          ],
          "script_error": null
        }
      ],
      "vout": [
        {
          "value": 1000,
          "script_pubkey": "6a410164c3d7fe5dab4b0e53be990a14c9ef0665f62d8c6381835cb937e40098903e0aeb1233da38801f0e6d65d7ba8bb9369381305967f19dd7423097b31d681f15db"
        }
      ],
      "vmetaout": [
        {
          "outpoint": "304a39bcf373de3e595c00537c4a88b636c4843d247b8ea81e304604ce31d5bd:1",
          "value": 662,
          "script_pubkey": "51203945822eef6071f9d313b8516591892a71429721f8cb0476bea18a8e10d311ef",
          "name": "@comet",
          "covenant": {
            "type": "bid",
            "burn_increment": 1000,
            "signature": "64c3d7fe5dab4b0e53be990a14c9ef0665f62d8c6381835cb937e40098903e0aeb1233da38801f0e6d65d7ba8bb9369381305967f19dd7423097b31d681f15db",
            "total_burned": 1000,
            "claim_height": null
          }
        }
      ]
    }
  ]
}
```

[^1]: Checkout this outpoint here\
    [https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc#vout=](https://mempool.space/testnet4/tx/1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc#vout=1)
