# Wallet API

Both the wallet and server use [RPC](server-api.md) over same endpoint except that all wallet methods are prefixed with `wallet.`



## Import wallet

<mark style="color:green;">`walletexport`</mark> retrieve the current state of the spaces daemon

**Params**&#x20;

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>content</code></td><td>string (json)</td><td>Same format as <a href="wallet-api.md#export-wallet">export wallet</a></td></tr></tbody></table>

**Example Response**

```json
{
  "descriptor":"...",
  "spaces_descriptor":"....",
  "block_height":41530,
  "label":"walletname"
}
```



## Export wallet

<mark style="color:green;">`walletexport`</mark> retrieve the current state of the spaces daemon

**Params**&#x20;

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

**Example Response**

```json
{
  "descriptor":"...",
  "spaces_descriptor":"....",
  "block_height":41530,
  "label":"walletname"
}
```



## Get Wallet Info

<mark style="color:green;">`walletgetinfo`</mark> retrieve the current state of the spaces daemon

**Params**&#x20;

<table><thead><tr><th width="223">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

**Example Response**

```json
{
  "label": "default",
  "start_block": 41530,
  "tip": 41818,
  "descriptors": [
    ....
  ]
}
```



## Get Balance

<mark style="color:green;">`walletgetbalance`</mark> get the wallet balance

**Params**&#x20;

<table><thead><tr><th width="191">Name</th><th width="178">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

**Response**

```json
{
  "confirmed": {
    "total": 0,
    "spendable": 0,
    "immature": 0,
    "locked": 0
  },
  "unconfirmed": {
    "total": 0,
    "locked": 0
  }
}
```



## Batch Request Type

Can be passed to [`walletsendrequest`](wallet-api.md#send-request) to create one or multiple transactions



| Property          | Type                                                              | Description                                                                                                                                                                                                            |
| ----------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `auction_outputs` | number (optional)                                                 | Create the specified number of auction outputs. These are output pairs used during [bid transactions ](../deep-dive/images-and-media.md). If null, it will create the minimum number required to satisfy the requests. |
| `requests`        | JSON array of [WalletRequests](wallet-api.md#wallet-request-type) | Bundles one or multiple requests                                                                                                                                                                                       |
| `fee_rate`        | number (optional)                                                 | Fee rate in sat/kwu. If null, it will attempt to use `estimatesmartfee` instead.                                                                                                                                       |
| `dust`            | number (optional)                                                 | Custom dust amount to use for auction outputs                                                                                                                                                                          |
| `force`           | bool (optional)                                                   | Forces the transaction to be created even if it will be rejected by the protocol or results in revoking the space (mainly used for testing)                                                                            |



## Wallet Request Type

A single wallet request

| Property  | Type   | Description                                                                                                                                                                                   |
| --------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request` | string | The transaction/request type. Can be `open`, `bid`, `register`, `sendspaces`, or `sendcoins`                                                                                                  |
| `name`    | string | The space name                                                                                                                                                                                |
| `amount`  | string | Amount in Satoshis. Required for `open`, `bid` and `sendcoins` transactions.                                                                                                                  |
| `to`      | string | <p>Required for <code>sendspaces</code> and <code>sendcoins</code> requests. Optional for <code>register</code> transactions. </p><p></p><p>Can be a Bitcoin address or a space name.<br></p> |



## Send Request

<mark style="color:green;">`walletsendrequest`</mark> send one or multiple requests&#x20;

**Params**&#x20;

<table><thead><tr><th width="191">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr><tr><td><code>body</code></td><td>JSON</td><td>A <a href="wallet-api.md#batch-request-type">Batch Request</a> object</td></tr></tbody></table>

**Response**

```json
[
  {
    "txid": "1ea73982abb36cf2c62deced717fbe944c3af89abe768aa454642879b29e5adc",
    "tags": ["auction-outputs", "commitment"]
  },
  {
    "txid": "792a2fd221e4715d6c4f330fb46309d6f6a7ed4fd0f9c50471b77e643f9885d2",
    "tags": ["open"]
  }
]
```



## Bump Fee

<mark style="color:green;">`walletbumpfee`</mark> bumps the fee for the given wallet transaction

**Params**&#x20;

<table><thead><tr><th width="191">Name</th><th width="178">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr><tr><td><code>txid</code></td><td>string</td><td>Transaction id as hex string</td></tr><tr><td><code>fee_rate</code></td><td>number</td><td>the new fee rate in sat/vb</td></tr></tbody></table>



## List Spaces

<mark style="color:green;">`walletlistspaces`</mark> list all spaces currently owned by the wallet including ones actively in auction

**Params**&#x20;

<table><thead><tr><th width="275">Name</th><th width="178">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>



## List Unspent

<mark style="color:green;">`walletlistunspent`</mark> list unspent transaction outputs

**Params**&#x20;

<table><thead><tr><th width="191">Name</th><th width="178">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>



## List Auction Outputs

<mark style="color:green;">`walletlistauctionouputs`</mark> lists all output pairs the can be used during auctions

**Params**&#x20;

<table><thead><tr><th width="191">Name</th><th width="178">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>wallet</code></td><td>string</td><td>Wallet name</td></tr></tbody></table>

