# Transaction
# What Is a Transaction in Ethereum?

In previous articles, we covered the Ethereum Virtual Machine, its architecture, and accounts. But so far we haven't directly tackled the thing that kicks the whole story off: the **transaction**. Everything that happens on Ethereum — from a simple ETH transfer to the most complex smart contract call — begins with a transaction.

## What Exactly Is a Transaction?

A **transaction** is a signed message sent to the Ethereum network from an **EOA** (an account controlled by a private key). This message does one of three things:

1. **Transfers ETH** (from one account to another).
2. **Calls a smart contract** (invokes one of its functions).
3. **Deploys a new contract**.

Here's the key point: the only thing that can initiate a transaction is an EOA. A smart contract can never start a transaction on its own without being called first — something we already touched on in the article about accounts.

## First Things First: How Many Account Types Are There?

Since the entire concept of a transaction revolves around accounts, it's worth pausing to remind ourselves that Ethereum has exactly **two account types** — because the account type directly determines who can send a transaction and who can't.

### 1. EOA (Externally Owned Account)
An account controlled by a **private key** — the same kind of account created the moment you set up a wallet like MetaMask. An EOA has no code and is run entirely by a person (or any entity outside the blockchain).

**The most important thing about an EOA in the context of transactions:** only an EOA can **initiate** a transaction. Every transaction you see on the Ethereum network ultimately started from an EOA.

### 2. Contract Account
An account run by **code (bytecode)**, not a private key. This type of account comes into existence when a smart contract is deployed.

**The most important thing about a contract account in the context of transactions:** this type of account **can never initiate a transaction on its own**. It only springs into action once a transaction (from an EOA, or via a call from another contract) reaches it.

### Why Does This Distinction Matter for Understanding Transactions?

Because it determines exactly where a transaction can originate from:

**Controlled by**
EOA: Private key (a human)
Contract Account: Contract code

**Can initiate a transaction?**
EOA: Yes
Contract Account: No, it only responds

**Requires a signature?**
EOA: Yes, with a private key
Contract Account: No signature; it's only called

So even when you see a long chain of contract-to-contract calls (say, a DEX contract calling a token contract, which in turn calls another contract), at the very start of that chain there's always a **real transaction from an EOA**. Without that initial EOA, none of those calls would have happened in the first place.

(If you want the full details on the structure of these two account types, their state fields, and how their addresses are generated, we cover that thoroughly in our standalone article, "Accounts in Ethereum.")

## A Transaction Is Different From a Call

Before diving into the details, let's clarify an important distinction: when you just want to **read** information from a contract (like checking a token balance for an address), that's a **call**, not a transaction. Calls:

- Don't change anything in the state.
- Don't require a signature or real gas payment (since no node is obligated to permanently execute and record it).
- Get answered immediately and locally by a single node.

Transactions, on the other hand, always change something in the state (even if that change is just a reduction in the sender's balance due to gas), which is exactly why they must be signed, included in a block, and processed by every node on the network.

## The Core Fields of a Transaction

Every transaction consists of several core fields:

### Nonce
The sequential number of this transaction relative to previous transactions from the same account. If your last transaction had a nonce of 5, the next one must have a nonce of exactly 6. This mechanism is precisely what prevents a replay attack (rebroadcasting an old, already-signed transaction).

### To (Destination)
The address receiving the transaction. This can be:
- Another EOA's address (a simple ETH transfer).
- A smart contract's address (calling a function).
- **Empty** — in which case this transaction means deploying a new contract.

### Value
The amount of ETH being transferred, in wei. For function calls that don't involve a payment, this can be zero.

### Data
The data sent along with the transaction. For a simple ETH transfer, this field is usually empty. For a function call, it contains the **function selector** (which specifies which function to run) along with that function's arguments.

### Gas Limit
The maximum amount of gas the sender is willing to spend on this transaction. If execution reaches this limit before finishing, the transaction halts with an "out of gas" error.

### Gas Price (or Max Fee / Priority Fee)
The price the sender is willing to pay per unit of gas. The finer details here (especially after EIP-1559) will be the subject of the next article.

### Signature (v, r, s)
Three values produced by signing the transaction with the sender's private key. This signature both guarantees the transaction's authenticity and allows the network to derive the sender's address directly from the signature, without the sender needing to state it explicitly.

## The Three Main Types of Transactions

Based on the `To` and `Data` fields, we can sort transactions into three categories:

### 1. Simple ETH Transfer
`To` is an EOA address, `Data` is empty, and only the `Value` gets transferred. The simplest and cheapest type of transaction in terms of gas.

### 2. Contract Call (Message Call)
`To` is a smart contract's address, and `Data` contains everything needed to run a specific function. This is by far the most common type of transaction in today's Ethereum ecosystem (token swaps, DeFi interactions, buying an NFT, and so on).

### 3. New Contract Deployment
`To` is empty, and `Data` contains the full bytecode of the new contract. The result of this transaction is a new contract account, whose address (as covered in the accounts article) is computed from the sender's address and their nonce.

## The Path of a Transaction, From Sending to Finalization

Let's walk through this path once more, this time focusing specifically on the transaction itself:

1. The user signs the transaction with their own private key.
2. The signed transaction is broadcast to one or more nodes.
3. Nodes place it in the **mempool** (a queue of pending transactions) and propagate it to other nodes as well.
4. A block producer (validator) selects this transaction from the mempool and includes it in the next block — transactions offering a higher priority fee are typically selected sooner.
5. The EVM executes the bytecode associated with this transaction.
6. The result (success or revert) gets recorded in the state.
7. The block gets finalized, and the transaction becomes irreversible.

## When Does a Transaction Fail?

There are two main failure scenarios worth distinguishing clearly right from the start:

### Revert
The contract itself decided to halt execution — usually because a condition failed (like a `require` or `revert` in Solidity). In this case, all state changes are rolled back, but the gas already consumed up to that point is still deducted from the sender.

### Out of Gas
The gas specified in the `gas limit` ran out before execution finished. The result is similar to a revert: all state changes are rolled back, but the entire gas amount is lost.

In both cases, the transaction still gets **recorded** on the blockchain (since an execution attempt did take place); it's just marked with a "failed" status.

## Summary

In this article, we covered the basic concept of a transaction in Ethereum:

- A transaction is a signed message from an EOA that transfers ETH, calls a contract, or deploys a new contract.
- A transaction is different from a call; only transactions change the state and require a real signature and gas.
- Every transaction consists of fields like nonce, to, value, data, gas limit, and a signature.
- There are three main transaction types: a simple transfer, a contract call, and a new contract deployment.
- A transaction can fail via a revert or by running out of gas, and in both cases it still gets recorded on the blockchain.

In the next article, we'll go deeper into the more technical details of transactions: exactly how signing works, the different transaction formats (including the changes introduced by EIP-1559), the precise gas calculation, and the receipt generated after every transaction.
