# The `beta-4` testnet

The `beta-4` network is the latest Fuel testnet. It expands on the features of `beta-4`, introducing TODO.

Ethereum contracts (Goerli):

"FuelMessagePortal": 

"FuelChainConsensus": 

"FuelERC20Gateway": 

The ERC-20 Gateway contract on Georli Ethereum is at 

Goerli block explorer: [https://goerli.etherscan.io/](https://goerli.etherscan.io/)

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [https://faucet-beta-4.fuel.network/](https://faucet-beta-4.fuel.network/).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-4 is available at [https://beta-4.fuel.network/playground](https://beta-4.fuel.network/playground).

🔍 Block explorer - A block explorer (still heavily in development) is available at [https://fuellabs.github.io/block-explorer-v2/](https://fuellabs.github.io/block-explorer-v2/). Be sure to select `beta-4` from the dropdown on the top right.

Join the [Fuel Labs Discord](https://discord.com/invite/fuelnetwork) and head to the 🧪︱testnet channel to get support from our team.

## SDK Versioning

Version 0.49.0 or higher is the recommended version of the TS SDK on `beta-4`.  

Version 0.44.0 or higher is the recommended version for the Rusk SDK on `beta-4`.

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

forc : 0.42.1
    - forc-client
      - forc-deploy : 0.42.1
      - forc-run : 0.42.1
    - forc-doc : 0.42.1
    - forc-explore : 0.28.1
    - forc-fmt : 0.42.1
    - forc-index : 0.18.5
    - forc-lsp : 0.42.1
    - forc-tx : 0.42.1
    - forc-wallet : 0.2.4
  fuel-core : 0.18.3
  fuel-indexer : 0.18.5

To set the `beta-4` toolchain as your default, run

```console
$ fuelup default beta-4
default toolchain set to 'beta-4-aarch64-apple-darwin'
```

## Predicate

Messages intended for contracts use a pre-defined predicate as the message recipient. This predicate allows anyone to relay the message to the target contract and only the target contract. Once the contract receives the message it can see who originated it along with any special message payload and processes it accordingly. Since anyone can relay the message using this predicate it opens up possibilities for automated message processing as a service.

The predicate root for the beta-4 testnet is 
