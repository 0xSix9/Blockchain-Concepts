# The Architecture of the Ethereum Virtual Machine
In the previous article, we covered the general concept of the Ethereum Virtual Machine (EVM): a global, decentralized, deterministic virtual computer on which smart contracts run. Now it's time to go a bit deeper and see exactly what happens under the hood.

## The EVM Is a Stack-Based Machine

The first thing to understand about the EVM's architecture is that it operates on a **stack**, not on registers like most conventional computer processors.

In a register-based machine (like the CPU in your laptop), data is held in named slots such as `R1` or `R2`, and instructions refer to them directly. In a stack-based machine, everything is read from and written to the top of a stack — much like a stack of plates, where you can only take from or add to the top.

Why did Ethereum choose this model? Because designing, implementing, and security-auditing a stack-based machine is simpler — and simplicity carries enormous value when something needs to behave identically across thousands of independent nodes.

The EVM's stack has a fixed limit: a maximum of **1,024 items**, each exactly **256 bits (32 bytes)** in size. That number isn't arbitrary — 256 bits is a perfect fit for working with Keccak-256 hashes and large cryptographic numbers (like public keys).

## Three Core Data Spaces

To manage data during transaction execution, the EVM relies on three distinct spaces, each with its own role, cost, and lifespan:

| Space | Description | Gas Cost | Lifespan |
|---|---|---|---|
| **Stack** | Up to 1,024 items, 256 bits each | Very cheap | Only during a single execution |
| **Memory** | Temporary, addressable byte array | Moderate, grows non-linearly | Only during a single execution |
| **Storage** | Permanent key-value store on the blockchain | Very expensive | Forever |

### Stack
The fastest and cheapest space. Most arithmetic and logical operations (addition, subtraction, comparisons) happen directly on this stack.

### Memory
A temporary space similar to RAM that gets wiped clean every time a function starts executing. It's used to hold data like arrays or strings during a function's execution. It costs more than the stack, and the cost per additional byte grows as usage increases.

### Storage
The only space that is actually recorded on the blockchain and persists forever. This is where, for example, a user's token balance is stored in an ERC-20 contract. Because it must be permanently maintained by every single node, writing to it (via an opcode called `SSTORE`) is one of the **most expensive** operations in the EVM — it can cost up to 20,000 gas units, compared to a simple addition, which costs only 3.

This stark cost difference is exactly why experienced developers always try to minimize storage usage and rely on memory or calldata as much as possible instead.

## Opcodes and Bytecode: The EVM's Real Language

Solidity code is compiled into **bytecode**: a sequence of bytes that the EVM can execute. Bytecode is commonly displayed in hexadecimal. Some bytes encode EVM instructions (opcode bytes); other bytes may be immediate data consumed by an instruction. **Not every byte in bytecode is a separate opcode.**

For example, the EVM opcode byte `0x60` identifies `PUSH1`. The byte that follows it is read as a one-byte value to push onto the stack. By contrast, `0x01` identifies `ADD`. The readable names `PUSH1` and `ADD` are called **mnemonics**. A disassembler or a tool such as [EVM.codes](https://www.evm.codes/) interprets bytecode and displays these instructions in a human-readable form; the EVM itself does not need a disassembler to execute code.

For the byte-by-byte breakdown, hexadecimal notation, offsets, and a worked example, see [Bytecode and Opcodes](../05-bytecode-and-opcodes/README.md).

A few commonly used opcodes:

```
PUSH1 0x60     ; pushes a one-byte value onto the stack
ADD            ; adds the top two values on the stack
MSTORE         ; writes a value to memory
SSTORE         ; writes a value to storage (expensive!)
SLOAD          ; reads a value from storage
CALL           ; calls another contract
DELEGATECALL   ; calls another contract using the caller's own context
REVERT         ; halts execution and undoes all changes
```

For example, this simple Solidity function:

```solidity
function add(uint a, uint b) public pure returns (uint) {
    return a + b;
}
```

Produces something like this at the opcode level (simplified for clarity):

```
PUSH1 0x00
CALLDATALOAD
PUSH1 0x20
CALLDATALOAD
ADD
```

You can inspect the bytecode of any real contract on Ethereum via Etherscan, or step through a function opcode by opcode using a tool like Remix's debugger.

## Gas Metering: Cost at the Opcode Level

In the previous article we mentioned that every transaction has a gas cost. At the architectural level, what does that actually mean?

Every opcode has a fixed, predetermined cost defined by the Ethereum protocol. As the EVM executes a contract's bytecode, it moves opcode by opcode and deducts from your gas limit accordingly. If gas runs out at any point, execution halts immediately (an "out of gas" error), and all state changes are rolled back — but the gas already consumed up to that point is still lost.

This mechanism, known as **gas metering**, is precisely what prevents infinite loops and denial-of-service attacks: no matter how many times an operation repeats, it carries a real cost that must be paid.

## Global State and the Merkle Patricia Trie

Beyond executing instructions, the EVM also has to manage a **global state** — information about every account that exists on the network.

Ethereum has two types of accounts:

- **EOA (Externally Owned Account)**: ordinary user accounts controlled by a private key.
- **Contract Account**: smart contract accounts that have code and storage but no private key.

Every account includes four core fields:

```
nonce        → number of transactions sent from this account
balance      → ETH balance
storageRoot  → root of the Merkle tree for this account's storage
codeHash     → hash of the contract's code (empty for EOAs)
```

Rather than being stored as a simple list, this massive state is maintained in a data structure called a **Merkle Patricia Trie**. This structure allows nodes to quickly prove that a particular value exists (or doesn't exist) in the state, without needing to download the entire dataset — something critical for both scalability and security.

## The Full Lifecycle of a Transaction

Now that we've covered the individual pieces, let's walk through exactly what happens, step by step, when a transaction is sent:

1. The user signs a transaction with their private key and broadcasts it to the network.
2. The transaction enters the **mempool** (a temporary queue of pending transactions).
3. A block producer (validator) selects this transaction and includes it in the next block.
4. Every node on the network executes the corresponding bytecode on its own local instance of the EVM.
5. During execution, opcodes are processed one by one, and gas is deducted along the way.
6. If execution completes successfully, the changes are applied to the global state and become permanent.
7. If an error occurs anywhere (like a revert or running out of gas), all state changes are undone — but the gas already spent is still charged to the user.
8. Ultimately, all nodes must arrive at the same state for the block to be finalized.

## A Key Architectural Concern: Reentrancy

One direct consequence of the EVM's architecture is a well-known vulnerability called **reentrancy**. When a contract hands off execution control to another contract via opcodes like `CALL`, that destination contract can turn around and call back into the original function — before the first contract's state has been updated.

This is precisely the weakness exploited in the infamous **DAO Hack** of 2016, which resulted in the loss of millions of dollars. Understanding why this attack is possible requires understanding the EVM's call architecture and execution ordering — it's a direct product of the machine's design.

## Summary

In this article, we went deeper into the EVM's internal structure:

- The EVM is a stack-based machine, not a register-based one.
- The stack, memory, and storage each carry different costs and lifespans.
- The code that actually executes is a sequence of simple, low-level opcodes.
- Every opcode has a specific gas cost, and their sum determines the total cost of a transaction.
- The entire network's state is maintained in a structure called the Merkle Patricia Trie.
- Understanding this architecture is the key to understanding security vulnerabilities like reentrancy.

With these two articles, we now have a complete picture of the EVM — both conceptually and technically. Any further topics (deeper security analysis, the EVM's evolution through EIPs, or comparisons with other virtual machines) can become the subject of future, standalone articles.
