# Blockchain Nodes: What Are Nodes and What Types of Nodes Exist?
![node](https://github.com/0xSix9/node/blob/c10d8c2fd2dade3886724de45a914e2f6003f70f/img/node.png)
## Introduction

A blockchain is often described as a decentralized network where thousands of computers communicate with each other without relying on a central server.

But what are those computers?

They are called **Nodes**.

A node is a computer or server that participates in a blockchain network by communicating with other nodes and performing one or more tasks such as receiving transactions, validating data, storing blockchain information, propagating transactions and blocks, or participating in consensus.

Understanding nodes is fundamental for anyone who wants to work with blockchain technology, especially **smart contract developers**.

A smart contract may run on a blockchain, but the blockchain itself is operated by a distributed network of nodes.

---

# 1. What Is a Blockchain Node?

A **blockchain node** is a computer running blockchain software that connects to other computers in the network and participates in the operation of the blockchain.

For example, an Ethereum node runs Ethereum client software, while a Solana node runs Solana validator software.

A simplified network might look like this:

```text
                 Node
                  |
        +---------+---------+
        |         |         |
      Node      Node      Node
        |         |         |
        +---------+---------+
                  |
                Node
```

There is no central server controlling the entire network.

Instead, nodes communicate with each other through a **peer-to-peer (P2P) network**.

Each node can receive information from other nodes and send information to other nodes.

---

# 2. Why Do Blockchains Need Nodes?

A blockchain needs nodes because the network must perform several important tasks.

Nodes can:

* Receive transactions
* Verify transactions
* Propagate transactions
* Receive blocks
* Verify blocks
* Propagate blocks
* Store blockchain data
* Maintain blockchain state
* Participate in consensus
* Execute transactions
* Provide blockchain data to applications

Without nodes, there would be no distributed network capable of maintaining the blockchain.

For example, when a user sends ETH to another address, the transaction does not magically appear inside the blockchain.

The process begins when the transaction is sent to a node.

```text
User
  |
  | Transaction
  v
Node
  |
  | Verification
  v
Network
  |
  | Propagation
  v
Validator
  |
  | Block
  v
Blockchain
```

---

# 3. Node vs Blockchain

It is important to understand the difference between a blockchain and a node.

A **blockchain** is the distributed ledger and state maintained by the network.

A **node** is a computer participating in that network.

Think of it like this:

```text
Blockchain = The system / ledger

Node = A computer participating in the system
```

There can be thousands of nodes participating in the same blockchain.

For example:

```text
             Blockchain Network

      ┌──────────┐
      │  Node A  │
      └─────┬────┘
            |
      ┌─────┴────┐
      │  Node B  │
      └─────┬────┘
            |
      ┌─────┴────┐
      │  Node C  │
      └─────┬────┘
            |
      ┌─────┴────┐
      │  Node D  │
      └──────────┘
```

All of these computers participate in the same decentralized network.

---

# 4. How Does a Node Connect to Other Nodes?

Blockchain nodes usually communicate using a **Peer-to-Peer (P2P)** network.

In a P2P network, nodes communicate directly with other nodes rather than communicating through a single central server.

For example:

```text
Node A <------> Node B
  ↑                ↑
  |                |
  ↓                ↓
Node C <------> Node D
```

A node may maintain connections with multiple peers.

When a node receives new information, it can propagate that information to its peers.

For example:

```text
User
 |
 v
Node A
 |
 +----> Node B
 |       |
 |       +----> Node D
 |
 +----> Node C
```

This allows information to spread throughout the network.

---

# 5. What Does a Node Actually Do?

A node can perform several different tasks depending on its type.

The most important responsibilities are:

## 5.1 Receiving Transactions

A node can receive transactions from wallets, applications, or other nodes.

For example:

```text
Alice
 |
 | Send 1 ETH
 v
Node
```

The node receives the transaction and begins processing it.

---

## 5.2 Verifying Transactions

The node checks whether the transaction is valid.

Depending on the blockchain, checks may include:

* Is the transaction correctly formatted?
* Is the signature valid?
* Does the sender have enough balance?
* Is the transaction nonce correct?
* Are the referenced accounts valid?
* Does the transaction follow protocol rules?
* Is the transaction executable?

If the transaction is invalid, the node rejects it.

---

# 6. Transaction Propagation

After a node receives a valid transaction, it can propagate the transaction to other nodes.

For example:

```text
Wallet
  |
  v
Node A
 /   \
v     v
B     C
 \   /
  v v
 Node D
```

Eventually, many nodes in the network can learn about the transaction.

This is one of the fundamental properties of decentralized networks.

---

# 7. Mempool

Before a transaction becomes part of a block, it is often kept in a temporary pool of pending transactions.

This is commonly called a **mempool**.

A simplified process is:

```text
User
 |
 v
Node
 |
 v
Transaction Validation
 |
 v
Mempool
 |
 v
Block Production
 |
 v
Block
```

The exact architecture differs between blockchains.

For example, Ethereum nodes maintain transaction pools, while Solana's transaction processing architecture is different and does not map perfectly to Ethereum's traditional mempool model.

Therefore, the term "mempool" should not be assumed to mean exactly the same thing on every blockchain.

---

# 8. Block Validation

Nodes can also receive blocks from other nodes.

When a node receives a block, it can verify whether the block follows the blockchain's rules.

Depending on the blockchain, checks can include:

* Block structure
* Previous block reference
* Transactions
* Digital signatures
* State transitions
* Consensus-related information
* Block producer identity
* Execution results

If the block is invalid, the node rejects it.

---

# 9. Block Propagation

After a valid block is received, a node can propagate it to its peers.

For example:

```text
Validator
    |
    v
Node A
 /   \
v     v
B     C
 \   /
  v v
 Node D
```

This allows the new block to spread throughout the network.

---

# 10. Node Storage

Nodes may store different amounts of blockchain data.

This is one of the main reasons different types of nodes exist.

Some nodes store a large portion or all of the blockchain history.

Other nodes store only the information necessary for their specific role.

This leads to several categories such as:

* Full Node
* Archive Node
* Light Node
* Validator Node
* RPC Node

These categories are related but are not always mutually exclusive.

For example, a validator can also operate as a full node.

An RPC server can also be backed by a full node or archive node.

---

# 11. Full Node

A **Full Node** is a node capable of independently verifying the blockchain according to the protocol rules.

A full node generally maintains the information required to verify the current blockchain state and validate new blocks and transactions.

The exact storage requirements and behavior depend on the blockchain.

A simplified architecture:

```text
Blockchain
    |
    v
Full Node
    |
    +---- Validate Transactions
    |
    +---- Validate Blocks
    |
    +---- Maintain State
    |
    +---- Communicate with Peers
```

The important concept is:

> A full node does not simply trust another computer's statement that the blockchain is valid. It verifies the rules itself.

---

# 12. Why Are Full Nodes Important?

Full nodes provide decentralization and independent verification.

Imagine that a blockchain had only one server:

```text
Users
  |
  v
Central Server
```

The server could potentially control the entire system.

A decentralized blockchain instead looks more like:

```text
       Node
      /    \
    Node  Node
    /        \
  Node ------ Node
```

Many independent computers verify and maintain the network.

This makes it much harder for one machine to become the single source of truth.

---

# 13. Archive Node

An **Archive Node** is a node designed to retain extensive historical blockchain data, including historical states that ordinary full nodes may not retain in the same way.

Archive nodes are particularly useful for:

* Blockchain analytics
* Historical state queries
* Block explorers
* Research
* Indexers
* Historical smart contract analysis
* Debugging
* Infrastructure providers

For example, an application may want to know:

> What was the state of this account at a particular historical block?

An archive node can provide historical state information that may not be available from a standard full node.

---

# 14. Full Node vs Archive Node

The simplest distinction is:

```text
Full Node
    |
    +-- Maintains what is needed for current validation
    |
    +-- Can verify the blockchain
    |
    +-- Usually does not retain every historical state

Archive Node
    |
    +-- Provides extensive historical data
    |
    +-- Can answer historical state queries
    |
    +-- Requires significantly more storage
```

The exact implementation differs between blockchain clients.

---

# 15. Light Node

A **Light Node** is designed to operate with significantly less storage and resource requirements than a full node.

Instead of downloading and maintaining the complete blockchain data, a light client can rely on cryptographic proofs or information from full nodes, depending on the protocol.

Conceptually:

```text
Light Node
     |
     | Request
     v
Full Node
     |
     | Proof / Data
     v
Light Node
```

Light clients are useful when running a complete node is impractical.

For example:

* Mobile devices
* Low-resource computers
* Embedded systems
* Lightweight applications

However, light-node architecture differs significantly between blockchains.

---

# 16. Validator Node

A **Validator Node** is a node that participates in the blockchain's consensus mechanism.

Validators are especially important in Proof-of-Stake-based networks.

A validator can be responsible for tasks such as:

* Participating in consensus
* Verifying transactions
* Verifying blocks
* Producing blocks when selected
* Voting on blocks
* Maintaining network state

The exact responsibilities depend on the blockchain.

---

# 17. Is Every Full Node a Validator?

No.

This is an important distinction.

A blockchain can have:

```text
Full Node
   |
   +---- Validator
   |
   +---- Non-validator
```

A full node can independently verify the blockchain without participating in block production or consensus voting.

For example, someone may run a full node simply to independently verify blockchain data.

A validator has additional responsibilities related to consensus.

---

# 18. Validator and Consensus

The relationship between validators and consensus depends on the blockchain.

For example, in a Proof-of-Stake network, validators may stake assets and participate in block production or voting.

A simplified model:

```text
Transactions
     |
     v
Network
     |
     v
Validator
     |
     | Consensus
     v
New Block
     |
     v
Network
```

Different blockchain protocols implement this process differently.

---

# 19. RPC Node

An **RPC Node** is a node that exposes an interface through which applications can communicate with the blockchain.

RPC stands for:

**Remote Procedure Call**

A decentralized application usually does not communicate with the blockchain by manually connecting to every peer.

Instead, the application commonly sends requests to an RPC endpoint.

For example:

```text
dApp
 |
 | RPC Request
 v
RPC Node
 |
 v
Blockchain Network
```

A wallet might ask:

```text
What is the balance of this address?
```

The application sends an RPC request.

The RPC infrastructure retrieves the information from the blockchain node and returns the result.

---

# 20. RPC Is Not a Separate Consensus Role

This distinction is very important.

"RPC node" describes how a node is exposed to applications.

"Validator" describes participation in consensus.

"Full node" describes the node's verification/data capabilities.

Therefore, these concepts can overlap.

For example:

```text
                 Node
                  |
        +---------+---------+
        |         |         |
     Full Node Validator   RPC
```

A single infrastructure setup can potentially perform multiple roles.

---

# 21. Public RPC vs Private RPC

RPC infrastructure can be:

### Public RPC

Available to many users.

```text
Many dApps
   |
   v
Public RPC
   |
   v
Blockchain
```

Public RPC endpoints are convenient but may have:

* Rate limits
* Request limits
* Performance limitations
* Shared infrastructure

### Private RPC

An organization may operate its own RPC infrastructure.

```text
Company
   |
   v
Private RPC
   |
   v
Blockchain Node
```

Private RPC infrastructure provides more control over:

* Performance
* Reliability
* Rate limits
* Monitoring
* Security
* Infrastructure configuration

---

# 22. Bootnode

A **Bootnode** helps a node discover other nodes in a network.

When a new node starts, it needs to find peers.

A simplified process:

```text
New Node
   |
   v
Bootnode
   |
   +----> Peer A
   +----> Peer B
   +----> Peer C
```

The bootnode is primarily used for **peer discovery**.

It does not necessarily perform the entire job of a full node or validator.

---

# 23. Seed Node

A seed node is another mechanism used to help nodes discover peers.

The exact implementation depends on the blockchain.

The general idea is:

```text
New Node
   |
   v
Seed / Discovery Mechanism
   |
   v
Other Peers
```

After discovering peers, the node can establish direct connections with them.

---

# 24. Mining Node

In Proof-of-Work blockchains, specialized nodes can participate in mining.

A mining setup can look like:

```text
Transactions
     |
     v
Node
     |
     v
Miner
     |
     v
Proof of Work
     |
     v
Block
```

The miner attempts to satisfy the blockchain's Proof-of-Work requirements.

Bitcoin is the most famous example of a Proof-of-Work blockchain.

---

# 25. Mining Node vs Validator Node

These two concepts should not be confused.

### Proof of Work

```text
Miner
 |
 | Computational Work
 v
Block
```

### Proof of Stake

```text
Validator
 |
 | Stake + Consensus Rules
 v
Block
```

Both can participate in block production, but they use different consensus mechanisms.

---

# 26. Node Communication

Nodes communicate using network protocols.

A node may communicate information such as:

* Transactions
* Blocks
* Consensus messages
* Peer information
* Requests for blockchain data

A simplified flow:

```text
Node A
  |
  | Transaction
  v
Node B
  |
  | Transaction
  v
Node C
```

The blockchain therefore behaves as a distributed communication network.

---

# 27. Node Verification vs Trust

One of the most important ideas in blockchain is:

**Don't trust. Verify.**

Suppose Node A tells Node B:

> "This transaction is valid."

Node B should not necessarily accept that statement blindly.

Instead, Node B can independently verify the transaction according to the blockchain's rules.

```text
Node A
 |
 | "This is valid"
 v
Node B
 |
 | Verify independently
 v
Accept / Reject
```

This principle is fundamental to decentralized systems.

---

# 28. Node and Cryptography

Nodes rely heavily on cryptography.

For example, nodes may verify:

### Digital Signatures

To determine whether a transaction was authorized by the appropriate private key.

```text
Private Key
     |
     v
Signature
     |
     v
Transaction
     |
     v
Node verifies signature
```

### Hashes

Nodes use hashes to verify data integrity and relationships between blockchain objects.

For example:

```text
Block N
   |
   | Hash
   v
Block N+1
```

Cryptography allows nodes to verify information without trusting a central authority.

---

# 29. Node and State

Modern blockchains do not simply store a list of transactions.

They also maintain a representation of the current **state** of the blockchain.

For example, the state can include:

* Account balances
* Smart contract storage
* Contract code
* Token information
* Account metadata

A node processes transactions and applies state transitions.

Conceptually:

```text
Old State
    |
    | Transaction
    v
Execution
    |
    v
New State
```

This concept is extremely important for smart contract developers.

---

# 30. Nodes and Smart Contracts

When a smart contract transaction is submitted, nodes are involved in processing it.

A simplified Ethereum-style flow:

```text
User
 |
 | Transaction
 v
RPC
 |
 v
Node
 |
 v
Transaction Pool
 |
 v
Validator
 |
 v
Block
 |
 v
Other Nodes
 |
 v
Execution + State Update
```

When the transaction becomes part of a valid block, nodes execute and verify the state transition according to the protocol.

---

# 31. Nodes in Ethereum

Ethereum uses multiple types of clients and node components.

At a high level, Ethereum's modern architecture separates:

### Execution Layer

Responsible for:

* Transaction execution
* EVM execution
* Maintaining execution state
* Execution-layer networking

### Consensus Layer

Responsible for:

* Proof-of-Stake consensus
* Validator duties
* Block proposal
* Attestations
* Consensus state

A simplified architecture:

```text
                Ethereum Node

        ┌────────────────────────┐
        │     Consensus Layer    │
        │                        │
        │ Validators / Consensus │
        └───────────┬────────────┘
                    |
                    |
        ┌───────────┴────────────┐
        │     Execution Layer    │
        │                        │
        │ EVM / Transactions     │
        │ State / Execution      │
        └────────────────────────┘
```

This is an important difference from older descriptions of Ethereum nodes, where the architecture was often presented as a single client.

---

# 32. Ethereum Full Node

An Ethereum node generally runs software for both the execution layer and consensus layer.

For example:

```text
Execution Client
       +
Consensus Client
       =
Ethereum Node
```

The execution client handles execution-related responsibilities, while the consensus client handles Proof-of-Stake consensus.

A validator adds validator-specific functionality.

---

# 33. Ethereum Validator

An Ethereum validator participates in Proof-of-Stake consensus.

A validator can perform duties such as:

* Proposing blocks when selected
* Attesting to blocks
* Participating in consensus
* Maintaining validator keys
* Following consensus rules

Conceptually:

```text
Ethereum Network
       |
       v
Validator
       |
       +---- Propose
       |
       +---- Attest
       |
       +---- Consensus
```

A validator requires more than simply running an ordinary RPC endpoint.

---

# 34. Nodes in Solana

Solana's architecture is different from Ethereum's.

Solana nodes commonly participate in the network as **validators** or other supporting infrastructure.

A Solana validator processes transactions and participates in the network's consensus and block-production system.

Solana uses **Proof of History (PoH)** as a cryptographic clock combined with its consensus mechanism.

A simplified view:

```text
Users
  |
  v
RPC
  |
  v
Solana Network
  |
  v
Validator
  |
  v
Transactions / Entries
  |
  v
Confirmed Blockchain State
```

Solana's architecture is optimized for high-throughput transaction processing and parallel execution.

---

# 35. Leader in Solana

In Solana, validators are assigned opportunities to produce blocks during specific time periods called **slots**.

A simplified concept is:

```text
Slot 1 -> Validator A
Slot 2 -> Validator B
Slot 3 -> Validator C
Slot 4 -> Validator A
```

The validator assigned to a slot acts as the leader for that slot.

The leader processes transactions and produces the corresponding entries/blocks according to Solana's protocol.

Other validators then process and verify the resulting data.

---

# 36. Slot vs Node

A common misunderstanding is to think that a slot is a physical resource assigned permanently to a validator.

It is not.

A **slot** is a unit of time in Solana's ledger progression.

Validators are assigned leadership opportunities for particular slots.

For example:

```text
Time
 |
 +---- Slot 1 ----> Validator A
 |
 +---- Slot 2 ----> Validator B
 |
 +---- Slot 3 ----> Validator C
 |
 +---- Slot 4 ----> Validator A
```

The validator does not own the slot permanently.

It is assigned leadership for that slot according to the protocol's schedule.

---

# 37. RPC Nodes for Solana

Applications interacting with Solana commonly use RPC endpoints.

For example:

```text
Solana dApp
     |
     | RPC
     v
Solana RPC Infrastructure
     |
     v
Solana Network
```

A developer can use RPC methods to:

* Query accounts
* Read balances
* Submit transactions
* Read blocks
* Read transaction information
* Query program accounts
* Request blockchain data

This is why RPC is extremely important for Web3 developers.

---

# 38. Nodes and APIs

A blockchain node itself is not necessarily the same thing as an API.

Think of it this way:

```text
Blockchain Node
      |
      | Exposes
      v
RPC Interface
      |
      v
Application
```

The node provides access to blockchain functionality.

The RPC interface provides a standardized way for software to communicate with the node.

---

# 39. Node, RPC, Indexer, and Explorer

These four concepts are often confused.

They are different.

### Node

Communicates with the blockchain network and validates/maintains blockchain data according to its role.

### RPC

Provides an interface for applications to interact with node infrastructure.

### Indexer

Processes blockchain data into a format optimized for querying.

### Block Explorer

Provides a user interface for viewing blockchain data.

The architecture may look like:

```text
Blockchain
    |
    v
   Node
    |
    v
Indexer
    |
    v
Database
    |
    v
Explorer
```

Or:

```text
dApp
 |
 v
RPC
 |
 v
Node
 |
 v
Blockchain
```

---

# 40. Why Do Indexers Exist?

Raw blockchain data is not always convenient for applications.

Suppose you want:

> Show me every token transfer made by this wallet during the last six months.

You could potentially process enormous amounts of blockchain data.

An indexer can process blockchain data beforehand and create optimized databases.

```text
Blockchain
    |
    v
Node
    |
    v
Indexer
    |
    v
Database
    |
    v
Application
```

This is why many serious Web3 applications use both RPC infrastructure and indexing systems.

---

# 41. Node Types Are Not Always Exclusive

One of the most important things to understand is that these categories can overlap.

For example:

```text
                 Node
                  |
       ┌──────────┼──────────┐
       |          |          |
     Full     Validator     RPC
       |
    Archive
```

A node can potentially be:

* A full node
* A validator
* An RPC provider
* An archive node

depending on its software, configuration, hardware and purpose.

Therefore, saying:

> "There are exactly five completely separate types of nodes"

would be an oversimplification.

Different terms describe different aspects of a node.

---

# 42. A Better Way to Classify Nodes

Nodes can be classified based on several dimensions.

## Based on Data

* Full Node
* Archive Node
* Light Node

## Based on Consensus Role

* Validator
* Miner

## Based on Network Infrastructure

* RPC Node
* Bootnode
* Seed/Discovery Node

These categories can overlap.

---

# 43. What Happens When You Send a Transaction?

Let's put everything together.

Imagine Alice wants to send cryptocurrency to Bob.

### Step 1 — Wallet Creates Transaction

```text
Alice's Wallet
     |
     v
Transaction
```

The wallet signs the transaction using Alice's private key.

---

### Step 2 — Transaction Sent to RPC

```text
Wallet
  |
  v
RPC
```

The RPC endpoint forwards the transaction into the blockchain network.

---

### Step 3 — Node Receives Transaction

```text
RPC
 |
 v
Node
```

The node checks the transaction.

---

### Step 4 — Transaction Propagates

```text
Node A
 /   \
v     v
B     C
```

The transaction spreads through the network.

---

### Step 5 — Block Producer Processes It

Depending on the blockchain:

```text
Validator / Miner
       |
       v
Transaction
       |
       v
Block
```

---

### Step 6 — Other Nodes Verify the Block

```text
Block
  |
  +----> Node A -> Verify
  |
  +----> Node B -> Verify
  |
  +----> Node C -> Verify
```

---

### Step 7 — State Changes

The transaction is executed according to the protocol.

```text
Old State
    |
    | Transaction
    v
New State
```

Alice's balance decreases and Bob's balance increases, assuming the transaction is valid and successfully executed.

---

# 44. What Happens If a Node Is Malicious?

Blockchain protocols are designed to prevent a single malicious node from simply changing the blockchain.

Suppose:

```text
Node A -> "This transaction is valid!"
```

Other nodes do not have to trust Node A.

They can verify the transaction themselves.

If Node A sends invalid information:

```text
Malicious Node
      |
      v
Invalid Data
      |
      v
Other Nodes
      |
      v
Reject
```

This is one of the fundamental mechanisms behind blockchain security.

---

# 45. What If a Node Goes Offline?

A decentralized network should continue operating even if individual nodes go offline.

For example:

```text
Node A ❌
Node B ✅
Node C ✅
Node D ✅
Node E ✅
```

The remaining nodes can continue communicating.

This is one of the advantages of distributed infrastructure.

However, the impact of a node going offline depends on its role.

For example:

* An ordinary full node going offline affects that operator.
* An RPC server going offline can affect applications using it.
* A validator going offline can affect its consensus participation and potentially its rewards.
* A critical infrastructure provider going offline can affect many applications.

---

# 46. Running Your Own Node

A developer or organization can run their own blockchain node.

A simplified process is:

```text
Hardware / Server
       |
       v
Blockchain Client
       |
       v
Node
       |
       v
Blockchain Network
```

Running your own node provides benefits such as:

* Independent verification
* Reduced dependence on third-party RPC providers
* Greater control
* Better privacy
* Infrastructure experimentation
* Network participation
* Development and testing

But it also introduces costs:

* Hardware
* Storage
* Bandwidth
* Maintenance
* Monitoring
* Updates
* Security

---

# 47. Why Smart Contract Developers Should Understand Nodes

As a smart contract developer, you may not need to operate a validator.

But you absolutely need to understand what nodes do.

Because your application interacts with blockchain infrastructure through nodes.

For example:

```text
Frontend
   |
   v
Wallet
   |
   v
RPC
   |
   v
Node
   |
   v
Blockchain
```

If you understand nodes, you can better understand:

* RPC
* Transactions
* Block confirmations
* Finality
* Network latency
* Transaction propagation
* Blockchain state
* Validators
* Block production
* Node providers
* Indexers
* Blockchain explorers

---

# 48. The Most Important Node Types to Know

For a blockchain developer, these are the most important concepts:

| Type                   | Main Purpose                                                         |
| ---------------------- | -------------------------------------------------------------------- |
| Full Node              | Independently verifies and maintains blockchain data                 |
| Archive Node           | Provides extensive historical blockchain/state data                  |
| Light Node             | Accesses blockchain with significantly reduced resource requirements |
| Validator              | Participates in consensus                                            |
| Miner                  | Participates in Proof-of-Work block production                       |
| RPC Node               | Provides blockchain access to applications                           |
| Bootnode               | Helps nodes discover peers                                           |
| Indexer Infrastructure | Processes blockchain data for efficient querying                     |

Remember that these categories can overlap.

---

# 49. The Big Picture

The complete blockchain infrastructure can be visualized like this:

```text
                     Users
                       |
                       v
                    Wallet
                       |
                       v
                      RPC
                       |
                       v
                +-------------+
                |    Nodes    |
                +-------------+
                 /     |      \
                /      |       \
               v       v        v
          Full Node Validator Archive
               |       |          |
               |       |          |
               +-------+----------+
                       |
                       v
                 Blockchain
```

Applications can access the blockchain through RPC infrastructure.

Nodes communicate with each other through the P2P network.

Full nodes independently verify blockchain rules.

Validators participate in consensus.

Archive nodes retain extensive historical information.

Indexers process blockchain data for efficient application queries.

Together, these components create the infrastructure that allows a decentralized blockchain to operate.

---

# 50. Final Summary

A **node** is a computer running blockchain software that participates in a blockchain network.

Nodes are responsible for different tasks depending on their configuration and role.

The most important types include:

**Full Node**

Independently verifies blockchain data and maintains the information necessary for its operation.

**Archive Node**

Retains extensive historical blockchain/state information and is useful for analytics, research and historical queries.

**Light Node**

Uses significantly fewer resources and relies on proofs or other nodes for some information.

**Validator**

Participates in the blockchain's consensus mechanism and may produce or vote on blocks.

**Miner**

Participates in Proof-of-Work block production.

**RPC Node**

Provides an interface that allows applications and wallets to communicate with blockchain infrastructure.

**Bootnode**

Helps new nodes discover peers.

The most important concept to remember is:

> **A node is the computer participating in the blockchain network, while different node types describe different responsibilities, capabilities, and configurations.**

For a smart contract developer, the most important relationship to understand is:

```text
User
  |
  v
Wallet
  |
  v
RPC
  |
  v
Node
  |
  v
Blockchain Network
  |
  v
Validator
  |
  v
Block
  |
  v
Other Nodes
  |
  v
Verified Blockchain State
```

Once you understand this architecture, concepts such as **RPC, validators, block propagation, transaction propagation, finality, consensus, network latency, indexing, and blockchain infrastructure** become much easier to understand.
