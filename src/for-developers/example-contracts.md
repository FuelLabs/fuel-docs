# Sway Application Examples

[sway-applications](https://github.com/FuelLabs/sway-applications) contains fullstack-applications and smart contracts built with Sway. These examples serve as building blocks for developers and demonstrate common use-cases, patterns, and functionality that can be implemented using Sway.

> Each project above is independent of the other projects and thus every project has its own instructions and files required to run it.

## Example Applications

* [Airdrop](https://github.com/FuelLabs/sway-applications/blob/master/airdrop) is a token distribution program where users are able to claim tokens given a valid merkle proof.
* [Automated Market Maker (AMM)](https://github.com/FuelLabs/sway-applications/blob/master/AMM) is a decentralized exchange protocol that manages liquidity pools supplied by its users and determines prices algorithmically while exchanging assets.
* [Decentralized Autonomous Organization (DAO)](https://github.com/FuelLabs/sway-applications/blob/master/DAO) is an organization where users get to vote on governance proposals using governance tokens.
* [English Auction](https://github.com/FuelLabs/sway-applications/blob/master/auctions/english-auction) is an auction where users bid up the price of an asset until the bidding period has ended or a reserve has been met.
* [Escrow](https://github.com/FuelLabs/sway-applications/blob/master/escrow) is a third party that keeps an asset on behalf of multiple parties.
* [Fundraiser](https://github.com/FuelLabs/sway-applications/blob/master/fundraiser) is a program allowing users to pledge towards a goal.
* [Frational-NFT](https://github.com/FuelLabs/sway-applications/blob/master/fractional-NFT) allows multiple parties to claim ownership of an NFT directly proportional to the number of tokens they hold.
* [Multi-Signature Wallet](https://github.com/FuelLabs/sway-applications/blob/master/multisig-wallet) is a wallet that requires multiple signatures to execute a transaction.
* [Name-Registry](https://github.com/FuelLabs/sway-applications/blob/master/name-registry) allows users to perform transactions with human readable names instead of addresses.
* [Non-Fungible Token (NFT)](https://github.com/FuelLabs/sway-applications/blob/master/NFT) is a token contract which provides unique collectibles, identified and differentiated by token IDs, where tokens contain metadata giving them distinctive characteristics.
* [Oracle](https://github.com/FuelLabs/sway-applications/blob/master/oracle) is a smart contract that provides off-chain data to on-chain applications.
* [OTC Swap Predicate](https://github.com/FuelLabs/sway-applications/blob/master/OTC-swap-predicate) is a predicate that can be used to propose and execute an atomic swap between two parties without requiring any on-chain state.

> Please note that each project has a `fuel-toolchain.toml` file that contains all the dependencies required to run it and automatically overrides toolchain dependencies installed on your local machine. This means that you can run the project without having to manually install or update your toolchain.
