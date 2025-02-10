# Wallet API

Both the wallet and server use [JSON-RPC](server-api.md) over the same endpoint. 

CLI commands accept an optional `--wallet` parameter to specify which wallet to use. If not specified, it defaults to `default`.

RPC commands differ from their CLI counterparts due to legacy compatibility reasons. This may be harmonized in future releases.

## Create wallet

<mark style="color:green;">`walletcreate`</mark> creates and loads a new wallet

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 createwallet -w newwallet
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletcreate","params":["newwallet"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
null
```

## Load wallet

<mark style="color:green;">`walletload`</mark> creates and loads a new wallet

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 loadwallet
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletload","params":["default"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
null
```

## Export wallet

<mark style="color:green;">`walletexport`</mark> exports the wallet in JSON format. CLI command saves the response to the target file.

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 exportwallet -w mywallet wallet.json
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletexport","params":["mywallet"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "descriptor": "...",
  "blockheight": 41530,
  "label": "mywallet"
}
```

## Import wallet

<mark style="color:green;">`importwallet`</mark> imports the wallet from JSON description. CLI command loads the description from the source file.

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>content</code></td><td>string (json)</td><td>Same format as <a href="wallet-api.md#export-wallet">export wallet</a></td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 importwallet wallet.json
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletimport","params":[{"descriptor":"...","blockheight":41530,"label":"mywallet"}],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
null
```

## Get Wallet Info

<mark style="color:green;">`getwalletinfo`</mark> retrieve the wallet info

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getwalletinfo -w mywallet
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletgetinfo","params":["mywallet"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "label": "mywallet",
  "start_block": 56801,
  "tip": 56821,
  "descriptors": [
    {
      "descriptor": "...",
      "internal": false,
      "spaces": true
    },
    {
      "descriptor": "...",
      "internal": true,
      "spaces": true
    }
  ]
}
```

## Get Balance

<mark style="color:green;">`balance`</mark> gets the wallet balance

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 balance -w mywallet
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletgetbalance","params":["mywallet"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "balance": 0,
  "details": {
    "immature": 0,
    "trusted_pending": 0,
    "untrusted_pending": 0,
    "confirmed": 0,
    "dust": 0
  }
}
```

## Get New Address

CLI has separate commands instead for different `kind` values.

<mark style="color:green;">`getnewaddress`</mark> gets a new Bitcoin address suitable for receiving coins compatible with most bitcoin wallets.

<mark style="color:green;">`getnewspaceaddress`</mark> gets a new Bitcoin address suitable for receiving spaces and coins (Spaces compatible bitcoin wallets only).

**Params**

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr><tr><td><code>kind</code></td><td>string</td><td><code>Coin</code> or <code>Space</code></td></tr></tbody></table>

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 getnewaddress -w mywallet
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletgetnewaddress","params":["mywallet", "Coin"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
"tb1phg2rcjunxew9eqruehs2ejqmyjhzwnzfzrvmyl3ugaezpvf5d87qwj763n"
```

## Send Request

<mark style="color:green;">`walletsendrequest`</mark> send one or multiple requests

CLI has separate commands instead: `open`, `bid`, `register`, `transfer`, `renew` and `createbidouts`

**Params**

| Name    | Type                                                             | Description                                 |
|---------|------------------------------------------------------------------|---------------------------------------------|
|`name`   | string                                                           | Wallet name                                 |
|`request`| [Batch Request](wallet-api.md#batch-request-type)                | Request containing one or more transactions |

### Batch Request Type

| Property          | Type                                                               | Description                                                                                                                                                                                            |
| ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `bidouts`         | number (optional)                                                  | Create the specified number of auction outputs. These are output pairs used during the bidding process. If null, it will create several to satisfy the requests.                                       |
| `requests`        | array of [Wallet Requests](wallet-api.md#wallet-request-type)      | Bundles one or multiple requests                                                                                                                                                                       |
| `fee_rate`        | number (optional)                                                  | Fee rate in sat/vB. If null, it will attempt to use `estimatesmartfee` instead.                                                                                                                        |
| `dust`            | number (optional)                                                  | Custom dust amount to use for auction outputs                                                                                                                                                          |
| `force`           | bool                                                               | Forces the transaction to be created even if it will be rejected by the protocol or results in revoking the space (mainly used for testing)                                                            |
| `confirmed_only`  | bool                                                               | Use only confirmed UTXOs                                                                                                                                                                               |
| `skip_tx_check`   | bool                                                               | Skip tx checker (not recommended)                                                                                                                                                                      |

### Wallet Request Type

A single wallet request

| Property  | Type             | Description                                                                                                                                                                           |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request` | string           | The transaction/request type. Can be `open`, `bid`, `register`, `transfer`, `sendcoins` or `execute`.                                                                                 |
| `name`    | string           | The space name. Required for `open`, `bid` and `register` transactions.                                                                                                                                                                              |
| `spaces`  | array of strings | The spaces names. Required for `transfer` transaction.                                                                                                                                                                              |
| `amount`  | string           | Amount in satoshis. Required for `open`, `bid` and `sendcoins` transactions.                                                                                                          |
| `to`      | string           | Recipient Bitcoin address or a space name. Required for `transfer` and `sendcoins` requests. Optional for `register` transactions.                                                    |


