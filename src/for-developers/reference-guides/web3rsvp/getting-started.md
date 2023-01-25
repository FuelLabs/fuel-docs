# Getting Started

1. Install the [Rust toolchain](https://www.rust-lang.org/tools/install). To download `Rustup` and install `Rust`, run the following in your terminal, then follow the on-screen instructions:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

2. Install the [Fuel toolchain](https://github.com/FuelLabs/fuelup) by installing `fuelup`, the `rustup` equivalent for Fuel, that lets you download binary releases of the Fuel toolchain as follows:

```bash
curl --proto '=https' --tlsv1.2 -sSf \ https://fuellabs.github.io/fuelup/fuelup-init.sh | sh
```

3. Install the beta-2 toolchain distribution and set it as your default by running the following:

```bash
fuelup toolchain install beta-2
fuelup default beta-2
```

4. Check the current toolchain version installed by running the following:

```bash
fuelup show
```

It will show an output similar to the following:

```bash
Default host: aarch64-apple-darwin
fuelup home: $home/.fuelup

installed toolchains
--------------------
beta-2-aarch64-apple-darwin (default)

active toolchain
----------------
beta-2-aarch64-apple-darwin (default)
  forc : 0.33.1
    - forc-client
      - forc-deploy : 0.33.1
      - forc-run : 0.33.1
    - forc-doc : 0.33.1
    - forc-explore - not found
    - forc-fmt : 0.33.1
    - forc-index - not found
    - forc-lsp : 0.33.1
    - forc-wallet : 0.1.2
```

5. Finally, add the [Sway extension](https://marketplace.visualstudio.com/items?itemName=FuelLabs.sway-vscode-plugin) to your VS Code.