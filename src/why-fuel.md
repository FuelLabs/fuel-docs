# Why Fuel?

Fuel is the fastest modular execution layer, delivering the highest security and flexible throughput while delivering a superior developer experience.

There are three main pillars to Fuel’s technology stack:

1. Parallel transaction execution 
2. The Fuel Virtual Machine (FuelVM)
3. The Sway programming language - a Rust-based DSL built specifically for writing smart contracts and its Forc toolchain.

## Parallel Transaction Execution

Fuel delivers unmatched processing capacity through its ability to execute transactions in parallel by using strict state access lists as a UTXO model. This enables Fuel to use far more threads and cores of your CPU that are typically idle in single-threaded blockchains. 

## FuelVM

FuelVM is a new VM designed from scratch for high computational bandwidth. It carries traits from WASM, EVM, and Solana’s SeaLevel.

## The Sway Language 

Sway is a language developed for the Fuel blockchain. It is heavily inspired by Rust and aims to bring modern language development and performance to the blockchain ecosystem. Sway was created alongside the FuelVM and designed for the high-compute Fuel environment.

Our development environment retains the benefits of smart contract languages like Solidity, while adopting the paradigms introduced in the Rust tooling ecosystem. Now, developers can have a completely vertically-integrated experience where every component from the virtual machine through to the CLI works in harmony.

## Built for Modular 

Modular blockchains offer more scalability by decoupling the core functions of a blockchain without compromising on security.  L2s on monolithic L1s in addition to new monolithic layer-1s are becoming outdated technologies. They were not designed to handle the massive data throughput modular systems provide. Designs will be more future-proof as bridging liquidity between modular execution layers will be trust minimized and not succumb to the faint of many layer-1 bridges.

As we move away from monolithic designs, we’re moving towards a modular future, where execution is separated from data availability and consensus (e.g. tomorrow's Eth2, or Celestia).

Fuel is built specifically *for* this new, open, modular stack.
