# Fuel Wallet

You can deploy a contract without signing with a wallet by running the following command:

`forc deploy --unsigned`

You can initialize a wallet and an account for your wallet to sign transactions

To initialize a wallet you can use `forc wallet init`. It will ask you to choose a password to encrypt your wallet. After the initialization is done you will have your mnemonic phrase.

After you have an initialized wallet, you can create an account for it by `running forc wallet new`. It will ask your password to decrypt the wallet before creating an account.

Check out the documentation on [Initializing a Wallet and Adding Accounts](https://fuellabs.github.io/sway/master/forc_client.html).

## Fuel Faucet

Assets for the Testnet node can be obtained from the faucet at
[https://faucet-beta-1.fuel.network/](https://faucet-beta-1.fuel.network/).

## Block Explorer

The block explorer for the testnet can be found [here](https://fuellabs.github.io/block-explorer-v2/).
