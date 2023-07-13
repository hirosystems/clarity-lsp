                      
       /     /   ▶ Hord   
      / --- /      Ordinal indexing engine based on Chainhook.
     /     /       Build indexes, standards and protocols on top of Ordinals and Inscriptions (BRC20, etc).
                  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Introduction](https://img.shields.io/badge/%23-%20Introduction%20-orange?labelColor=gray)](#Introduction)
&nbsp;&nbsp;&nbsp;&nbsp;[![Features](https://img.shields.io/badge/%23-Features-orange?labelColor=gray)](#Features)
&nbsp;&nbsp;&nbsp;&nbsp;[![Getting started](https://img.shields.io/badge/%23-Quick%20Start-orange?labelColor=gray)](#Quick-start)
&nbsp;&nbsp;&nbsp;&nbsp;[![Bug reporting](https://img.shields.io/badge/%23-Documentation-orange?labelColor=gray)](#Bug-report)
&nbsp;&nbsp;&nbsp;&nbsp;[![Contribute](https://img.shields.io/badge/%23-Contribute-orange?labelColor=gray)](#Contribute)

***

## Introduction

The [Ordinal theory](https://trustmachines.co/glossary/ordinal-theory) is a protocol aiming at attributing unique identifiers to every single satoshis minted. Thanks to this numbering scheme, satoshis can be **inscribed** with arbitrary content (aka **inscriptions**), creating bitcoin-native digital artifacts more commonly known as NFTs. Inscriptions do not require a sidechain or separate token.
These inscribed sats can then be transferred using bitcoin transactions, sent to bitcoin addresses, and held in bitcoin UTXOs. These transactions, addresses, and UTXOs are normal bitcoin transactions, addresses, and UTXOS in all respects, with the exception that in order to send individual sats, transactions must control the order and value of inputs and outputs according to ordinal theory.

The [Chainhook SDK](https://github.com/hirosystems/chainhook) is a re-org aware transaction indexing engine for Stacks and Bitcoin. It helps developers extracting efficiently transactions from blocks, along with keeping a consistent view of the chainstate thanks to its event based architecture.

**hord** is an indexer designed to help developers building new re-org resistant applications on top of the Ordinal theory.

Thanks to `hord`, Bitcoin developers can reliably implenent and stack protocols leveraging ordinals inscriptions and transfers.

Constistent Indexers are crucial for the **Ordinal Theory**: indexers are to the **Ordinal Theory** what Smart contracts are to blockchains: they help developers creating new protocols and new applications.

---
<a name="Quick-start"></a>

## Install `hord`

### Install from source

```bash 
$ git clone https://github.com/hirosystems/hord.git
$ cd hord
$ cargo hord-install
```

### Guide to local Bitcoin testnet / mainnet predicate scanning

In order to scan the Bitcoin chain with a given predicate, a `bitcoind` instance with access to the RPC methods `getblockhash` and `getblock` must be accessible. The RPC calls latency will directly impact the speed of the scans.

*Note: the configuration of a `bitcoind` instance is out of scope for this guide.*

Assuming: 

`1)` a `bitcoind` node correctly configured and 

`2)` a local HTTP server running on port `3000` exposing a `POST /api/events` endpoint, 

Scans can be performed using the following command:
```bash
$ hord scan --http-post=http://localhost:3000/api/events --testnet
```
When using the flag `--testnet`, the scan operation will generate a configuration file in memory using the following settings:
```toml
[storage]
working_dir = "cache" # Directory used by chainhook node for caching data

[network]
mode = "testnet"
bitcoind_rpc_url = "http://0.0.0.0:18332"
bitcoind_rpc_username = "bitcoind_username"
bitcoind_rpc_password = "bitcoind_password"
bitcoind_zmq_url = "http://0.0.0.0:18543"
```

When using the flag `--mainnet`, the scan operation will generate a configuration file in memory using the following settings:
```toml
[storage]
working_dir = "cache"

[network]
mode = "testnet"
bitcoind_rpc_url = "http://0.0.0.0:8332"
bitcoind_rpc_username = "bitcoind_username"
bitcoind_rpc_password = "bitcoind_password"
bitcoind_zmq_url = "http://0.0.0.0:18543"
```

By passing the flag `--config=/path/to/config.toml`, developers can customize the credentials and network address of their Bitcoin node. 
```bash
$ hord config new --testnet
✔ Generated config file Hord.toml

$ hord scan --http-post=http://localhost:3000/api/events --config-path=./Hord.toml
```

---
## Run `hord` as a service for streaming new blocks

`hord` can be run as a background service for streaming and processing new canonical blocks appended to the Bitcoin and Stacks blockchains.

```bash
$ hord service start --http-post=http://localhost:3000/api/events --config-path=./path/to/config.toml
```

New `http-post` endpoints can also be added dynamically by spinning up a redis server and adding the following section in the `Hord.tonl` configuration file:

```toml
[http_api]
http_port = 20456
database_uri = "redis://localhost:6379/"
```

Running `hord` with the command

```bash
$ hord service start --config-path=./path/to/config.toml
```

will spin up a HTTP API for managing events destinations.


A comprehensive OpenAPI specification explaining how to interact with this HTTP REST API can be found [here](https://github.com/hirosystems/chainhook/blob/develop/docs/chainhook-openapi.json).

<a name="Bug-report"></a>
## Bugs and feature requests

If you encounter a bug or have a feature request, we encourage you to follow the steps below:

 1. **Search for existing issues:** Before submitting a new issue, please search [existing and closed issues](../../issues) to check if a similar problem or feature request has already been reported.
 1. **Open a new issue:** If it hasn't been addressed, please [open a new issue](../../issues/new/choose). Choose the appropriate issue template and provide as much detail as possible, including steps to reproduce the bug or a clear description of the requested feature.
 1. **Evaluation SLA:** Our team reads and evaluates all the issues and pull requests. We are avaliable Monday to Friday and we make a best effort to respond within 7 business days.

Please **do not** use the issue tracker for personal support requests or to ask for the status of a transaction. You'll find help at the [#support Discord channel](https://discord.gg/SK3DxdsP).


## Contribute

Development of this product happens in the open on GitHub, and we are grateful to the community for contributing bugfixes and improvements. Read below to learn how you can take part in improving the product.

### Code of Conduct
Please read our [Code of conduct](../../../.github/blob/main/CODE_OF_CONDUCT.md) since we expect project participants to adhere to it. 

### Contributing Guide
Read our [contributing guide](.github/CONTRIBUTING.md) to learn about our development process, how to propose bugfixes and improvements, and how to build and test your changes.


### Community

Join our community and stay connected with the latest updates and discussions:

- [Join our Discord community chat](https://discord.gg/ZQR6cyZC) to engage with other users, ask questions, and participate in discussions.

- [Visit hiro.so](https://www.hiro.so/) for updates and subcribing to the mailing list.

- Follow [Hiro on Twitter.](https://twitter.com/hirosystems)