{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 open @mynewspace 1000
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{
       "jsonrpc": "2.0",
       "method": "walletsendrequest",
       "params": [
         "default",
         {
           "bidouts": null,
           "requests": [
             {
               "request": "open",
               "name": "@mynewspace",
               "amount": 1000
             }
           ],
           "fee_rate": null,
           "dust": null,
           "force": false,
           "confirmed_only": false,
           "skip_tx_check": false
         }
       ],
       "id": 1
     }'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[
  {
    "txid": "3529a729fc2dff2184339f1accf7c8fc3139bcc25f0b874816b6522daea7ee5b",
    "tags": [
      "bidouts",
      "commitment"
    ]
  },
  {
    "txid": "747c3877dea45c065b40a61b190d2160e917124bc46054c6c80e7f6ee6bc7995",
    "tags": [
      "open"
    ]
  }
]
```

## Bump Fee

<mark style="color:green;">`bumpfee`</mark> bumps the fee for the given wallet transaction

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |
| `txid`            | string             | The transaction id as a hex string  |
| `fee_rate`        | number (optional)  | Fee rate in sat/vB                  |
| `skip_tx_check`   | bool               | Skip tx checker (not recommended)   |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 bumpfee 14804a155846b56a87c6d90088d8174e8e8640d5bcaa00132a4968e105b48d27 --fee-rate 1000
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletbumpfee","params":["default", "14804a155846b56a87c6d90088d8174e8e8640d5bcaa00132a4968e105b48d27", 1000],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[
  {
    "txid":"65dcad71550fc2f59553b7525d131f8c3b416d1310b46e5aa6428feaf742b0e4",
    "tags":[
      "fee-bump"
    ]
  }
]
```

## Sell

<mark style="color:green;">`walletsell`</mark> creates PSBT which can be used to sell the space

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |
| `space`           | string             | The space name                      |
| `price`           | number             | Price in satoshis                   |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 sell @space3 10000
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7218 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletsell","params":["default", "@space", 10000],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "space": "@space",
  "price": 10000,
  "seller": "bcrts1pr5ux9k9jngxy8jjxyhwv7w0hvztyteklsn5y47urtwhcmdppgqpqrj9p9g",
  "signature": "........"
}
```

## Verify Listing

<mark style="color:green;">`verifylisting`</mark> verify the space sale listing

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `listing`         | object             | The `walletsell` output             |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 verifylisting --signature ........ --seller bcrts1pr5ux9k9jngxy8jjxyhwv7w0hvztyteklsn5y47urtwhcmdppgqpqrj9p9g @space 10000
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7218 \
     -H "Content-Type: application/json" \
     -d '{
        "jsonrpc": "2.0",
        "method": "verifylisting",
        "params": [{
          "space": "@space",
          "price": 10000,
          "seller": "bcrts1pr5ux9k9jngxy8jjxyhwv7w0hvztyteklsn5y47urtwhcmdppgqpqrj9p9g",
          "signature": "........"
        }],
        "id": 1
      }'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
null
```

## Buy

