# The Fuel Toolchain

The full stack of tools designed and blessed by FuelLabs for enabling/assisting the Fuel application development experience.

Tooling Overview:
- `fuel-core`: The Fuel VM node client.
- `forc`: The Fuel Orchestrator. Includes Sway compiler, packaging and plugin support.

Official forc plugins:
- `forc-fmt`: The Sway code formatter.
- `forc-lsp`: The Sway Language Server Protocol implementation.
- `forc-explore`: Runs the Fuel block explorer.
- `forc-client`: For deploying and running Sway apps via CLI.
- `fuelup`: The FuelLabs toolchain manager - an easy approach to retrieving all of the above.

For more specific documentation on the toolchain, check out the Sway docs [here](https://fuellabs.github.io/sway/v0.19.2/introduction/sway-toolchain.html).

## Building on Fuel

Developers can get everything they need to start creating Sway applications for the Fuel VM with a single toolchain, blessed by the same teams that create the FuelVM and Sway language.

We take a curated, “batteries-included”-yet-modular approach to providing tooling. Other tools for Ethereum development are built on top of the existing EVM, and the languages (Yul, Solidity, etc.) are built by different teams than the tooling (ganache, hard hat, foundry). In contrast, Fuel tooling has the benefit of being tightly coupled with the compiler, FuelVM, SDK, etc..This allows for tighter integration and a more unified development environment that can talk with the other parts of the fuel ecosystem. One example is in the VS Code plugin, where the language server interacts directly with the sway compiler. You can also spin up a local instance of fuel-core and, in the future, interact with your smart contracts via the type script SDK, all from the plugin.
