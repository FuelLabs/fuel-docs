# What is Fuel?

Fuel v1 began as a layer-2 (L2) scalability technology for a monolithic Ethereum. It was the first optimistic rollup on mainnet Ethereum, deployed at the end of 2020. 

Today, Fuel is the fastest modular execution layer. Fuel delivers the highest security and flexible throughput, with a focus on a superior developer experience.  

Here’s how we do it: 
- Parallel transaction execution
- Fuel Virtual Machine (FuelVM)
- Developer tooling with Sway Language and Forc

## Parallel Transactions 

Fuel delivers unmatched processing capacity through its ability to execute transactions in parallel by using strict state access lists in the form of a UTXO model. This enables Fuel to use far more threads and cores of your CPU that are typically idle in single-threaded blockchains. As a result, Fuel can deliver far more compute, state accesses, and transactional throughput than its single-threaded counterparts.

With the EVM, it is difficult to know if, and where there are dependencies between transactions, so you are forced to execute transactions sequentially. 

The FuelVM uses the UTXO model, enabling parallel transaction execution by identifying transaction dependencies through what is known as state access lists. With the FuelVM, Fuel full nodes identify the accounts that a transaction touches, mapping out dependencies before execution. Read more about how UTXOs work here. 

## FuelVM 

The FuelVM learns from the Ethereum ecosystem, implementing improvements that were suggested to the Ethereum VM (EVM) for many years but couldn’t be implemented due to the need to maintain backward compatibility. 

Here are some EIP that have been implemented in the FuelVM:

