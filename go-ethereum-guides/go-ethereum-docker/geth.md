## Ethereum tools in ethereum/go-ethereum repository

| Command | Description |
|--|--|
| **`geth`**    | Our main Ethereum CLI client. It is the entry point into the Ethereum network (main-, test- or private net), capable of running as a full node (default), archive node (retaining all historical state) or a light node (retrieving data live). It can be used by other processes as a gateway into the Ethereum network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports. `geth --help` and the [CLI Wiki page](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options) for command line options. |
| `abigen` | Source code generator to convert Ethereum contract definitions into easy to use, compile-time type-safe Go packages. It operates on plain [Ethereum contract ABIs](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI) with expanded functionality if the contract bytecode is also available. However, it also accepts Solidity source files, making development much more streamlined. Please see our [Native DApps](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go-bindings-to-Ethereum-contracts) wiki page for details. |
| `bootnode` | Stripped down version of our Ethereum client implementation that only takes part in the network node discovery protocol, but does not run any of the higher level application protocols. It can be used as a lightweight bootstrap node to aid in finding peers in private networks. |
| `evm` | Developer utility version of the EVM (Ethereum Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow isolated, fine-grained debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug`). |
| `gethrpctest` | Developer utility tool to support our [ethereum/rpc-test](https://github.com/ethereum/rpc-tests) test suite which validates baseline conformity to the [Ethereum JSON RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC) specs. Please see the [test suite's readme](https://github.com/ethereum/rpc-tests/blob/master/README.md) for details. |
| `rlpdump` | Developer utility tool to convert binary RLP ([Recursive Length Prefix](https://github.com/ethereum/wiki/wiki/RLP)) dumps (data encoding used by the Ethereum protocol both network as well as consensus wise) to user-friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`). |
| `puppeth` | a CLI wizard that aids in creating a new Ethereum network. |

# geth

Running the geth starts an Ethereum client that functions as an Ethereum node by itself,
and can act as a gateway through which other applications can use to connect to an Ethereum network.

Starting geth with the `console` option starts a light, or "fast-sync", node,
and geth's interactive Javascript console that allows you to call the web3 Javascript DApp API
and the admin API,

You can connect to a running geth node on the default IPC endpoint (`$HOME/.ethereum/geth.ipc`, `/tmp/geth.ipc` when running a `--dev` node, or `$HOME/.ethereum/<testnet>/geth.ipc` when running a testnet nodoe e.g. running a Rinkeby node with `geth --rinkeby` will open a geth IPC endpoint on `$HOME/.ethereum/rinkeby/geth.ipc`)by running `geth attach`

You can also run `geth attach` on any geth IPC or JSON-RPC endpoint. For example, run:

- `geth attach ipc:/some/custom/path/geth.ipc` to connect to an IPC endpoint on a custom path.
- `geth attach http://192.168.1.2:8545` to connect to a JSON-RPC endpoint through HTTPS on `192.168.1.2`.
- `geth attach ws://192.168.1.2:8546` to connect to a JSON-RPC endpoint through WebSockets on `192.168.1.2`.

https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console

Running `docker ethereum/client-go geth` starts a "fast-sync" node by default.

## enabling management APIs over IPC and JSON-RPC

By default Geth enables all APIs over the IPC (ipc) interface and only the `db`, `eth`, `net` and `web3` APIs over the HTTP and WebSocket interfaces.
