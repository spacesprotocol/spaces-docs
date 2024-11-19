# Configuration

The Spaces daemon listens on the following ports by default:

| Network  | Port |
| -------- | ---- |
| Mainnet  | 7225 |
| Testnet4 | 7224 |
| Testnet3 | 7223 |
| Regtest  | 7218 |

## Options

All these arguments can be specified as environment variables with `SPACED_` prefix and `UPPER_SNAKE_CASE`

<table><thead><tr><th width="270">Option</th><th>Description</th><th>Default</th></tr></thead><tbody><tr><td><code>--bitcoin-rpc-cookie</code></td><td>Bitcoin RPC cookie file path</td><td>None</td></tr><tr><td><code>--bitcoin-rpc-password</code></td><td>Bitcoin RPC password</td><td>None</td></tr><tr><td><code>--bitcoin-rpc-url</code></td><td>Bitcoin RPC URL</td><td>Bitcoin core default URL based on the specified <code>--chain</code> e.g.<a href="http://127.0.0.1:8332"><code>http://127.0.0.1:8332</code></a> for mainnet</td></tr><tr><td><code>--bitcoin-rpc-user</code></td><td>Bitcoin RPC user</td><td>None</td></tr><tr><td><code>--block-index</code></td><td>Enable block indexing</td><td><code>false</code></td></tr><tr><td><code>--chain</code></td><td>Network to use</td><td>None (Required)</td></tr><tr><td><code>--config</code></td><td>Path to a configuration file</td><td>None</td></tr><tr><td><code>--data-dir</code></td><td>Custom data directory to store spaced state</td><td>None</td></tr><tr><td><code>--jobs</code></td><td>Number of concurrent workers during sync</td><td><code>8</code></td></tr><tr><td><code>--rpc-bind</code></td><td>Bind address for JSON-RPC connections</td><td><code>127.0.0.1, ::1</code></td></tr><tr><td><code>--rpc-port</code></td><td>Port for JSON-RPC connections</td><td>None</td></tr></tbody></table>
