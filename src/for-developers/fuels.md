# Fuel Wallet

Fuel has a beta wallet extension that can be downloaded. Keep in mind this is an alpha version. For now, it isn't possible to download the extension directly from Chrome Extensions Store. For instructions on how to download the extension, check out the docs [here](https://fuels-wallet.vercel.app/docs/install/#installing-extension).

Fuels is an SDK that allows developers to let users interact with the Fuel node via the Fuel wallet. Check out the API reference [here](https://fuels-wallet.vercel.app/docs/api/).

## Unsigned Transactions

You can deploy a contract to a local fuel instance without signing with a wallet by running the following command:

`forc deploy --unsigned`

To deploy a contract to the testnet, you'll need a Fuel wallet. You can initialize a wallet and an account for your wallet to sign transactions.

[Check out the documentation](https://github.com/FuelLabs/forc-wallet#forc-wallet) for a step-by-step guide to initializing a wallet and creating an account.