<mark style="color:green;">`walletbuy`</mark> buy space using the sell output

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |
| `listing`         | object             | The `walletsell` output             |
| `fee_rate`        | number (optional)  | Fee rate in sat/vB                  |
| `skip_tx_check`   | bool               | Skip tx checker (not recommended)   |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 buy --signature ........ --seller bcrts1pr5ux9k9jngxy8jjxyhwv7w0hvztyteklsn5y47urtwhcmdppgqpqrj9p9g @space 10000
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7218 \
     -H "Content-Type: application/json" \
     -d '{
        "jsonrpc": "2.0",
        "method": "walletbuy",
        "params": [
          "default",
          {
            "space": "@space",
            "price": 10000,
            "seller": "bcrts1pr5ux9k9jngxy8jjxyhwv7w0hvztyteklsn5y47urtwhcmdppgqpqrj9p9g",
            "signature": "........"
          },
          null,
          false
        ],
        "id": 1
      }'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
{
  "txid": "f51c8c474f53a846187e9eb4c0fa0a44226b35e4fd5693f138d170d57f9bb977",
  "events": [
    {
      "type": "buy",
      "space": "@space",
      "previous_spaceout": "a04100e569761211716e1452ae17a95d4d3ddd9fe8d03e8371ed8fa58b0504f6:1"
    }
  ]
}
```

## Sign Message

<mark style="color:green;">`walletsignmessage`</mark> signs the message

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |
| `space`           | string             | The space name                      |
| `message`         | string             | Message as hex string               |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 signmessage @testspace message
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7218 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletsignmessage","params":["default", "@testspace", "6d657373616765"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
"673ef14b408979ab8bc9fcae23dc309187999189fdd429128f899763de8951ded9337f0861cb60e50eded5a9344796a6810bf2186a4bc9fd27d2bb45ee23ded8"
```

## Verify Message

<mark style="color:green;">`verifymessage`</mark> verify a signed message

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `space`           | string             | The space name                      |
| `message`         | string             | Message as hex string               |
| `signature`       | string             | Signature as hex string             |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 --signature 673ef14b408979ab8bc9fcae23dc309187999189fdd429128f899763de8951ded9337f0861cb60e50eded5a9344796a6810bf2186a4bc9fd27d2bb45ee23ded8 @testspace message
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7218 \
     -H "Content-Type: application/json" \
     -d '{
        "jsonrpc": "2.0",
        "method": "verifymessage",
        "params": [{
            "space": "@testspace",
            "message": "6d657373616765",
            "signature": "673ef14b408979ab8bc9fcae23dc309187999189fdd429128f899763de8951ded9337f0861cb60e50eded5a9344796a6810bf2186a4bc9fd27d2bb45ee23ded8"
        }],
        "id": 1
      }'
```
{% endtab %}
{% endtabs %}

**Example Response**

```json
null
```

## List Spaces

<mark style="color:green;">`walletlistspaces`</mark> list all spaces currently owned by the wallet including ones actively in auction

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 listspaces
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletlistspaces","params":["default"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
{
   "winning": [
      {
         "txid": "9c44384840b40ef159764801a185a87f84947919d60d9e87771cbd47044e0467",
         "n": 3,
         "name": "@space2",
         "covenant": {
            "type": "bid",
            "burn_increment": 10000,
            "signature": "........",
            "total_burned": 10000,
            "claim_height": 9937
         },
         "value": 662,
         "script_pubkey": "........"
      }
   ],
   "outbid": [
      {
         "txid": "c2d156cde1437915eafdc551018abc594c4737ef65c0033a3ea6c1c6d779fefa",
         "n": 1,
         "name": "@space1",
         "covenant": {
            "type": "transfer",
            "expire_height": 55121,
            "data": null
         },
         "value": 666,
         "script_pubkey": "........"
      }
   ],
   "owned": [
      {
         "txid": "a04100e569761211716e1452ae17a95d4d3ddd9fe8d03e8371ed8fa58b0504f6",
         "n": 1,
         "name": "@space3",
         "covenant": {
            "type": "transfer",
            "expire_height": 55121,
            "data": null
         },
         "value": 666,
         "script_pubkey":  "........"
      }
   ]
}
```

## List Unspent

