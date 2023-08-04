# Monolithic Architecture

Blockchains as we know them have four functions. In no particular order:

- Execution: Execute transactions to make updates to the state.
- Settlement: Dispute resolution.
- Consensus: Defines the state and validates that all nodes on the blockchain have the same state.
- Data availability: Ensure block data has been published to the network( Availability is about the posting of data, not its presence ) 

Monolithic blockchains are a type of blockchain architecture that handle all four functions, at the same time, on this single layer.

![monolithic](./images/monolithic.png)

## Challenges with Monolithic

Some constraints and challenges with a monolithic architecture:

### Costly and inefficient transaction verification

In order to verify the validity of transactions in the chain, full nodes must verify the old signatures from the gossip network or download the whole ledger to validate previous state transitions. 

In the case of zk recursive zk-proof chains/rollups like mina the whole state can be confirmed in a single zk proof but it introduces the drawback of the computational overhead of proof generation, 

### Resource constraints

The resource capacity of its nodes binds the blockchain. Throughput is constrained by the resource requirements of a _single_ node since the blockchain is replicated, not distributed, across nodes.

This  can be somewhat mitigated by Proposer Builder Separation which specializes in the tasks of agents in the transaction supply chain where blocks are built by different parties and executed by Validator nodes. 

### Shared resources

In a monolithic architecture, the four functions of the chain operate on the same finite set of compute resources. For example, using a node's capacity for execution means less capacity left over for data availability.

Which fuel tries to solve by decoupling execution and consensus layer. 

### Scalability

Scalability is defined as the ratio of throughput to decentralization. To increase throughput—the number transactions per second—you have to increase bandwidth, compute, and storage capacity, which pushes up the cost to run a full node as a user. This is not scalability, as it reduces the number of people who can run a full node to validate the chain.
