# Accounts
# Accounts in Ethereum: Everything You Need to Know

In previous articles, we covered the Ethereum Virtual Machine, its architecture, and how opcodes get executed. But there's a foundational concept that everything else revolves around, one we haven't tackled on its own yet: the **account**.

Every transaction, every smart contract, every ETH transfer ultimately happens between two or more **accounts**. In this article, we're going to take a thorough look at what an account actually is in Ethereum, what types exist, and what it's made of.

## What Is an Account in Ethereum?

Unlike Bitcoin, which is built on the UTXO model (a collection of unspent outputs from previous transactions), Ethereum uses a completely different approach called the **account-based model**.

In this model, every account is an independent entity with a unique address and a defined state — much like a bank account that holds a balance, a transaction history, and information specific to itself. Ethereum's entire state is essentially maintained as a collection of these accounts.

Every account is identified by a **20-byte address (40 hexadecimal characters)**, something like:

```
0x742d35Cc6634C0532925a3b844Bc454e4438f44e
```

## The Two Main Types of Accounts

Ethereum has exactly **two** types of accounts. Structurally they look similar, but behaviorally and in terms of control, they're completely different.

### 1. EOA (Externally Owned Account)

An **EOA** is an account controlled by a person (or any entity outside the blockchain) through a **private key**. This is the type of account that gets created the moment you set up a wallet like MetaMask.

Key characteristics of an EOA:
- Created from a key pair (private/public).
- Its address is derived from a hash of the public key.
- **Has no code** — it can't contain any programming logic.
- Can **initiate transactions** (something a smart contract cannot do on its own).
- Every action it takes must be signed with its private key.

Put simply: an EOA is **you**. Every action on Ethereum — sending ETH, calling a contract, deploying a new contract — starts from an EOA.

### 2. Contract Account

A **contract account** is created when a smart contract gets deployed. Unlike an EOA:

- It has **no private key** — no human directly controls it.
- It has **code** (bytecode) — the actual logic you wrote in your contract.
- It only executes when a transaction (from an EOA or from another contract account) calls it.
- It cannot initiate a transaction on its own; it always acts in response to a call from somewhere else.

Put simply: a contract account is **code**. An automated program that only springs into action when called, and behaves exactly according to the logic written inside it.

## Quick Comparison Table

| Feature | EOA | Contract Account |
|---|---|---|
| Controlled by | Private key (a human) | Contract code |
| Has code? | No | Yes |
| Can initiate a transaction? | Yes | No (only responds) |
| How it's created | Generating a private/public key pair | Deploying a contract |
| Example | Your MetaMask wallet | An ERC-20 contract |

## What's Inside Every Account's State?

Now that we know the two account types, let's look at exactly what information every account — whether EOA or contract — keeps in its state. According to the Ethereum protocol specification, every account consists of four core fields:

### Nonce
An integer that counts the number of transactions sent from this account (for an EOA), or the number of contracts this account has created (for a contract account).

The nonce plays a critical role: preventing **replay attacks** (rebroadcasting an already-signed transaction). Because every transaction must be sent with exactly the next expected nonce, no one can rebroadcast an old transaction and have it accepted again.

### Balance
This account's ETH balance, denominated in the smallest unit of ether, called **wei** (1 ETH equals 10 to the power of 18 wei).

### Storage Root
The root of a Merkle Patricia trie representing all the data this account has stored in **storage**. For EOAs, this field is empty, since an EOA has no storage data. But for contract accounts, this is exactly where all of a contract's permanent variables live (like users' token balances in an ERC-20 contract).

### Code Hash
The hash of this account's bytecode. For EOAs, this is always the hash of an empty string (since there's no code). For contract accounts, this hash points to the contract's actual bytecode, which is stored elsewhere in the node's database.

## How Are Account Addresses Generated?

### EOA Address
1. A random 256-bit private key is generated.
2. Using elliptic curve cryptography (ECDSA), a public key is derived from it.
3. The public key is hashed using Keccak-256.
4. The last 20 bytes of that hash become the final account address.

### Contract Account Address
A new contract's address is computed from a combination of **the address of the sender that deployed it** and **that sender's nonce at the moment of deployment** (hashed together using Keccak-256). Because of this, a contract's address is actually predictable even before it's deployed — something that has real practical use in advanced patterns like CREATE2.

## How Do Accounts Interact With Each Other?

We already covered contract-to-contract communication in an earlier discussion; let's revisit it here through an account-centric lens:

- An **EOA** can send ETH directly to another EOA (a simple transfer).
- An **EOA** can call a function on a **contract account** (like depositing into a staking contract).
- A **contract account** can call a function on another **contract account** (like a DEX contract talking to a token contract).
- But a **contract account** can never initiate anything on its own without being called first. There's always an EOA at the very beginning of every transaction chain.

That last point matters a lot: no matter how long a chain of contract-to-contract calls gets (A calls B, B calls C, and so on), **the starting point is always an EOA**.

## A Few Practical Notes

### An EOA Can Have Multiple Addresses
There's no restriction preventing one person from creating multiple EOAs. Most users have several wallets (different addresses), each derived from a different private key.

### Accounts Can (Almost) Never Be Deleted
There used to be an opcode called `SELFDESTRUCT` that allowed a contract to remove itself from the blockchain and send its remaining ETH to another address. However, this behavior has been heavily restricted in recent Ethereum upgrades and no longer actually destroys a contract — it only transfers its ETH balance.

### Empty Accounts
An address might never have been formally "created," yet it's still a valid address — because until someone sends it ETH or a transaction originates from it, it simply doesn't exist in Ethereum's state. The moment it sends or receives its first transaction, the account automatically appears in the state.

## Summary

In this article, we covered the concept of accounts in Ethereum:

- Ethereum runs on an **account-based model**, unlike Bitcoin's UTXO model.
- There are two account types: **EOA** (controlled by a private key, no code) and **contract account** (controlled by code, no private key).
- Every account consists of four core fields: **nonce**, **balance**, **storage root**, and **code hash**.
- An EOA's address is derived from a public key; a contract account's address is derived from its deployer's address and nonce.
- Every chain of contract-to-contract calls always starts with an EOA.

Understanding this concept properly lays the groundwork for many other topics — from how wallets actually work to more advanced mechanisms like proxy contracts or multisig accounts, which we can explore in future articles.


---

## Related Deep Dive

- [Multisig Contracts: Owners, Thresholds, Signatures, and Security](01-multisig-contracts/README.md) — a developer-focused guide to M-of-N authorization, signature hashing, execution flow, and audit properties.
