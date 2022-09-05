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

### Deploy the Contract

It's now time to deploy the contract and call it on a Fuel node. We will show how to do this using `forc` from the command line, but you can also do it using the [Rust SDK](https://github.com/FuelLabs/fuels-rs#deploying-a-sway-contract) or the [TypeScript SDK](https://github.com/FuelLabs/fuels-ts/#deploying-contracts).

## Testing your Contract

In the directory `tests`, navigate to `harness.rs.` Here you'll see there is some boilerplate code to help you start interacting with and testing your contract.

At the bottom of the file, define the body of `can_get_contract_instance`. Here is what your code should look like to verify that the value of the counter did get incremented:

```sway
#[tokio::test]
async fn can_get_contract_id() {
    let (instance, _id) = get_contract_instance().await;
    // Now you have an instance of your contract you can use to test each function
    let result = instance.increment().call().await.unwrap();
    assert!(result.value > 0)
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
npx fuelchain --target=fuels --out-dir=./src/contracts ../counter-dapp/out/debug/*-abi.json
```

You should see something like this:

```sh
Successfully generated 4 typings!
```

Now you should be able to find a new folder `counter-dapp/frontend/src/contracts`. This folder was auto-generated by our `fuelchain` command, this files abstract the work we would need to do, to create a contract instance, and also generate a complete TypeScript interface to the Contract, making easy to develop.

### Modify the App.

Inside the `frontend` folder let's add code that interacts with our contract.
Read the comments to help you understand the App parts.

Change the file `counter-dapp/frontend/src/App.tsx` to:

```ts
import React, { useEffect, useState } from "react";
import { Wallet } from "fuels";
import "./App.css";
// Import the contract factory from the generated folder
// from the fuelchain command
import { ContractAbi__factory } from "./contracts";

// Se the Wallet Secret used on the chainConfig.json
// this enables us to have a account with initial balance
const WALLET_SECRET =
  "0xa449b1ffee0e2205fa924c6740cc48b3b473aa28587df6dab12abc245d1f5298";
// The address of the contract deployed to our local node
// the contract id is output right after the forc deploy command
// Ex.: Contract id: 0xa326e3472fd4abc417ba43e369f59ea44f8325d42ba6cf71ec4b58123fd8668a
// const CONTRACT_ID = "0xa326e3472fd4abc417ba43e369f59ea44f8325d42ba6cf71ec4b58123fd8668a"
const CONTRACT_ID =
  "<replace this with the contract id displayed on forc deploy>";
// Create a Wallet from given secretKey in this case
// The one we configured at the chainConfig.json
const wallet = new Wallet(WALLET_SECRET);
// Connects out Contract instance to the deployed contract
// address using the given wallet.
const contract = ContractAbi__factory.connect(CONTRACT_ID, wallet);

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
    const { value } = await contract.functions.increment(1).call();
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

#### ✨⛽✨ Congrats you have completed your first DApp on Fuel ✨⛽✨