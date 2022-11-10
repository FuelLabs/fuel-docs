# The `beta-2` testnet

The `beta-2` network is launched with a bridge to Ethereum's Goerli test network. With this network, developers are able to build and test cross-chain dapps, laying the foundations for projects building on Fuel to tap into Ethereum's massive liquidity and existing user base. Read more about `beta-2` [here.](./beta-2.md)

Ethereum contracts (Goerli):
"FuelMessagePortal": `0x4c7181fff2053D232Fc74Ff165b83FE8Dcb65910`,
"BinaryMerkleTreeLib": `0x40Ab53ba8BEEe302539f417eCC6C5FBb99F3B7Cd`,
"FuelSidechainConsensus": `0xF712e555ce59858f680890DA7dc2B51eD048580d`,
"L1ERC20Gateway": `0x96c53cd98B7297564716a8f2E1de2C83928Af2fe`

Fuel contract address:

Bridge contract ID:

Goerli block explorer: [https://goerli.etherscan.io/](https://goerli.etherscan.io/)

🚰 Faucet - Use the faucet to get test ETH to deploy contracts with or to interact with contracts. Available here: [https://faucet-beta-2.fuel.network/](https://faucet-beta-2.fuel.network/).

📃 GraphQL endpoint - The Fuel Core node uses GraphQL instead of JSON RPC. A playground for the public GraphQL endpoint for beta-1 is available at [https://node-beta-2.fuel.network/playground](https://node-beta-2.fuel.network/playground).

🔍 Block explorer - A block explorer (still heavily in development) is available at [insert here](link-here).

Join the [Fuel Labs Discord](https://discord.com/invite/fuelnetwork) and head to the 🧪︱testnet-beta-2 channel to get support from our team.

## SDK Versioning

Version 0.17.0 is the recommended version of the TS SDK on `beta-1`.  

Version 0.30.0 is the recommended version for the Rusk SDK on `beta-2`, 

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

## Predicate

Messages intended for contracts use a pre-defined predicate as the message recipient. This predicate allows anyone to relay the message to the target contract and only the target contract. Once the contract receives the message it can see who originated it along with any special message payload and processes it accordingly. Since anyone can relay the message using this predicate it opens up possibilities for automated message processing as a service. 

The predicate root is `0xc453f2ed45abb180e0a17aa88e78941eb8169c5f949ee218b45afcb0cfd2c0a8`.
