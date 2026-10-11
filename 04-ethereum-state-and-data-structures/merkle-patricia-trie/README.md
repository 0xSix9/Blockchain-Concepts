# What Is a Merkle Patricia Trie and Why Does Ethereum Use It?
![Merkle Patricia Trie](https://github.com/0xSix9/merkle-patricia-trie/blob/2b3cf15267543777cd0c7da5e40bcf50caeb6758/img/merkle-patricia-trie.png)
## Introduction

In the previous article, we discussed the **Merkle Tree** and learned how a large collection of data can be represented by a single root hash.

When we move to Ethereum, however, we need a more advanced data structure.

Ethereum does not simply store a list of transactions. It needs to organize and verify large amounts of **key-value data**, including:

- Accounts
- Balances
- Nonces
- Smart contract code
- Contract storage
- Transactions
- Transaction receipts

For this reason, Ethereum's **Execution Layer** uses a modified version of the **Merkle Patricia Trie**. Ethereum's official documentation describes it as a deterministic and cryptographically verifiable data structure for encoding Ethereum state and other execution-layer data.

---

# First: What Is a Trie?

Before understanding the Merkle Patricia Trie, we need to understand what a **Trie** is.

A Trie is a tree-based data structure that is especially useful for storing data associated with keys.

Suppose we have these keys:

```text
cat
car
can
dog
```

A Trie can share the common parts of these keys:

```text
             root
            /    \
           c      d
           |      |
           a      o
         / | \    |
        t  r  n   g
```

Here, `cat`, `car`, and `can` share the common path `ca`.

This makes a Trie useful for organizing key-based data.

---

# What Is a Patricia Trie?

A basic Trie can become inefficient when paths are long and contain many nodes that do not represent meaningful branching points.

A **Patricia Trie** introduces path compression, reducing unnecessary intermediate nodes and making key-based data more efficient to organize.

---

# What Does "Merkle" Add?

A Patricia Trie organizes key-value data efficiently, but Ethereum also needs the data structure to be **cryptographically verifiable**.

This is where Merkle hashing comes in.

```text
Patricia Trie
      +
Hashing
      ↓
Merkle Patricia Trie
```

Nodes are linked through cryptographic hashes. As a result, the root of the trie becomes a cryptographic commitment to the data contained in the structure.

If the underlying data changes, the hashes along the affected path change, and eventually the root hash changes as well.

---

# Why Does Ethereum Need It?

Ethereum maintains a large global state containing information about accounts and smart contracts.

This includes:

```text
Accounts
Balances
Nonces
Contract Code
Contract Storage
```

Ethereum needs a data structure that allows this information to be:

1. Deterministically organized.
2. Efficiently retrieved.
3. Cryptographically verified.
4. Represented by a root hash.
5. Used to construct proofs about individual pieces of data.

The modified Merkle Patricia Trie provides these properties.

---

# The Ethereum State Trie

One of the most important tries in Ethereum is the **State Trie**.

The State Trie represents Ethereum's global state.

```text
Ethereum State
      ↓
State Trie
      ↓
stateRoot
```

An Ethereum account is represented by four main fields:

```text
[nonce, balance, storageRoot, codeHash]
```

The account data is stored in the State Trie under a path derived from the hash of the Ethereum address.

In simplified form:

```text
Account Address
      ↓
keccak256(address)
      ↓
State Trie
      ↓
Account Data
```

---

# What Is `storageRoot`?

An Ethereum account contains a field called:

```text
storageRoot
```

For a contract account, this is the root of another Merkle Patricia Trie: the contract's **Storage Trie**.

```text
Ethereum
   │
   └── State Trie
          │
          └── Contract Account
                    │
                    ↓
               Storage Trie
                    │
                    ↓
               storageRoot
```

So `storageRoot` is itself the root hash of another trie.

---

# What Is the Storage Trie?

The **Storage Trie** stores the persistent storage of a smart contract.

For example:

```solidity
uint256 value;
```

The value is stored in the contract's persistent storage.

Conceptually:

```text
Storage Key
     ↓
Hash
     ↓
Storage Trie
     ↓
Storage Value
```

The root of this trie is represented by:

```text
storageRoot
```

Each contract account can have its own Storage Trie.

---

# The Three Important Roots in Ethereum

Ethereum's Execution Layer has three important roots associated with three tries:

```text
stateRoot
transactionsRoot
receiptsRoot
```

Conceptually:

```text
                Ethereum Block
                      │
             ┌────────┼────────┐
             │        │        │
             ↓        ↓        ↓
          State   Transactions  Receipts
           Trie       Trie       Trie
             │          │          │
             ↓          ↓          ↓
         stateRoot  transactionsRoot  receiptsRoot
```

## 1. `stateRoot`

`stateRoot` represents a cryptographic commitment to the resulting global state after the block's transactions have been executed.

```text
Previous State
      +
Transactions
      ↓
EVM Execution
      ↓
New State
      ↓
State Trie
      ↓
stateRoot
```

If an account's balance changes, or a smart contract's persistent storage changes, the affected trie data changes and the `stateRoot` changes.

---

## 2. `transactionsRoot`

A block contains a set of transactions.

Ethereum organizes the transactions of a block in a transaction trie.

```text
Transactions
      ↓
Transactions Trie
      ↓
transactionsRoot
```

The path for each transaction is based on its transaction index.

---

## 3. `receiptsRoot`

When a transaction is executed, Ethereum produces a **Transaction Receipt** containing information about its execution.

A receipt can include:

- Status
- Cumulative gas used
- Logs
- Logs bloom

The receipts for a block are organized in a trie:

```text
Transaction Receipts
        ↓
Receipts Trie
        ↓
receiptsRoot
```

---

# Putting Everything Together

```text
                         Ethereum Block
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ↓                ↓                ↓
          State Trie     Transactions Trie   Receipts Trie
              │                │                │
              ↓                ↓                ↓
          stateRoot      transactionsRoot   receiptsRoot
```

The State Trie also connects to Storage Tries belonging to contract accounts:

```text
                         stateRoot
                             │
                         State Trie
                             │
                         Contract
                             │
                             ↓
                       Storage Trie
                             │
                             ↓
                       storageRoot
```

---

# Why Is It Called "Merkle"?

The word **Merkle** refers to the cryptographic hashing used throughout the structure.

Conceptually:

```text
Child Node
    ↓
Hash
    ↓
Parent Node
    ↓
Root
```

If the underlying data changes, the affected hashes change and the root changes.

This gives the structure its cryptographic integrity.

---

# Why Is It Called "Patricia"?

The word **Patricia** refers to the Patricia Trie concept.

PATRICIA stands for:

**Practical Algorithm To Retrieve Information Coded in Alphanumeric**

A Patricia Trie uses path compression to reduce unnecessary nodes and improve the organization of key-based data.

Ethereum combines this idea with Merkle-style hashing to create a structure suitable for cryptographically verifiable key-value data.

---

# Why Not Just Use a Normal Merkle Tree?

A normal Merkle Tree is excellent for representing a collection of data:

```text
A
B
C
D
 ↓
Merkle Tree
 ↓
Root
```

However, Ethereum works heavily with **key-value data**:

```text
Address → Account
Storage Key → Storage Value
Transaction Index → Transaction
```

Ethereum therefore needs a structure that provides both:

- Efficient key-value organization
- Cryptographic verification

This is where the combination becomes useful:

```text
Merkle Tree
     │
     │ Hash-based verification
     ↓
Patricia Trie
     │
     │ Key-value organization
     ↓
Merkle Patricia Trie
```

---

# Merkle Proofs in Ethereum

Suppose we want to prove that a particular account exists in the Ethereum State Trie.

We do not necessarily need to provide the entire state.

Instead, a proof can provide the nodes and hashes needed to reconstruct the path from that account to the root.

```text
Account
   +
Proof Nodes
   ↓
Reconstruct Root
   ↓
Compare with stateRoot
```

If the reconstructed root matches the expected `stateRoot`, the proof can be verified.

---

# Deterministic Structure

Another important property of a Merkle Patricia Trie is that it is **deterministic**.

If two nodes have exactly the same underlying state, they should arrive at the same root.

```text
Same Data
    ↓
Same Trie
    ↓
Same Root
```

But if the data changes:

```text
Changed Data
     ↓
Changed Trie
     ↓
Different Root
```

This is extremely important for Ethereum because different nodes can independently process the same transactions and verify that they arrived at the same resulting state root.

---

# A Simple Conceptual Example

Suppose we have three accounts:

```text
Alice → 100 ETH
Bob   → 50 ETH
Carol → 200 ETH
```

These accounts become part of the State Trie:

```text
Account Data
     ↓
State Trie
     ↓
Root
```

Now Alice receives 10 ETH:

```text
Alice → 110 ETH
```

The affected data changes:

```text
Alice Balance Changes
        ↓
Account Node Changes
        ↓
Parent Node Changes
        ↓
Root Changes
```

Therefore:

```text
Old stateRoot ≠ New stateRoot
```

The new root represents the new state.

---

# The Big Picture

```text
                         Ethereum
                            │
                     Execution State
                            │
                       State Trie
                            │
                        stateRoot
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
      Accounts                         Contract Account
                                              │
                                              ↓
                                        Storage Trie
                                              │
                                        storageRoot
```

And at the block level:

```text
                         Block
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ↓             ↓             ↓
         State Trie   Transactions Trie  Receipts Trie
             │             │             │
             ↓             ↓             ↓
         stateRoot   transactionsRoot  receiptsRoot
```

---

# Merkle Tree vs. Merkle Patricia Trie

| Merkle Tree | Merkle Patricia Trie |
|---|---|
| Hash-based tree structure | Hash-based key-value trie |
| Good for collections of data | Good for key-value data |
| Simpler structure | More complex structure |
| Focuses on hashing and proofs | Combines key-value organization with hashing and proofs |
| Common example: Bitcoin transaction Merkle Tree | Important example: Ethereum execution-layer tries |

The relationship can be summarized as:

```text
Merkle Tree
      ↓
Basic Hash-Tree Concept
      ↓
Merkle Patricia Trie
      ↓
Ethereum Execution Layer
```

---

# An Important Note About Ethereum

When discussing Ethereum, it is more accurate to refer to the structure as a **modified Merkle Patricia Trie**.

Ethereum uses a modified version designed for its execution-layer data structures.

This discussion is specifically about Ethereum's **Execution Layer**. Ethereum's Consensus Layer uses different data structures and serialization mechanisms, including **SSZ (Simple Serialize)**.

---

# Conclusion

The **Merkle Patricia Trie** is one of the most important data structures in Ethereum's Execution Layer.

It combines:

```text
Patricia Trie
      +
Merkle Hashing
      ↓
Merkle Patricia Trie
```

The Patricia Trie provides an efficient way to organize **key-value data**.

The Merkle component provides **cryptographic verification** and allows large datasets to be represented by a root hash.

The most important relationships to remember are:

```text
State
  ↓
State Trie
  ↓
stateRoot
```

```text
Transactions
  ↓
Transactions Trie
  ↓
transactionsRoot
```

```text
Receipts
  ↓
Receipts Trie
  ↓
receiptsRoot
```

And for smart contract storage:

```text
Contract
   ↓
Storage Trie
   ↓
storageRoot
```

In one sentence:

> **Ethereum uses modified Merkle Patricia Tries in its Execution Layer to organize key-value data deterministically and provide cryptographically verifiable root commitments for important parts of the blockchain state.**

If we think of the **Merkle Tree** as the foundation of hash-based data verification, the **Merkle Patricia Trie** is the next step: it combines that cryptographic idea with an efficient structure for Ethereum's key-value state.

Once this concept is understood, fields such as `stateRoot`, `transactionsRoot`, `receiptsRoot`, and `storageRoot` become much easier to understand.
