# Developer Quickstart

This guide will walk developers through writing a smart contract in Sway, a simple test, deploying to Fuel, and building a frontend. 

Before we begin, it may be helpful to understand terminology that will used throughout the docs and how they relate to each other:

- **Fuel**: the Fuel blockchain.
- **FuelVM**: the virtual machine powering Fuel.
- **Sway**: the domain-specific language crafted for the FuelVM; it is inspired by Rust.
- **Forc**: the build system and package manager for Sway, similar to Cargo for Rust.

## Understand Sway Program Types

There are four types of Sway programs:

- `contract`
- `predicate`
- `script`
- `library`

Contracts, predicates, and scripts can produce artifacts usable on the blockchain, while a library is simply a project designed for code reuse and is not directly deployable.

See [the chapter on program types](../sway-program-types/index.md) for more information.

## Your First Sway Project

We'll build a simple counter contract with two functions: one to increment the counter, and one to return the value of the counter.

A few pieces of info that will be helpful before moving on:

- The main features of a smart contract that differentiate it from scripts or predicates are that it is callable and stateful.
- A script is runnable bytecode on the chain which can call contracts to perform some task. It does not represent ownership of any resources and it cannot be called by a contract.

### Writing the Contract

First, let's [install the Sway toolchain](./installation.md). Then with `forc` installed, create a contract project:

```sh
forc new counter_dapp
```

Here is the project that Forc has initialized:

```console
$ cd counter_dapp
$ tree .
├── Cargo.toml
├── Forc.toml
├── src
│   └── main.sw
└── tests
    └── harness.rs
```

`Forc.toml` is the _manifest file_ (similar to `Cargo.toml` for Cargo or `package.json` for Node), and defines project metadata such as the project name and dependencies.

We'll be writing our code in the `src/main.sw`.

Open your project in a code editor and delete the boilerplate code in `src/main.sw`. 

Every Sway file must start with a declaration of what type of program the file contains; here, we've declared that this file is a contract.

```sway
contract;
```

Next, we'll define a our storage value. In our case, we have a single counter that we'll call `counter` of type 64-bit unsigned integer and initialize it to 0.

```sway
storage {
    counter: u64 = 0,
}
```

### ABI

An ABI defines an interface, and there is no function body in the ABI. A contract must either define or import an ABI declaration and implement it. It is considered best practice to define your ABI in a separate library and import it into your contract because this allows callers of the contract to import and use the ABI in scripts to call your contract.

For simplicity, we will define the ABI natively in the contract.

```sway
abi Counter {
    #[storage(read, write)]
    fn increment();

    #[storage(read)]
    fn counter() -> u64;
}
```

### Going line by line

`#[storage(read, write)]` is an annotation which denotes that this function has permission to read and write a value in storage.

`fn increment()` - We're introducing the functionality to increment and denoting it shouldn't return any value.

`#[storage(read)]` is an annotation which denotes that this function has permission to read values in storage.

`fn counter() -> u64;` - We're introducing the functionality to increment the counter and denoting the function's return value.

### Implement ABI

Below your ABI definition, you will write the implementation of the functions defined in your ABI.

```sway
impl Counter for Contract {
    #[storage(read)]
    fn counter() -> u64 {
      return storage.counter;
    }
    #[storage(read, write)]
    fn increment(){
        storage.counter = storage.counter + 1;
    }
}
```

> **Note**
> `return storage.counter;` is equivalent to `storage.counter`.

### What we just did

Read and return the counter property value from the contract storage.

```sway
fn counter() -> u64 {
    return storage.counter;
}
```

The function body accesses the value counter in storage, and increments the value by one. Then, we return the newly updated value of counter.

```sway
fn increment() {
    storage.counter = storage.counter + 1;
}
```

Here's what your code should look like so far: 

```sway
contract;

storage {
    counter: u64 = 0,
}

abi Counter {
    #[storage(read, write)]
    fn increment();

    #[storage(read)]
    fn counter() -> u64;
}

impl Counter for Contract {
    #[storage(read)]
    fn counter() -> u64 {
      return storage.counter;
    }
    #[storage(read, write)]
    fn increment(){
        storage.counter = storage.counter + 1;
    }
}
```


### Build the Contract

Build `counter_dapp` by running the following command in your terminal from inside the `counter_dapp` directory:

```sh
forc build
```

You should see something like this output:

```console
Compiled library "core".
  Compiled library "std".
  Compiled contract "counter_dapp".
  Bytecode size is 240 bytes.
```

## Testing your Contract

In the directory `tests`, navigate to `harness.rs.` Here you'll see there is some boilerplate code to help you start interacting with and testing your contract.

At the bottom of the file, define the body of `can_get_contract_instance`. Here is what your code should look like to verify that the value of the counter did get incremented:

