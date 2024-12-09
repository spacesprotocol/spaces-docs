# Wallet API

Both the wallet and server use [JSON-RPC](server-api.md) over the same endpoint. 

CLI commands accept an optional `--wallet` parameter to specify which wallet to use. If not specified, it defaults to `default`.

RPC commands differ from their CLI counterparts due to legacy compatibility reasons. This may be harmonized in future releases.

## Create wallet

<mark style="color:green;">`createwallet`</mark> creates and loads a new wallet

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

<mark style="color:green;">`loadwallet`</mark> creates and loads a new wallet

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

<mark style="color:green;">`exportwallet`</mark> exports the wallet in JSON format. CLI command saves the response to the target file.

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

CLI has separate commands instead: `open`, `bid`, `register`, `transfer`, `send` and `createbidouts`

**Params**

| Name    | Type                                                             | Description                                 |
|---------|------------------------------------------------------------------|---------------------------------------------|
|`name`   | string                                                           | Wallet name                                 |
|`request`| JSON object of [Batch Request](wallet-api.md#batch-request-type) | Request containing one or more transactions |

### Batch Request Type

| Property          | Type                                                               | Description                                                                                                                                                                                            |
| ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `bidouts`         | number (optional)                                                  | Create the specified number of auction outputs. These are output pairs used during the bidding process. If null, it will create the minimum number required to satisfy the requests.                   |
| `requests`        | JSON array of [Wallet Requests](wallet-api.md#wallet-request-type) | Bundles one or multiple requests                                                                                                                                                                       |
| `fee_rate`        | number (optional)                                                  | Fee rate in sat/vB. If null, it will attempt to use `estimatesmartfee` instead.                                                                                                                        |
| `dust`            | number (optional)                                                  | Custom dust amount to use for auction outputs                                                                                                                                                          |
| `force`           | bool                                                               | Forces the transaction to be created even if it will be rejected by the protocol or results in revoking the space (mainly used for testing)                                                            |
| `confirmed_only`  | bool                                                               | Use only confirmed UTXOs                                                                                                                                                                               |
| `skip_tx_check`   | bool                                                               | Skip tx checker (not recommended)                                                                                                                                                                      |

### Wallet Request Type

A single wallet request

| Property  | Type   | Description                                                                                                                                                                           |
| --------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request` | string | The transaction/request type. Can be `open`, `bid`, `register`, `execute`, `transfer`, or `sendcoins`                                                                                 |
| `name`    | string | The space name                                                                                                                                                                        |
| `amount`  | string | Amount in Satoshis. Required for `open`, `bid` and `sendcoins` transactions.                                                                                                          |
| `to`      | string | Recipient Bitcoin address or a space name. Required for <code>transfer</code> and <code>sendcoins</code> requests. Optional for <code>register</code> transactions.                   |


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
| `fee_rate`        | number             | Fee rate in sat/vB                  |
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

## List Spaces

<mark style="color:green;">`listspaces`</mark> list all spaces currently owned by the wallet including ones actively in auction

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
[
  {
    "outpoint": "633bdcf13aa3612511c48e6c6ce5273bb782056798fdfe438c733d75f9c78c74:1",
    "txout": {
      "value": 662,
      "script_pubkey": "5120f6ce498e67df193ef20929adb989fdda6f008f601a53ed1074adafe224bd0183"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 1,
    "confirmation_time": {
      "Confirmed": {
        "height": 57229,
        "time": 1733755861
      }
    },
    "space": {
      "name": "@test129873189372198",
      "covenant": {
        "type": "bid",
        "burn_increment": 1000,
        "signature": "08182475f106138f5b96b5de4f57d5661d0421390a056cd5da39beb46e617697b36f5c8deb6e737d625285f4f6244d4e4d48ac9d841a9ba41acea5d1d7f29458",
        "total_burned": 1000,
        "claim_height": null
      }
    },
    "is_spaceout": true
  }
]
```

## List Unspent

<mark style="color:green;">`listunspent`</mark> list unspent transaction outputs

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
    "outpoint": "633bdcf13aa3612511c48e6c6ce5273bb782056798fdfe438c733d75f9c78c74:1",
    "txout": {
      "value": 662,
      "script_pubkey": "5120f6ce498e67df193ef20929adb989fdda6f008f601a53ed1074adafe224bd0183"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 1,
    "confirmation_time": {
      "Confirmed": {
        "height": 57229,
        "time": 1733755861
      }
    },
    "space": {
      "name": "@test129873189372198",
      "covenant": {
        "type": "bid",
        "burn_increment": 1000,
        "signature": "08182475f106138f5b96b5de4f57d5661d0421390a056cd5da39beb46e617697b36f5c8deb6e737d625285f4f6244d4e4d48ac9d841a9ba41acea5d1d7f29458",
        "total_burned": 1000,
        "claim_height": null
      }
    },
    "is_spaceout": true
  },
  {
    "outpoint": "65dcad71550fc2f59553b7525d131f8c3b416d1310b46e5aa6428feaf742b0e4:1",
    "txout": {
      "value": 662,
      "script_pubkey": "5120aac8a8c793b5f8041fc602c22717a743c1e54f0bb225c9f251dd0a6171f99124"
    },
    "keychain": "External",
    "is_spent": false,
    "derivation_index": 2,
    "confirmation_time": {
      "Confirmed": {
        "height": 57235,
        "time": 1733757063
      }
    },
    "space": null,
    "is_spaceout": true
  }
]
```

## List Auction Outputs

<mark style="color:green;">`listbidouts`</mark> lists all output pairs that can be used during bids

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
        "script_pubkey": "512095f0c166b48eb39dc9dcda3225d5ba1a24db25806e27126b08f272fdf6ac9040"
      }
    },
    "auction": {
      "outpoint": "d8907044ae65355b244395f42318a250eeabed49bc1dab63a5cf2553daaed2ad:1",
      "txout": {
        "value": 662,
        "script_pubkey": "512064f286fc6ce6959e025b7f2de1722318500ae0f5e745fecd015f85a354790401"
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
     -d '{"jsonrpc":"2.0","method":"walletlisttransactions","params":["default",2,0],"id":1}'
```
{% endtab %}
{% endtabs %}

**Response**

```json
[ 
  {
    "txid": "633bdcf13aa3612511c48e6c6ce5273bb782056798fdfe438c733d75f9c78c74",
    "confirmed": true,
    "sent": 26152,
    "received": 22207,
    "fee": 3945
  },
  {
    "txid": "1c6bb6df12990f77851f84f0f954b665a3eab82ba4acfabcacdfa7da93d87509",
    "confirmed": true,
    "sent": 0,
    "received": 26152,
    "fee": null
  }
]
```
