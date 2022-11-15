# The `beta-1` testnet

The beta-1 network is the first public Fuel testnet, shared by all developers and users that interact with it. Developers may deploy contracts to it at will—no permission or whitelisting required—and users may interact with deployed contracts as well.

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [https://faucet-beta-1.fuel.network](https://faucet-beta-1.fuel.network).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-1 is available at [https://node-beta-1.fuel.network/playground](https://node-beta-1.fuel.network/playground).

🔍 Block explorer - A block explorer (still heavily in development) is available at [https://fuellabs.github.io/block-explorer-v2](https://fuellabs.github.io/block-explorer-v2).

Join the [Fuel Labs Discord](https://discord.com/invite/fuelnetwork) and head to the 🧪︱testnet-beta-1 channel to get support from our team.

## SDK Versioning

Version 0.17.0 is the recommended version of the TS SDK on `beta-1`.

Version 0.30.0 is the recommended version for the Rusk SDK on `beta-1`,  

## Toolchain Configuration

To configure the optimal toolchain for `beta-1`, ensure you have [fuelup](https://fuellabs.github.io/fuelup/v0.12.0/) installed, then run the following command:

```console
$ fuelup toolchain install beta-1
Downloading: forc forc-wallet fuel-core

Adding component forc v0.26.0 to 'beta-1-aarch64-apple-darwin'
Fetching binary from https://github.com/FuelLabs/sway/releases/download/v0.26.0/forc-binaries-darwin_arm64.tar.gz
Unpacking and moving forc to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Unpacking and moving forc-deploy to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Unpacking and moving forc-run to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Unpacking and moving forc-explore to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Unpacking and moving forc-lsp to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Unpacking and moving forc-fmt to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Fetching core forc dependencies
Installed forc v0.26.0 for toolchain 'beta-1-aarch64-apple-darwin'

Adding component forc-wallet v0.1.2 to 'beta-1-aarch64-apple-darwin'
Fetching binary from https://github.com/FuelLabs/forc-wallet/releases/download/v0.1.2/forc-wallet-0.1.2-aarch64-apple-darwin.tar.gz
Unpacking and moving forc-wallet to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Installed forc-wallet v0.1.2 for toolchain 'beta-1-aarch64-apple-darwin'

Adding component fuel-core v0.10.1 to 'beta-1-aarch64-apple-darwin'
Fetching binary from https://github.com/FuelLabs/fuel-core/releases/download/v0.10.1/fuel-core-0.10.1-aarch64-apple-darwin.tar.gz
Unpacking and moving fuel-core to /Users/sarah/.fuelup/toolchains/beta-1-aarch64-apple-darwin/bin
Installed fuel-core v0.10.1 for toolchain 'beta-1-aarch64-apple-darwin'

Installed:
- forc 0.26.0
- forc-wallet 0.1.2
- fuel-core 0.10.1


The Fuel toolchain is installed and up to date
```

This installs the following components:

- forc 0.26.0
- forc-wallet 0.1.2
- fuel-core 0.10.1
