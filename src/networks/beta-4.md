# The `beta-4` testnet

The `beta-4` network is the latest Fuel testnet. It builds on the foundation of `beta-3`, enhancing public P2P access, allowing parallel predicate execution, and introducing a newly redesigned bridging approach that ensures security for full rollups.

Ethereum contracts (Sepolia):

"FuelMessagePortal": `0x457A5a9320d06118764c400163c441cb8551cfa2`

"FuelChainState": `0x053cDdc8D065dEB051584a5ae4DB45348be285c9`

"FuelERC20Gateway": `0x10530552f00079cfF07f3c6D541C651a667Cf41D`

Sepolia block explorer: [https://sepolia.etherscan.io/](https://sepolia.etherscan.io)

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [https://faucet-beta-4.fuel.network/](https://faucet-beta-4.fuel.network/).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-4 is available at [https://beta-4.fuel.network/playground](https://beta-4.fuel.network/playground).

🔍 Block explorer - A block explorer (still heavily in development) is available at [https://fuellabs.github.io/block-explorer-v2/](https://fuellabs.github.io/block-explorer-v2/). Be sure to select `beta-4` from the dropdown on the top right.

Join the [Fuel Labs Forum](https://forum.fuel.network/) to get support from our team and others building on Fuel!

## SDK Versioning

Version 0.54.0 or higher is the recommended version of the TS SDK on `beta-4`.  

Version 0.47.0 or higher is the recommended version for the Rust SDK on `beta-4`.

## Toolchain Configuration

To configure the optimal toolchain for `beta-4`, ensure you have [fuelup](https://fuellabs.github.io/fuelup/latest) installed, then run the following command:

```shell
fuelup self update
```

Then install the beta-4 toolchain with

```shell
fuelup toolchain install beta-4
```

This installs the following components and versions:

- forc 0.44.0
- forc-explore 0.28.1
- forc-index 0.19.5
- forc-wallet 0.2.5
- fuel-core 0.20.4
- fuel-indexer 0.19.5

To set the `beta-4` toolchain as your default, run

```console
$ fuelup default beta-4
default toolchain set to 'beta-4-aarch64-apple-darwin'
```

## Predicate

Messages intended for contracts use a pre-defined predicate as the message recipient. This predicate allows anyone to relay the message to the target contract and only the target contract. Once the contract receives the message it can see who originated it along with any special message payload and processes it accordingly. Since anyone can relay the message using this predicate it opens up possibilities for automated message processing as a service.

The predicate root for the beta-4 testnet is `0x86a8f7487cb0d3faca1895173d5ff35c1e839bd2ab88657eede9933ea8988815`.
