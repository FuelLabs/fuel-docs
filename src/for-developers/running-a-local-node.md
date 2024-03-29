# Running a local Fuel node

In addition to deploying and testing on the Fuel Testnet, you can also run a local Fuel Node.

There are two types of Fuel networks that can be run:

1. In-memory network (without persistance)
2. Local network with persistance

## In-memory local node (without state persistance)

An in-memory node does not persist the blockchain state anywhere, it is only stored in memory as long as the node is active and running.

To spin Up a local in-memory Fuel node, run the following command:

```sh
fuel-core run --db-type in-memory
```

To deploy a contract to the local node, run the following command:

```sh
forc deploy <signing-key> --url 127.0.0.1:4000/graphql
```

Or to deploy without using a signing key:

```sh
forc deploy --unsigned --url 127.0.0.1:4000/graphql
```

## Local node (with state persistance)

This node does persist the blockchain state locally.

To run a local node with persistance, you must configure a `chainConfig.json` file. Here is an example of what that looks like:

```json
{
    "chain_name": "local_testnet",
    "block_production": {
      "ProofOfAuthority": {
        "trigger": "instant"
      }
    },
    "parent_network": {
      "type": "LocalTest"
    },
    "block_gas_limit": 1000000000,
    "initial_state": {
      "coins": [
        {
          "owner": "0x94ffcc53b892684acefaebc8a3d4a595e528a8cf664eeb3ef36f1020b0809d0d",
          "amount": "0xFFFFFFFFFFFFFFFF",
          "asset_id": "0x0000000000000000000000000000000000000000000000000000000000000000"
        },
        {
          "owner": "0x80d5e88c2b23ec2be6b2e76f3499a1a2755bb2773363785111a719513fb57b8e",
          "amount": "0x00000000FFFFFFFF",
          "asset_id": "0x0000000000000000000000000000000000000000000000000000000000000000"
        },
        {
          "owner": "0xf13c949256d0e119fecaec414ea452f21f9dc1870fb6262ff53b37c32cab4749",
          "amount": "0x00000000FFFFFFFF",
          "asset_id": "0x0000000000000000000000000000000000000000000000000000000000000000"
        }
      ]
    },
    "transaction_parameters": {
      "contract_max_size": 16777216,
      "max_inputs": 255,
      "max_outputs": 255,
      "max_witnesses": 255,
      "max_gas_per_tx": 100000000,
      "max_script_length": 1048576,
      "max_script_data_length": 1048576,
      "max_static_contracts": 255,
      "max_storage_slots": 255,
      "max_predicate_length": 1048576,
      "max_predicate_data_length": 1048576,
      "gas_price_factor": 1000000000,
      "gas_per_byte": 4,
      "max_message_data_length": 1048576
    },
    "block_producer": {
      "utxo_validation": true,
      "coinbase_recipient": "0x0000000000000000000000000000000000000000000000000000000000000000"
    },
    "consensus": {
      "PoA": {
        "signing_key": "0x22ec92c3105c942a6640bdc4e4907286ec4728e8cfc0d8ac59aad4d8e1ccaefb"
      }
    }
  }
  ```

  To start the node, run the following command:

```sh
fuel-core run --ip 127.0.0.1 --port 4000 --chain ./chainConfig.json --db-path ./.fueldb
```

## Connecting to the local node from a browser wallet

To connect to the local node using a browser wallet, import the network address as:

```sh
http://127.0.0.1:4000/graphql
```
