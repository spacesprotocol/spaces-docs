---
cover: ../.gitbook/assets/Fabric bg (1).svg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Fabric DHT

Fabric enables you to sign Nostr events, and DNS records with a Bitcoin sovereign handle (a `space`), or a public key (an `npub`), and publish them to the Fabric peer-to-peer network without adding any bloat to the Bitcoin blockchain!

## Who can use it?

* **Own a Space?** Publish forward records tied to your handle e.g., `@example`. To grab one, see [quick start](quickstart.md) or buy one from a [secondary marketplace](https://spaces.market).
* **No Space? No Problem!** Use your `npub` to publish Nostr events (like NIP-65 relay lists) instead.

```
npm install -g @spacesprotocol/fabric 
```

and verify installation

```
fabric --version
beam --version
```

By default, the `beam` tool will try to load trust anchors from `http://127.0.0.1:7225/root-anchors.json` to verify space handles. If you don't have a spaces [spaces client](https://github.com/spacesprotocol/spaces), you can load the file from an external source that you trust e.g.

```
export FABRIC_REMOTE_ANCHORS=https://bitpki.com/root-anchors.json
```

Don't trust, verify! Run your own Bitcoin full node and spaces client.\


#### DNS Records

```
beam <space> <record-type>
```

For example:

```
beam @buffrr TXT
```

#### Nostr events

```
beam <space-or-pubkey> <event-kind> <optional-d-tag>
```



## Publishing records by handle

If you own a space, you can publish forward records resolvable by your handle e.g. `@example`

### Publish DNS records

{% hint style="info" %}
Spaces use the DNS class code 2 (CLASS2) to further distinguish them from regular domains in the IN class.
{% endhint %}

1. **Create a zone file**:

Add your records to a file and save it e.g. `myexample.zone`

```
@example. 3600 CLASS2  TXT   "Hello Fabric"
@example. 3600 CLASS2  A      127.0.0.1
```

Remember to replace `@example`with your space name, and remember to put the `.`at the end. It's not a typo in DNS zone files!

2. **Sign the file**

```
space-cli signzone @example myexample.zone > signed.event.json
```

The wallet will turn it into a signed Nostr event and bundles proof information to make it ready for publishing to Fabric.

3. **Publish it with** `beam`

```
beam publish signed.event.json
```

### Publish Nostr Events

To sign a forward event (lookup by a space handle), you could do the following:

Note: there's no standard yet for using kind 0 for forward lookups yet but it's here as an example.

```
{
  "kind": 0,
  "created_at": 1679673265,
  "content": "{\"name\": \"Alice\", \"about\": \"A Nostr enthusiast\", \"picture\": \"https://example.com/avatar.jpg\"}",
  "tags": []
}
```

```
space-cli signevent @example event.json --anchor > event.signed.json
```

For lookups by handle, you should use the `--anchor`option to include trust path information and append the `space`tag to the event.

```
beam publish event.signed.json
```

## Publishing records by pubkey

If you have an `npub`, you could publish events on Fabric. This is still experimental and no Nostr client supports lookups through Fabric yet. Here's an example publishing a NIP-65 relay list:&#x20;

```
{
  "kind": 10002,
  "created_at": 1679673265,
  "pubkey": "97c70a44366a6535c145b333f973ea86dfdc2d7a99da618c40c64705ad98e322",
  "tags": [
    ["r", "wss://alicerelay.example.com"],
    ["r", "wss://brando-relay.com"],
    ["r", "wss://expensive-relay.example2.com", "write"],
    ["r", "wss://nostr-relay.example.com", "read"]
  ],
  "content": "",
  "sig": "<signature>"
}
```

Then publish it&#x20;

```
beam publish event.signed.json
```