| EIP                                                                        | Description                                                                                                                                     | Implementation                                                                                                                                                                   |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Easy Parallelizability](https://github.com/ethereum/EIPs/issues/648)      | Allow transactions in the EVM to be processed in parallel by specifying what addresses they can access.                             | Fuel can execute transactions in parallel by using strict state access lists as a UTXO model and can make use of all the threads and cores of your CPU to validate transactions. |
| [EIP 2098: Compact Signature Representation](https://eips.ethereum.org/EIPS/eip-2098)  | Reduce signatures from 65 bytes to 64 bytes to simplify handling transactions in client code, reduce gas costs, and reduce transaction sizes. | How fuel implements here         |                                                                                                                                    
|  [EIP 1153: Transient Storage Opcodes](https://eips.ethereum.org/EIPS/eip-1153) | Transient storage opcodes, which manipulate state that behaves identically to storage, except that transient storage is discarded after every transaction. In other words, the values of transient storage are never deserialized from storage or serialized to storage. Thus transient storage is cheaper since it never requires disk access.   | FuelVM has a new shared global memory architecture which makes making calls between contracts as easy as passing pointers to data, because call frames can read anything from a shared memory block. This is in contrast to the EVM where every contract call has to serialize data that is forwarded or returned - this is wasted processing which limits cross-contract communication. This shared memory is not globally writable, and contracts can only write to their subset of the shared memory.    |
| [EIP 3074: AUTH and AUTH opcodes](https://eips.ethereum.org/EIPS/eip-3074) | Introduces two EVM instructions, `AUTH` and `AUTHCALL`, to enable batching capabilities, allowing for gas sponsoring, expirations, scripting, and beyond. | Fuel scripts? |
| [EIP 3102: Binary Trie Structure](https://eips.ethereum.org/EIPS/eip-3102) | Introduces binary structure and merkelization rule for the account and storage tries, which are merged into a single “state” trie. Binary tries make for smaller (~4x) proofs than hexary tries, making it the design of choice for a stateless-friendly Ethereum. | Stateless account abstraction? |
| [EIP 4758: Deactivate SELFDESTRUCT](https://eips.ethereum.org/EIPS/eip-4758) | Rename `SELFDESTRUCT` opcode to `SENDALL`, and only immediately move all ETH in the account to the target; no longer destroying code or storage or alters the nonce. Disabling `SELFDESTRUCT` will be a requirement for statelessness. | How does fuel implement here? |
| [EIP 5027: Remove the limit on contract code size](https://eips.ethereum.org/EIPS/eip-5027) |  Remove the limit on the code size so that users can deploy a large-code contract without worrying about splitting the contract into several sub-contracts. With the dramatic growth of dApplications, the functionalities of smart contracts are becoming more and more complicated, and thus, the sizes of newly developed contracts are steadily increasing. As a result, we a facing more and more issues with the 24576-bytes contract size limit. | ?? |
|[EIP-5065: Instruction for Transferring Ether](https://eips.ethereum.org/EIPS/eip-5065) | Add a new instruction that transfers ether to a destination address without handing over the flow of execution to it. Ethereum currently does not have any ideal way to transfer ether without transferring the flow of execution. People have come up with reentrancy guards and similar solutions to prevent some types of attacks but it’s not an ideal solution. | How does fuel do this |
| [EIP-86: Abstraction of Transaction Origin and Signature](https://eips.ethereum.org/EIPS/eip-86) | Implements a set of changes that serve the combined purpose of “abstracting out” signature verification and nonce checking, allowing users to create “account contracts” that perform any desired signature/nonce checks instead of using the mechanism that is currently hard-coded into transaction processing. | How does fuel do this |
| [EIP-1051: Overflow Checking for the EVM](https://eips.ethereum.org/EIPS/eip-1051) | This EIP adds overflow checking for EVM arithmetic operations, and two new opcodes that check and clear the overflow flags. Since the EVM operates on mod 2^256 integers and provides no built-in overflow detection or prevention, this requires manual checks on every arithmetic operation. | How does fuel do this |
| [EIP-2803: Rich Transactions](https://eips.ethereum.org/EIPS/eip-2803) | If a transaction has a to of address x, then the data of the transaction will be treated as EVM bytecode and it will be executed from the context of the CALLER of the transaction (aka: the transaction signer). Many Ethereum DApps presently require users to approve multiple transactions in order to produce one effect. This results in a poor user-experience, and complicates the experience of interacting with DApps. | How we do this fam |
| [EIP-2926: Chunk-based Code Merkelization](https://eips.ethereum.org/EIPS/eip-2926) | Bytecode is currently the second contributor to block witness size, after the proof hashes. Transitioning the trie from hexary to binary reduces the hash section of the witness by 3x, thereby making code the first contributor. By breaking contract code into chunks and committing to those chunks in a merkle tree, stateless clients would only need the chunks that were touched during a given transaction to execute it. | Explain |
| [EIP-2938: Account Abstraction](https://eips.ethereum.org/EIPS/eip-2938) | Bytecode is currently the second contributor to block witness size, after the proof hashes. Transitioning the trie from hexary to binary reduces the hash section of the witness by 3x, thereby making code the first contributor. By breaking contract code into chunks and committing to those chunks in a merkle tree, stateless clients would only need the chunks that were touched during a given transaction to execute it. | Explain |





## Sway Language 

Sway is a domain-specific language (DSL) for the Fuel Virtual Machine (FuelVM), a blockchain-optimized VM designed for the Fuel blockchain. Sway is based on Rust and includes syntax to leverage a blockchain VM without a needlessly verbose boilerplate.

Sway was created alongside the FuelVM and designed for the high-compute Fuel environment. Check out the Sway docs here. 

## Developer Tooling 

Fuel Labs is developing a suite of developer tooling to create a seamless developer experience. By creating everything in-house, Fuel Labs guarantees the maintenance of the toolchain, avoiding the pitfalls of a fragmented developer ecosystem. 

Fuel provides a powerful and sleek developer experience with our own domain-specific language, called Sway, and a supportive toolchain, called Forc (the Fuel Orchestrator). Our development environment retains the benefits of smart contract languages like Solidity, while adopting the paradigms introduced in the Rust tooling ecosystem.  Now, developers can have a completely vertically-integrated experience where every component from the virtual machine through to the CLI works in harmony.

**The Fuel Toolchain:**
Forc: build, run tests, launch a local instance of a block explorer, format 

**Coming Soon:**
A suite of auditing facilities, from fuzzing and formal verification to code coverage and runtime gating
Indexing and GraphQL query API 
