# The `beta-1` testnet

The beta-1 network is the first public Fuel testnet, shared by all developers and users that interact with it. Developers may deploy contracts to it at will—no permission or whitelisting required—and users may interact with deployed contracts as well.

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [https://faucet-beta-1.fuel.network](https://faucet-beta-1.fuel.network).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-1 is available at [https://node-beta-1.fuel.network/playground](https://node-beta-1.fuel.network/playground).

🔍 Block explorer - A block explorer (still heavily in development) is available at [https://fuellabs.github.io/block-explorer-v2](https://fuellabs.github.io/block-explorer-v2).

Join the [Fuel Labs Discord](https://discord.com/invite/fuelnetwork) and head to the 🧪︱testnet-beta-1 channel to get support from our team. 

## Toolchain Configuration

To configure the optimal toolchain for `beta-1`, run the following command: `fuelup toolchain install beta-1.` Make sure you have `fuelup` installed on your system.

This installs the following components:
- forc 0.26.0
- forc-wallet 0.1.2
- fuel-core 0.10.1

To create a custom toolchain with the same set of versions run the following commands:

```console
$ fuelup toolchain new beta-1-custom-toolchain
$ fuelup component add forc@0.26.0
$ fuelup component add forc-wallet@0.1.2
$ fuelup component add fuel-core@0.10.1
```
