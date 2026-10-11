# Rollups: How Ethereum Scales Transaction Execution
![rollups](https://github.com/0xSix9/rollups/blob/04cc1eea7084e3011274023c95019df420962b3d/img/Rollups.png)
Ethereum's mainnet provides strong security and decentralized settlement, but it has limited execution capacity.

As more users and applications enter the Ethereum ecosystem, processing every transaction directly on Ethereum becomes expensive and inefficient.

**Rollups** are one of the most important Layer 2 scaling technologies designed to solve this problem.

The core idea is simple:

> **Execute many transactions outside Ethereum, then submit the necessary data and cryptographic information back to Ethereum.**

This allows Ethereum to support a much larger amount of transaction activity without requiring every transaction to be executed directly on Layer 1.

---

# What Is a Rollup?

A **Rollup** is a Layer 2 scaling system that executes transactions outside Ethereum's main execution environment and periodically submits information about those transactions to Ethereum.

A simplified architecture looks like this:

```text
Users
  ↓
Rollup
  ↓
Execute many transactions
  ↓
Batch transactions
  ↓
Submit data / proof
  ↓
Ethereum L1
  ↓
Settlement + Security
```

The important point is that the Rollup performs most of the transaction execution, while Ethereum acts as the underlying settlement and security layer.

---

# Why Are Rollups Needed?

Ethereum has limited block space.

If every transaction had to be executed directly on Ethereum, increasing demand would lead to higher competition for block space.

This can result in:

```text
More Users
    ↓
More Transactions
    ↓
Limited L1 Capacity
    ↓
More Competition
    ↓
Higher Gas Fees
```

Rollups move much of this execution away from Layer 1.

Instead of Ethereum processing thousands of individual transactions, a Rollup can process them together and submit a compressed representation of the activity to Ethereum.

---

# How Does a Rollup Work?

Imagine that thousands of users send transactions to a Rollup.

The Rollup processes those transactions outside Ethereum:

```text
Transaction 1 ─┐
Transaction 2 ─┤
Transaction 3 ─┤
Transaction 4 ─┤
Transaction 5 ─┤
     ...       ┤
Transaction N ─┘
        ↓
   Rollup Execution
        ↓
   Batch Transactions
        ↓
   Ethereum L1
```

Instead of Ethereum executing every transaction individually, the Rollup executes them and periodically submits the information Ethereum needs to verify or settle the resulting state.

This is where the word **Rollup** comes from: many transactions are effectively bundled together into batches.

---

# What Does Ethereum Do for a Rollup?

A Rollup is not simply an independent blockchain that happens to communicate with Ethereum.

Its relationship with Ethereum is fundamental.

Depending on the Rollup design, Ethereum can provide:

* Settlement
* Security
* Data availability
* Transaction ordering commitments
* Verification of state transitions

The Rollup handles the majority of execution, while Ethereum provides the underlying trust layer.

A useful mental model is:

```text
Rollup
  ↓
Execution

Ethereum
  ↓
Settlement
  ↓
Security
  ↓
Data Availability
```

---

# The Two Main Types of Rollups

There are two major categories of Rollups:

```text
Rollups
   │
   ├── Optimistic Rollups
   │
   └── ZK Rollups
```

They mainly differ in how they demonstrate that the transactions processed by the Rollup are valid.

---

# Optimistic Rollups

Optimistic Rollups use an **optimistic assumption**.

They generally assume that the submitted state transition is correct unless someone challenges it.

If a participant detects an invalid state transition, they can submit a **fraud proof** during the relevant challenge mechanism.

The simplified process is:

```text
L2 Transactions
      ↓
Rollup Execution
      ↓
State Update
      ↓
Submit to Ethereum
      ↓
Challenge if Invalid
      ↓
Fraud Proof
```

The key idea is:

> **The state is assumed to be valid unless successfully challenged.**

---

# ZK Rollups

ZK Rollups use a different approach.

Instead of relying primarily on someone challenging an incorrect state, the Rollup generates a **validity proof** demonstrating that the state transition was executed correctly.

The simplified process is:

```text
L2 Transactions
      ↓
Rollup Execution
      ↓
Generate Validity Proof
      ↓
Submit Proof to Ethereum
      ↓
Ethereum Verifies Proof
      ↓
State Accepted
```

This provides a cryptographic method for Ethereum to verify the correctness of the Rollup's computation.

---

# Optimistic vs ZK Rollups

The fundamental difference can be remembered like this:

```text
Optimistic Rollup
→ "Assume it is valid."
→ Challenge if it is not.
→ Fraud Proof

ZK Rollup
→ "Prove that it is valid."
→ Validity Proof
```

Both approaches are designed to move transaction execution away from Ethereum while maintaining a strong connection to Ethereum's security and settlement layer.

---

# Rollup Data and Data Availability

There is another critical part of the Rollup architecture:

**Data Availability.**

Suppose a Rollup executes thousands of transactions.

Other participants may need access to the underlying transaction data to reconstruct the Rollup's state or verify its behavior.

Therefore, the necessary data must be made available.

This creates an important flow:

```text
L2 Transactions
      ↓
Rollup Execution
      ↓
Transaction Data
      ↓
Ethereum Data Availability
```

Data availability is different from simply storing the final state.

The important question is:

> **Can the required data be obtained by participants who need to verify or reconstruct the Rollup's state?**

---

# How Did Rollups Publish Data Before Blobs?

Before EIP-4844, Rollups primarily published their required data to Ethereum using **calldata**.

The process looked approximately like:

```text
Rollup
  ↓
Compress Data
  ↓
Ethereum Calldata
  ↓
Data Availability
```

Calldata is part of Ethereum transactions and therefore competes with normal transaction data for block space.

As Rollup usage increased, publishing large amounts of data through calldata could become expensive.

---

# Rollups and Blobs

EIP-4844 introduced **blobs** as a more efficient mechanism for Rollups to publish data.

Instead of relying entirely on calldata:

```text
Rollup
   ↓
Batch Data
   ↓
Blob
   ↓
Ethereum
   ↓
Temporary Data Availability
```

Blobs provide a dedicated data-availability mechanism with a separate **blob gas market**.

This significantly reduces the cost of publishing large amounts of Rollup data compared with using calldata for the same purpose.

---

# Rollup Execution vs Ethereum Execution

One of the most important concepts to understand is that a Rollup does not mean Ethereum executes the same transactions again in the normal way.

Instead:

```text
Rollup
  ↓
Executes L2 Transactions
  ↓
Produces a new L2 State
  ↓
Provides Data / Proof
  ↓
Ethereum
  ↓
Verifies / Settles according to the Rollup design
```

Ethereum therefore does not need to perform the entire workload of the Rollup.

This is the fundamental scaling advantage.

---

# Batching Transactions

Rollups can process many transactions and combine their information into batches.

For example:

```text
1000 L2 Transactions
        ↓
   Rollup Execution
        ↓
     One Batch
        ↓
Data + State Information
        ↓
Ethereum
```

The cost of interacting with Ethereum can then be distributed across many Layer 2 transactions.

This is one of the reasons users can generally pay much lower fees on Rollups than directly on Ethereum L1.

---

# Compression

Rollups can also compress transaction-related data before publishing it to Ethereum.

Imagine thousands of transactions containing repeated or unnecessary information.

A Rollup can organize and compress the relevant data before submitting it.

Conceptually:

```text
Large Amount of L2 Data
        ↓
     Compression
        ↓
Smaller Data Representation
        ↓
      Ethereum
```

The less data that needs to be published to Ethereum, the lower the cost of using Ethereum's block space.

---

# Rollup State

A Rollup maintains its own state representing the balances, contracts, and other information associated with its Layer 2 environment.

As transactions execute, this state changes.

For example:

```text
Old L2 State
     ↓
L2 Transactions
     ↓
Rollup Execution
     ↓
New L2 State
```

The Rollup then needs to communicate the relevant state transition to Ethereum.

The exact mechanism depends on whether the system is an Optimistic Rollup or a ZK Rollup.

---

# Why Can't a Rollup Just Lie?

This is where the security mechanism becomes important.

An Optimistic Rollup uses a fraud-proof system that allows invalid claims to be challenged.

A ZK Rollup uses a validity proof that allows Ethereum to verify the correctness of the state transition.

Therefore, the Rollup's security model is tied to the mechanism through which Ethereum can reject invalid state transitions.

```text
Optimistic
→ Fraud Proof

ZK
→ Validity Proof
```

This is one of the most important differences between the two architectures.

---

# Are Rollups Independent Blockchains?

A Rollup has its own execution environment and can have its own infrastructure, but it should not be thought of simply as a completely independent blockchain.

Its security and settlement relationship with Ethereum are fundamental to its design.

An independent blockchain such as Solana has its own:

```text
Consensus
Security
Settlement
Data Availability
Execution
```

A Rollup, in contrast, is designed to leverage Ethereum for important parts of this stack.

This is why Rollups are considered **Layer 2 solutions built on Ethereum**.

---

# Rollups and Ethereum's Scaling Strategy

Rollups are central to Ethereum's modern scaling strategy.

Instead of trying to make Ethereum execute every transaction directly, Ethereum can provide a secure base layer while multiple Rollups handle large-scale execution.

The architecture can be visualized as:

```text
                    Ethereum L1
                         │
          ┌──────────────┼──────────────┐
          │              │              │
        Rollup          Rollup         Rollup
          │              │              │
       Users          Users           Users
          │              │              │
       L2 Tx           L2 Tx           L2 Tx
```

This allows the Ethereum ecosystem to scale horizontally through multiple Layer 2 networks.

---

# Rollups and Ethereum Fees

Rollups reduce the cost per transaction mainly by reducing how much expensive Ethereum block space each individual transaction needs.

Instead of:

```text
Transaction
    ↓
Ethereum L1
    ↓
Pay L1 execution + data costs
```

many transactions can be processed as:

```text
Many L2 Transactions
        ↓
    One Batch
        ↓
Ethereum
        ↓
Shared Cost
```

The exact fee structure varies between Rollups, but the fundamental economic idea is the same: **spread the cost of interacting with Ethereum across many transactions.**

---

# The Complete Rollup Architecture

Now we can connect all the concepts:

```text
                         Users
                           ↓
                    Layer 2 Rollup
                           ↓
                    Execute Transactions
                           ↓
                       Batch Data
                           ↓
                ┌──────────┴──────────┐
                │                     │
             Blob Data             Proof
                │                     │
                └──────────┬──────────┘
                           ↓
                      Ethereum L1
                           ↓
              Settlement + Security
                 + Data Availability
```

For an Optimistic Rollup, the proof mechanism involves fraud proofs.

For a ZK Rollup, the system submits validity proofs.

And with EIP-4844, Rollups can use blobs as an efficient way to make large amounts of data available on Ethereum.

---

# Why Rollups Matter

Rollups solve an important scalability problem:

**Ethereum does not need to execute every transaction itself to support a large number of transactions.**

Instead:

```text
L2
→ Execute many transactions

Ethereum
→ Provide the underlying security and settlement

Rollup
→ Connect the two
```

This architecture allows Ethereum to remain a strong settlement layer while moving high-volume transaction execution to Layer 2.

---

# Conclusion

**Rollups are Layer 2 scaling systems that execute transactions outside Ethereum and periodically submit the necessary information back to Ethereum.**

There are two major approaches:

```text
Optimistic Rollups
→ Fraud Proofs

ZK Rollups
→ Validity Proofs
```

Both aim to reduce the amount of work that Ethereum must perform directly while maintaining a strong relationship with Ethereum's security and settlement infrastructure.

The evolution from calldata to blobs through **EIP-4844** has further improved this architecture by making Rollup data publication more efficient and cheaper.

The complete picture is:

```text
Layer 2
   ↓
Rollup
   ↓
Execute Transactions
   ↓
Batch + Data / Proof
   ↓
Blob / Ethereum
   ↓
Ethereum L1
   ↓
Settlement + Security + Data Availability
```

This is one of the fundamental ideas behind Ethereum's modern scaling architecture.
