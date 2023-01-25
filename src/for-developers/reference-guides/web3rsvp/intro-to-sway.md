# Introduction to Sway

Sway is a strongly-typed programming language inspired by Rust. Sway can be used to write and deploy smart contracts on the Fuel blockchain. It inherits Rust's performance, control, and safety to use in a blockchain virtual machine environment optimized for gas costs and contract safety. 

Sway is backed by a powerful compiler and toolchain that work to abstract away complexities and ensure that your code is working, safe, and performant. 

Part of what makes Sway so unique is the fantastic suite of tools surrounding it that help you turn a contract into a full-stack dapp:

- 📚 Sway Standard Library: A native library of helpful types and methods.
- 🧰 Forc: The Fuel toolbox that helps you build, deploy, and manage your Sway projects.
- 🧑‍🔧 Fuelup: The official Fuel toolchain manager helps you install and manage versions. 
- 🦀 Fuels Rust SDK: Test and interact with your Sway contract with Rust.
- ⚡ Fuels Typescript SDK: Test and interact with your Sway contract with TypeScript.
- 🔭 Fuel Indexer: Easily make your own indexer to organize and query on-chain data.

## Sway Program Types

- 💼 `Contracts`: A contract is a set of functions with persistent state that can be deployed on a blockchain. Once deployed, the contract lives on the blockchain and can never be changed or deleted. Anyone can access the state or call public functions without permission. 

- 📋 `Scripts`: A script is a function that gets compiled into bytecode and passed into a transaction to be executed. It cannot be deployed or called like a contract and cannot store persistent state.

- 🔐 `Predicates`: A predicate is a pure function that can return true or false, and is sent inside a transaction as bytecode and checked at transaction validity time. If it evaluates to false the transaction will not be processed, and no gas will be used. If it evaluates to true, any coins belonging to the address equal to the Merkle root of the predicate bytecode may be spent by the transaction.

- 📗 `Libraries`: A library is a set of shareable code that can be used within a contract, a script, or a predicate.