# Building a Frontend to Interact With Your Contract

To build out our frontend application, we'll do the following:

1. **Initialize a React project.**
2. **Install the `fuels` SDK dependencies.**
3. **Write our frontend code.**
4. **Run our project.**

## Initialize a React project

To split our project's contract from frontend code, let's create a new folder `frontend` where we'll initialize our frontend project.

In the terminal, go back up one directory and initialize a react project using [`Create React App`](https://create-react-app.dev/).

```console
$ cd ..
$ npx create-react-app frontend --template typescript
Success! Created frontend at Fuel/fuel-project/frontend
```

You should now have your outer folder, `fuel-project`, with two folders inside: `counter-contract` and `frontend`

![project folder structure](/../images/quickstart-folder-structure.png)

### Install the `fuels` SDK dependency

The `fuels` umbrella package includes all the main tools you need for your frontend; `Wallet`, `Contracts`, `Providers`, and more.

Also, it contains the routines for ABI TypeScript generation.

> ABI stands for Application Binary Interface. ABI's inform the application the interface to interact with the VM, in other words, they provide info to the APP such as what methods a contract has, what params, types it expects, etc...

### Installing

Move into the `frontend` folder, then run:

```console
$ cd frontend
$ npm install fuels@0.29.1 --save
added 114 packages, and audited 115 packages in 29s
```

### Generating contract types

To make it easier to interact with our contract we use `fuels typegen` command to interpret the output ABI JSON from our contract, and generate Typescript definitions based on it. This JSON was created when we executed the `forc build` command to compile our Sway Contract into binary.

If you see the folder `fuel-project/counter-contract/out` you will be able to see the ABI JSON there. If you want to learn more, read the [ABI spec](https://fuellabs.github.io/fuel-specs/master/protocol/abi).

Inside the `fuel-project/frontend` directory run:

```console
$ npx fuels typegen -i ../counter-contract/out/debug/*-abi.json -o ./src/contracts
Generating files..

 - src/contracts/CounterContractAbi.d.ts
 - src/contracts/factories/CounterContractAbi__factory.ts
 - src/contracts/index.ts

Done.⚡
```

Now you should be able to find a new folder `fuel-project/frontend/src/contracts`. This folder was auto-generated by our `fuels typegen` command, and these files abstract the work we would need to do to create a contract instance, and generate a complete TypeScript interface to the Contract, making easy to develop.

### Create A Wallet (Again)

For interacting with the fuel network we have to submit signed transactions with enough funds to cover network fees. The Fuel TS SDK doesn't currently support Wallet integrations, requiring us to have a non-safe wallet inside the WebApp using a privateKey.

> **Note**: This should be done only for development purpose. Never expose a web app with a private key inside. The Fuel Wallet is in active development, follow the progress [here](https://github.com/FuelLabs/fuels-wallet).

In the root of the frontend project create a file named `createWallet.js` and add the following code:

File: `./frontend/createWallet.js`

```javascript
{{#include ../../beta2-quickstart-master/frontend/createWallet.js}}
```

In a terminal, run the following command:

```console
$ node createWallet.js
address fuel160ek8t7fzz89wzl595yz0rjrgj3xezjp6pujxzt2chn70jrdylus5apcuq
private key 0x719fb4da652f2bd4ad25ce04f4c2e491926605b40e5475a80551be68d57e0fcb
```

> **Note**: You should use the generated address and private key.

Save the private key, you will need this later to set it as a string value for a variable `WALLET_SECRET` in your `App.tsx` file. More on that below.

First, take the address of your wallet and use it to get some coins from [the testnet faucet](https://faucet-beta-2.fuel.network/).

Now you're ready to build and ship ⛽

> **Note**: The team is working to simplify the process of creating a wallet, and eliminate the need to create a wallet twice. Keep an eye out for these updates.

### Modify the App

Inside the `frontend/src` folder let's add code that interacts with our contract.
Read the comments to help you understand the App parts.
Change the file `fuel-project/frontend/src/App.tsx` to:

File: `./frontend/src/App.tsx`

```ts
{{#include ../../beta2-quickstart-master/frontend/src/App.tsx}}
```

### Run your project

Now it's time to have fun, run the project in your browser.

Inside the `fuel-project/frontend` directory run:

```console
$ npm start
Compiled successfully!

You can now view frontend in the browser.

  Local:            http://localhost:3001
  On Your Network:  http://192.168.4.48:3001

Note that the development build is not optimized.
To create a production build, use npm run build.
```

![screenshot of the UI](./images/quickstart-dapp-screenshot.png)

### You just built a fullstack dapp on Fuel! ⛽

[Here is the repo for this project](https://github.com/FuelLabs/beta2-quickstart). If you run into any problems, a good first step is to compare your code to this repo and resolve any differences.

Tweet us [@fuellabs\_](https://twitter.com/fuellabs_) letting us know you just built a dapp on Fuel, you might get invited to a private group of builders, be invited to the next Fuel dinner, get alpha on the project, or something 👀.

### Updating The Contract

If you make changes to your contract, here are the steps you should take to get your frontend and contract back in sync:

- In your contract directory, run `forc build`
- In your contract directory, redeploy the contract by running this command and following the same steps as above to sign the transaction with your wallet: `forc deploy --url node-beta-2.fuel.network/graphql --gas-price 1`
- In your frontend directory, re-run this command: `npx fuels typegen -i ../counter-contract/out/debug/*-abi.json -o ./src/contracts`

- In your `fuel-project/frontend` directory, update the contract ID in your `App.tsx` file

## Need Help?

Get help from the team by posting your question in the [Fuel Forum](https://forum.fuel.network/).