```sway
#[tokio::test]
async fn can_get_contract_id() {
    let (instance, _id) = get_contract_instance().await;
    // Now you have an instance of your contract you can use to test each function
    instance.increment().call().await.unwrap();
    let result = instance.count().call().await.unwrap();
    assert!(result.value > 0); 
}
```

Run the following command in the terminal: `forc test`.

You'll see something like this as your output:

```console
 Compiled library "core".
  Compiled library "std".
  Compiled contract "counter_dapp".
  Bytecode size is 208 bytes.
  Compiling counter+dapp v0.1.0 (</path/to/counter_dapp>)
  Finished test [unoptimized + debuginfo] target(s) in 11.71s
  Running tests/harness.rs (target/debug/deps/integration_tests-6a600a6a87f48edb)

running 1 test
test can_get_contract_id ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.24s
```

## Deploy the Contract

It's now time to deploy the contract and call it on a Fuel node. We will show how to do this using `forc` from the command line, but you can also do it using the [Rust SDK](https://github.com/FuelLabs/fuels-rs#deploying-a-sway-contract) or the [TypeScript SDK](https://github.com/FuelLabs/fuels-ts/#deploying-contracts).

We'll be deploying to the Fuel testnet. In order to deploy a contract, you need to have a wallet to sign the transaction. You can optionally pass the flag `--unsigned` to deploy a contract without a signature. 

By default fuel-core runs without UTXO validation, which means you can run unsigned transactions. This allows you to send invalid inputs to emulate different conditions.

#### Install the Wallet CLI

