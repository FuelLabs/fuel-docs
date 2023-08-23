# Writing A Sway Smart Contract

## Installation

<!-- This example should include the instructions for installing Rust & Fuelup-->
<!-- installation:example:start -->
Start by [installing the Rust toolchain](https://www.rust-lang.org/tools/install).

Then, [install the Fuel toolchain](https://github.com/FuelLabs/fuelup).
<!-- installation:example:end -->

<!-- This example should include the instructions for installing the latest toolchain-->
<!-- toolchain_installation:example:start -->
Make sure you have the latest version of `fuelup` by running the following command:
 
```console
fuelup self update
```

```console
Fetching binary from https://github.com/FuelLabs/fuelup/releases/download/v0.19.4/fuelup-0.19.4-aarch64-apple-darwin.tar.gz
Downloading component fuelup without verifying checksum
Unpacking and moving fuelup to /var/folders/tp/0l8zdx9j4s9_n609ykwxl0qw0000gn/T/.tmpiNJQHt
Moving /var/folders/tp/0l8zdx9j4s9_n609ykwxl0qw0000gn/T/.tmpiNJQHt/fuelup to /Users/.fuelup/bin/fuelup
```

Then run `fuelup toolchain install beta-4` to install the `beta-4` toolchain.

Finally, set the `beta-4` toolchain as your default distribution with the following command:

```console
fuelup default beta-4
```

```console
default toolchain set to 'beta-4'
```

You can check your current toolchain anytime by running `fuelup show`.
<!-- toolchain_installation:example:end -->

> Having problems with this part? Post your question on our forum [https://forum.fuel.network/](https://forum.fuel.network/). To help you as efficiently as possible, include the output of this command in your post: `fuelup show.`

## Your First Sway Project

We'll build a simple counter contract with two functions: one to increment the counter, and one to return the value of the counter.

**Start by creating a new, empty folder. We'll call it `fuel-project`.**

### Writing the Contract

Move inside of your `fuel-project` folder:

```sh
cd fuel-project
```

Then create a contract project using forc:

```sh
forc new counter-contract
```

You will get this output:

```sh
To compile, use `forc build`, and to run tests use `forc test`
----

Read the Docs:
- Sway Book: https://fuellabs.github.io/sway/lates
- Rust SDK Book: https://fuellabs.github.io/fuels-rs/latest
- TypeScript SDK: https://fuellabs.github.io/fuels-ts/

Join the Community:
- Follow us @SwayLang: https://twitter.com/SwayLang
- Ask questions on Discourse: https://forum.fuel.network/

Report Bugs:
- Sway Issues: https://github.com/FuelLabs/sway/issues/new
```

<!-- This example should include a tree for a new forc project and explain the boilerplate files-->
<!-- forc_new:example:start -->
Here is the project that `Forc` has initialized:

```console
$ tree counter-contract
counter-contract
├── Forc.toml
└── src
    └── main.sw
```

`Forc.toml` is the _manifest file_ (similar to `Cargo.toml` for Cargo or `package.json` for Node) and defines project metadata such as the project name and dependencies.
<!-- forc_new:example:end -->

Open your project in a code editor and delete everything in `src/main.sw` apart from the first line.

Every Sway file must start with a declaration of what type of program the file contains; here, we've declared that this file is a contract.

```sway
{{#include ../../quickstart-example/counter-contract/src/main.sw:contract}}
```

Next, we'll define a storage value. In our case, we have a single counter that we'll call `counter` of type 64-bit unsigned integer and initialize it to 0.

```sway
{{#include ../../quickstart-example/counter-contract/src/main.sw:storage}}
```

### ABI

An ABI defines an interface, and there is no function body in the ABI. A contract must either define or import an ABI declaration and implement it. It is considered best practice to define your ABI in a separate library and import it into your contract because this allows callers of the contract to import and use the ABI in scripts to call your contract.

For simplicity, we will define the ABI directly in the contract file itself.

```sway
{{#include ../../quickstart-example/counter-contract/src/main.sw:abi}}
```

### Implement ABI

Below your ABI definition, you will write the implementation of the functions defined in your ABI.

```sway
{{#include ../../quickstart-example/counter-contract/src/main.sw:counter-contract}}
```

> **Note**: `storage.counter` is an implicit return and is equivalent to `return storage.counter;`.

Here's what your code should look like so far:

File: `./counter-contract/src/main.sw`

```sway
{{#include ../../quickstart-example/counter-contract/src/main.sw:all}}
```

### Build the Contract

From inside the `fuel-project/counter-contract` directory, run the following command to build your contract:

```console
forc build
```

```console
  Compiled library "core".
  Compiled library "std".
  Compiled contract "counter-contract".
  Bytecode size is 232 bytes.
```

Let's have a look at the content of the `counter-contract` folder after building:

```console
$ tree .
.
├── Forc.lock
├── Forc.toml
├── out
│   └── debug
│       ├── counter-contract-abi.json
│       ├── counter-contract-storage_slots.json
│       └── counter-contract.bin
└── src
    └── main.sw
```

We now have an `out` directory that contains our build artifacts such as the JSON representation of our ABI and the contract bytecode.

## Testing your Contract

We will start by adding a Rust integration test harness using a Cargo generate template. If this is your first time going through this quickstart, you'll need to install the `cargo generate` command. In the future, you can skip this step as it will already be installed.

Navigate to your contract folder:

```console
cd counter-contract
```

```console
changed directory into `counter-countract`
```

Then run the installation command:

```console
cargo install cargo-generate
```

```console
 Updating crates.io index...
 installed package `cargo-generate v0.17.3`
```

> **Note**: You can learn more about cargo generate by visiting [its repository](https://github.com/cargo-generate/cargo-generate).

Now, let's generate the default test harness with the following:

```console
cargo generate --init fuellabs/sway templates/sway-test-rs --name counter-contract
```

```console
⚠️   Favorite `fuellabs/sway` not found in config, using it as a git repository: https://github.com/fuellabs/sway.git
🔧   Destination: /home/user/path/to/counter-contract ...
🔧   project-name: counter-contract ...
🔧   Generating template ...
🔧   Moving generated files into: `/home/user/path/to/counter-contract`...
✨   Done! New project created /home/user/path/to/counter-contract
```

Let's have a look at the result:

```console
$ tree .
.
├── Cargo.toml
├── Forc.lock
├── Forc.toml
├── out
│   └── debug
│       ├── counter-contract-abi.json
│       ├── counter-contract-storage_slots.json
│       └── counter-contract.bin
├── src
│   └── main.sw
└── tests
    └── harness.rs
```

We have two new files!

- The `Cargo.toml` is the manifest for our new test harness and specifies the required dependencies including `fuels` the Fuel Rust SDK.
- The `tests/harness.rs` contains some boilerplate test code to get us started, though doesn't call any contract methods just yet.

Now that we have our default test harness, let's add some useful tests to it.

At the bottom of `test/harness.rs`, define the body of `can_get_contract_id()`. Here is what your code should look like to verify that the value of the counter did get incremented:

File: `tests/harness.rs`

```sway
{{#include ../../quickstart-example/counter-contract/tests/harness.rs:contract-test}}
```

Run `cargo test` in the terminal:

```console
cargo test
```

If all goes well, the output should look as follows:

```console
  ...
  running 1 test
  test can_get_contract_id ... ok
  test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.11s
```

## Deploy the Contract

It's now time to deploy the contract to the testnet. We will show how to do this using `forc` from the command line, but you can also do it using the [Rust SDK](https://github.com/FuelLabs/fuels-rs#deploying-a-sway-contract) or the [TypeScript SDK](https://github.com/FuelLabs/fuels-ts/#deploying-contracts).

In order to deploy a contract, you need to have a wallet to sign the transaction and coins to pay for gas. First, we'll create a wallet.

### Install the Wallet CLI

👉 Follow [these steps to set up a wallet and create an account](https://github.com/FuelLabs/forc-wallet#forc-wallet).

After typing in a password, be sure to save the mnemonic phrase that is output.

With this, you'll get a fuel address that looks something like this: `fuel1efz7lf36w9da9jekqzyuzqsfrqrlzwtt3j3clvemm6eru8fe9nvqj5kar8`. Save this address as you'll need it to sign transactions when we deploy the contract.

### Get Testnet Coins

With your account address in hand, head to the [testnet faucet](https://faucet-beta-4.fuel.network/) to get some coins sent to your wallet.

### Deploy To Testnet

Now that you have a wallet, you can deploy with `forc deploy` and adding the `--testnet` to target the latest network:

```console
forc deploy --testnet
```


The terminal will ask for the password of the wallet: 

`Please provide the password of your encrypted wallet vault at "~/.fuel/wallets/.wallet": `


Once you unlocked the wallet, the terminal will show a list of the accounts of this wallet:

```console
Account 0 -- fuel18caanqmumttfnm8qp0eq7u9yluydxtqmzuaqtzdjlsww5t2jmg9skutn8n:
  Asset ID                                                           Amount
  0000000000000000000000000000000000000000000000000000000000000000 499999940
```

Just below the list, you'll see this prompt:

`Please provide the index of account to use for signing:`

Then you'll enter the number of the account of preference and press `Y` when prompted to accept the transaction.

Finally, you will get back the network endpoint where the contract was dpeloyed, a `ContractId` and the block where the transaction was signed. To confirm your contract was deployed. With this ID, you can head to the [block explorer](https://fuellabs.github.io/block-explorer-v2/) and view your contract.

```console
Contract deploy-to-beta-4 Deployed!

Network: https://beta-4.fuel.network
Contract ID: 0x8342d413de2a678245d9ee39f020795800c7e6a4ac5ff7daae275f533dc05e08
Deployed in block 0x4ea52b6652836c499e44b7e42f7c22d1ed1f03cf90a1d94cd0113b9023dfa636
```

![block explorer](../images/block-explorer.png)

### Congrats, you have completed your first smart contract on Fuel ⛽

[Here is the repo for this project](https://github.com/FuelLabs/quickstart). If you run into any problems, a good first step is to compare your code to this repo and resolve any differences.

Tweet us [@fuel_network](https://twitter.com/fuel_network) letting us know you just built a dapp on Fuel, you might get invited to a private group of builders, be invited to the next Fuel dinner, get alpha on the project, or something 👀.

## Need Help?

Get help from the team by posting your question in the [Fuel Forum](https://forum.fuel.network/).