<mark style="color:green;">`walletlistunspent`</mark> list unspent transaction outputs

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 listunspent
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletlistunspent","params":["default"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[
  {
    "outpoint": "838948b831a81119e20637a807ad0fe09bae248af6183fdd668168e7f58449b2:1",
    "txout": {
      "value": 666,
      "script_pubkey": "........"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 4,
    "chain_position": {
      "Confirmed": {
        "anchor": {
          "block_id": {
            "height": 6301,
            "hash": "6e4630c207d973ad8e59b2c0e245f50496fc6b31fc6c6c4fceedba886b83ea28"
          },
          "confirmation_time": 1738698507
        },
        "transitively": null
      }
    },
    "space": {
      "name": "@space",
      "covenant": {
        "type": "transfer",
        "expire_height": 58861,
        "data": null
      }
    },
    "is_spaceout": true
  },
  {
    "outpoint": "643aefc51869a615e8313db5c85e70058341ae48289afdf7be88438817986a6e:5",
    "txout": {
      "value": 662,
      "script_pubkey": "........"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 7,
    "chain_position": {
      "Confirmed": {
        "anchor": {
          "block_id": {
            "height": 8371,
            "hash": "6055898710bdfc75ef3a96e752ae11ba89190fb8e53beb527a86999e5789160f"
          },
          "confirmation_time": 1738705621
        },
        "transitively": null
      }
    },
    "space": null,
    "is_spaceout": true
  },
  {
    "outpoint": "7b235a4c5e87406315efbeebf2671124e0b05276441700a1232a7b14eb325751:0",
    "txout": {
      "value": 10000,
      "script_pubkey": "........"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 8,
    "chain_position": {
      "Confirmed": {
        "anchor": {
          "block_id": {
            "height": 8471,
            "hash": "24ee232b4c0d9c9ff60bb2e460bd80ef076ea63adedefecaa2a594431feb03dc"
          },
          "confirmation_time": 1738705638
        },
        "transitively": null
      }
    },
    "space": null,
    "is_spaceout": false
  }
]
```

## List Auction Outputs

<mark style="color:green;">`walletlistbidouts`</mark> lists all output pairs that can be used during bids

**Params**

| Name              | Type               | Description                         |
|-------------------|--------------------|-------------------------------------|
| `name`            | string             | Wallet name                         |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 listbidouts
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletlistbidouts","params":["default"],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[
  {
    "spend": {
      "outpoint": "d8907044ae65355b244395f42318a250eeabed49bc1dab63a5cf2553daaed2ad:0",
      "txout": {
        "value": 664,
        "script_pubkey": "........"
      }
    },
    "auction": {
      "outpoint": "d8907044ae65355b244395f42318a250eeabed49bc1dab63a5cf2553daaed2ad:1",
      "txout": {
        "value": 662,
        "script_pubkey": "........"
      }
    },
    "confirmed": false
  }
]
```

## List Transactions

<mark style="color:green;">`listtransactions`</mark> lists all the transactions starting from the most recent one 

**Params**

| Name              | Type               | Description                                                    |
|-------------------|--------------------|----------------------------------------------------------------|
| `name`            | string             | Wallet name                                                    |
| `count`           | number             | The maximum number of transactions to be shown                 |
| `skip`            | number             | The number of transactions to be skipped from the beginning    |

{% tabs %}
{% tab title="CLI" %}
```
space-cli --chain testnet4 listtransactions
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST http://127.0.0.1:7224 \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","method":"walletlisttransactions","params":["default",5,0],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[
  {
    "txid": "9d32499b14810a2f10033c9ed1e20b01449e34a4b2613274ba875f3fdf81a0c8",
    "confirmed": false,
    "sent": 88950412,
    "received": 88850537,
    "fee": 99537,
    "events": [
      {
        "type": "open",
        "space": "@space1",
        "initial_bid": 1000
      }
    ]
  },
  {
    "txid": "f51c8c474f53a846187e9eb4c0fa0a44226b35e4fd5693f138d170d57f9bb977",
    "confirmed": false,
    "sent": 88851203,
    "received": 88789693,
    "fee": 61510,
    "events": [
      {
        "type": "buy",
        "space": "@space3",
        "previous_spaceout": "a04100e569761211716e1452ae17a95d4d3ddd9fe8d03e8371ed8fa58b0504f6:1"
      }
    ]
  },
  {
    "txid": "4806424d1afed83e538baf1d55f508481efd10e4a6b2bae24923c0fb6e460717",
    "confirmed": false,
    "sent": 9999999,
    "received": 9968178,
    "fee": 31159,
    "events": [
      {
        "type": "commit",
        "space": "@space1"
      }
    ]
  },
  {
    "txid": "7b235a4c5e87406315efbeebf2671124e0b05276441700a1232a7b14eb325751",
    "confirmed": true,
    "sent": 88990907,
    "received": 88959748,
    "fee": 31159,
    "events": [
      {
        "type": "send",
        "amount": 10000,
        "recipient_script_pubkey": "........"
      }
    ]
  },
  {
    "txid": "cb6de0439f34b2bf0b26f0d87c79218026379ef7d26d76b55e1883d13f5901f9",
    "confirmed": true,
    "sent": 99666094,
    "received": 99558633,
    "fee": 98123,
    "events": [
      {
        "type": "open",
        "space": "@space2",
        "initial_bid": 10000
      }
    ]
  }
]
```