Follow [these steps to set up a wallet to sign transactions](https://github.com/FuelLabs/forc-wallet#forc-wallet).

After typing in a password, be sure to save the mnemonic phrase that is output.

With this, you'll get a fuel address that looks something like this: `fuel1efz7lf36w9da9jekqzyuzqsfrqrlzwtt3j3clvemm6eru8fe9nvqj5kar8`. Save this address as you'll need it to sign transactions when we deploy the contract.

#### Deploy To Testnet

Now that you have a wallet, you can deploy with `forc deploy` and passing in the testnet endpoint like this:

`forc deploy --url https://node-beta-1.fuel.network/graphql --gas-price 1`

> Note: We set the gas price to 1. Without this flag, the gas price is 0 by default and the transaction will fail. 

The terminal will ask for the address of the wallet you want to sign this transaction with, paste in the address you saved earlier, it looks like this: `fuel1efz7lf36w9da9jekqzyuzqsfrqrlzwtt3j3clvemm6eru8fe9nvqj5kar8`

The terminal will output your contract id. Be sure to save this as you will need it to build a frontend with the Typescript SDK.

The terminal will output a `transaction id to sign` and prompt you for a signature. Open a new terminal tab and view your accounts by running `forc wallet list`. If you followed these steps, you'll notice you only have one account, `0`. 

Grab the `transaction id` from your other terminal and sign with a specified account by running the following command: 

`forc wallet sign` + `[transaction id here, without brackets]` + `[the account number, without brackets]`

Your command should look like this: `forc wallet sign 16d7a8f9d15cfba1bd000d3f99cd4077dfa1fce2a6de83887afc3f739d6c84df 0`

Enter your password when prompted, and you'll get back a `signature`. Save that signature, and return to your other terminal window, and paste that in where its prompting you to `provide a signature for this transaction`.

Finally, you will get back a `TransactionId` to confirm your contract was deployed. With this ID, you can head to the [block explorer](https://fuellabs.github.io/block-explorer-v2/) and view your contract. 

## Create a Frontend to Interact with Contract

Now we are going to;

1. **Initialize a React project.**
2. **Install the `fuels` SDK dependencies.**
3. **Modify the App.**
4. **Run our project.**

#### Initialize a React project.

To split better our project let's create a new folder `frontend` and initialize our project inside it.

Inside `counter-dapp` run;

```sh
npx create-react-app frontend --template typescript
```

The command will generate a react app using [`Create React App`](https://create-react-app.dev/).

#### Install the `fuels` SDK dependencies.

On this step we need to install 3 dependencies for the project:

1. `fuels`: The umbrella package that includes all the main tools; `Wallet`, `Contracts`, `Providers` and more.
2. `fuelchain`: Fuelchain is the ABI TypeScript generator.
3. `typechain-target-fuels`: The Fuelchain Target is the specific interpreter for the [Fuel ABI Spec](https://github.com/FuelLabs/fuel-specs/blob/master/specs/protocol/abi.md).

> ABI stands for Application Binary Interface. ABI's inform the application the interface to interact with the VM, in other words, they provide info to the APP such as what methods a contract has, what params, types it expects, etc...

##### Install

Inside `counter-dapp/frontend` run;

```sh
npm install fuels --save
npm install fuelchain typechain-target-fuels --save-dev
```

##### Generating contract types

To make it easier to interact with our contract we use `fuelchain` to interpret the output ABI JSON from our contract. This JSON was created on the moment we executed the `forc build` to compile our Sway Contract into binary. If you see the folder `counter_dapp/contract/out` you will be able to see the ABI JSON there. If you want to learn more, read the [ABI Specs here](https://github.com/FuelLabs/fuel-specs/blob/master/specs/protocol/abi.md).

Inside `counter-dapp/frontend` run;

```sh
npx fuelchain --target=fuels --out-dir=./src/contracts ../fuel-counter/out/debug/*-abi.json
```

You should see something like this:

```sh
Successfully generated 4 typings!
```

Now you should be able to find a new folder `counter-dapp/frontend/src/contracts`. This folder was auto-generated by our `fuelchain` command, this files abstract the work we would need to do to create a contract instance, and generate a complete TypeScript interface to the Contract, making easy to develop.

### Create A Wallet 

In the root of the frontend project create a file, createWallet.js

```js
const { Wallet } = require("fuels");

const wallet = Wallet.generate();

console.log("address", wallet.address.toString());
console.log("private key", wallet.privateKey);
```

In a terminal, run the following command: `node createWallet.js`. You should see an output that looks like this: 

```console
address fuel160ek8t7fzz89wzl595yz0rjrgj3xezjp6pujxzt2chn70jrdylus5apcuq
private key 0x719fb4da652f2bd4ad25ce04f4c2e491926605b40e5475a80551be68d57e0fcb
```

Copy the private key, you will need this later to set it as a string value for a variable `WALLET_SECRET` in your `App.tsx` file. More on that below. 

First, take the address of your wallet and use it to get some coins from [the testnet faucet](https://faucet-beta-1.fuel.network/).

Now you're ready to build and ship ⛽

> Note: The team is working to simplify the process of creating a wallet, and eliminate the need to create a wallet twice. Keep an eye out for these updates.

### Modify the App.

Inside the `frontend` folder let's add code that interacts with our contract.
Read the comments to help you understand the App parts.

Change the file `counter-dapp/frontend/src/App.tsx` to:

```ts
import React, { useEffect, useState } from "react";
import { Wallet } from "fuels";
import "./App.css";
// Import the contract factory -- you can find the name in index.ts.
// You can also do command + space and the compiler will suggest the correct name.
import { CounterDappAbi__factory } from "./contracts";
// The address of the contract deployed the Fuel testnet
const CONTRACT_ID = "0xaf94c0a707756caae667ee43ca18bace441b25998c668010192444a19674dc4f";
//the private key from createWallet.js
const WALLET_SECRET = "0x719fb4da652f2bd4ad25ce04f4c2e491926605b40e5475a80551be68d57e0fcb";
//pass in the testnet node
const wallet = new Wallet(WALLET_SECRET, "https://node-beta-1.fuel.network/graphql");
// Connects out Contract instance to the deployed contract
// address using the given wallet.
const contract = CounterDappAbi__factory.connect(CONTRACT_ID, wallet);

function App() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    async function main() {
      // Executes the counter function to query the current contract state
      // the `.get()` is read-only, because of this it don't expand coins.
      const { value } = await contract.functions.counter().get();
      setCounter(Number(value));
    }
    main();
  }, []);

  async function increment() {
    // Creates a transactions to call the increment function passing the amount
    // we want to increment, because it creates a TX and updates the contract state
    // this requires the wallet to have enough coins to cover the costs and also
    // to sign the Transaction
    await contract.functions.increment().txParams({gasPrice:1}).call();
    const { value } = await contract.functions.counter().get();
    setCounter(Number(value));
  }

  return (
    <div className="App">
      <header className="App-header">
        <p>Counter: {counter}</p>
        <button onClick={increment}>Increment</button>
      </header>
    </div>
  );
}

export default App;
```

### 4. Run your project.

Now it's time to have fun run the project on your browser;

Inside `counter-dapp/frontend` run;

```sh
npm start
```

![screenshot of the UI](./images/quickstart-dapp-screenshot.png)

#### ✨⛽✨ Congrats you have completed your first DApp on Fuel ✨⛽✨

Tweet us [@fuellabs_](https://twitter.com/fuellabs_) letting us know you just built a dapp on Fuel, you might get invited to a private group of builders or something 👀

### Updating The Contract

If you make changes to your contract, here are the steps you should take to get your frontend and contract back in sync:

- In your contract directory, run `forc build`
- In your contract directory, redeploy the contract by running this command and following the same steps as above to sign the transaction with your wallet: `forc deploy --url https://node-beta-1.fuel.network/graphql --gas-price 1`
- In your frontend directory, re-run this command: `npx fuelchain --target=fuels --out-dir=./src/contracts ../fuel-counter/out/debug/*-abi.json`
- In your frontend directory, update the contract ID in your `App.tsx` file

## Need Help?

Head over to the [Fuel discord](https://discord.com/invite/fuelnetwork) to get help.