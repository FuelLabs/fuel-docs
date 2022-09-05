# Monolithic Architecture

Blockchains as we know them have four functions. In no particular order:

- Execution: Execute transactions to make updates to the state.
- Settlement: Dispute resolution.
- Consensus: Defines the state and validates that all nodes on the blockchain have the same state.
- Data availability: Ensure block data has been published to the network.

Monolithic blockchains are a type of blockchain architecture that handle all four functions, at the same time, on this single layer.

![monolithic](./images/monolithic.png)

## Challenges with Monolithic

Some constraints and challenges with a monolithic architecture:

### Costly and inefficient transaction verification

In order to verify the validity of transactions in the chain, full nodes must download the entire chain and execute each transaction locally.

### Not designed for fraud proofs

Using the general construction for fraud proofs, in order to create a fraud proof you have to provide the pre-state and compute the post-state, comparing what the block producer's post-state was to their outcome. If a contract calls other contracts, the pre-state could be extremely large, and the cost to produce a fraud proof could be unbound.

### Resource constrains

The blockchain is bound by the resource capacity of its nodes. Throughput is constrained by the resource requirements of a *single* node. Since the blockchain is replicated, not distributed, across nodes.

### Scalability

Scalability is defined as throughput / decentralization. To increase throughput, the number transactions per second, you have to increase bandwidth capacity, which pushes up the cost to run a full node as a user. This is not scalability, as it reduces the number of people with capacity to run a full node to validate the chain.
