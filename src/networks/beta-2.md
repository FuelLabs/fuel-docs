# The `beta-2` testnet

The `beta-2` network is launched with a bridge to Ethereum's Goerli test network. With this network, developers are able to build and test cross-chain dapps, laying the foundations for projects building on Fuel to tap into Ethereum's massive liquidity and existing user base. Read more about `beta-2` [here.](./beta-2.md)

Ethereum contract address:

Fuel contract address:

Bridge contract ID:

Goerli block explorer:

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [insert here](add-link).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-1 is available at [insert here](link-here).

🔍 Block explorer - A block explorer (still heavily in development) is available at [insert here](link-here).

Join the [Fuel Labs Discord](https://discord.com/invite/fuelnetwork) and head to the 🧪︱testnet-beta-1 channel to get support from our team. 

## Toolchain Configuration

To configure the optimal toolchain for `beta-2`, run the following command: `fuelup toolchain install beta-2.` Make sure you have `fuelup` installed on your system.

This installs the following components:
- forc
- forc-wallet
- fuel-core

To create a custom toolchain with the same set of versions run the following commands:

```console
$ fuelup toolchain new beta-1-custom-toolchain
$ fuelup component add
$ fuelup component add
$ fuelup component add
```
