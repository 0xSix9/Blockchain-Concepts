# Token
# Tokens and Standards on Ethereum: Why They Emerged and What Problem Each One Solves

## What is a token, exactly?

A token is a smart contract that manages the balance and transfer of a digital asset — not a separate file, but simply a **record in an on-chain database** (a mapping from a wallet address to a number). When we say "you have 100 tokens," it means that inside that token's contract, the number 100 is recorded next to your wallet address.

There are two main categories of tokens:

- **Fungible**: every unit is exactly interchangeable with another, just like money — one dollar is no different from another dollar. These are typically used for currencies, pool shares, or voting rights.
- **Non-Fungible (NFT)**: every unit is unique, like a painting or a specifically numbered ticket that can't be swapped one-for-one with another instance.

## Why did standards become necessary in the first place?

In Ethereum's early days (around 2015–2016), anyone who wanted to create a token wrote their own contract with completely arbitrary naming and structure. This caused two serious problems:

- **Exchanges and wallets** had no consistent way to interact with these tokens, because each one used a different name for the "transfer" or "balance" function — one used `transfer`, another `send`, another `moveTokens`.
- Every new contract that wanted to work with other tokens (say, a decentralized exchange or a lending protocol) had to write custom code for each individual token.

The solution was to define a **shared standard**: a set of required functions and behaviors that every token must implement, so that any contract or application aware of that standard could work with **any** token following it, without needing to know that token's specific custom code.

Important note: standards related to contract interfaces and tokens are known as **ERCs (Ethereum Request for Comment)**, and they don't require changes to Ethereum's core protocol — meaning only the community of projects and tools needs to adopt them, not the entire network.

## ERC-20: the fungible token standard

The first and most widely used token standard. It defines that every token must implement functions like `transfer`, `balanceOf`, `approve`, and `transferFrom`.

**Why did it emerge?** Because before it, every project (like the ICOs of that era) had its own arbitrary structure, and exchanges had to write custom code for every new coin. With ERC-20, an exchange writes support for the standard once, and from then on it automatically works with thousands of different tokens that follow the same standard.

## ERC-721: the NFT standard

The problem: ERC-20 assumed every unit of a token was completely identical, since it only stores a single balance number. But for a unique asset (like a digital artwork), each item needs its own **distinct, unique identifier**.

ERC-721 solved exactly this problem: every token has its own separate `tokenId`, instead of just a shared balance number with other tokens in the same contract.

## ERC-1155: the multi-token standard

The problem: if a game has both repeatable items (like coins or arrows) and unique items (like a legendary sword), it either has to deploy two separate contracts (one ERC-20, one ERC-721), or use only ERC-721, which is very inefficient for repeatable items since each unit has to be minted separately and consumes a lot of gas.

ERC-1155 came along to support both fungible and non-fungible assets **within a single contract**, and more importantly, it introduced **batch transfers** — moving several different items in a single transaction, which saves significant gas.

## ERC-4626: the tokenized vault standard

The problem: DeFi protocols (like Yearn or Aave) each had their own custom method for "depositing an asset and receiving a share of a pool." A developer wanting to integrate with 10 different protocols had to learn 10 different interfaces.

ERC-4626 defined a shared standard for "deposit an asset, receive a share-representing token, and withdraw later," so that any vault following it automatically becomes compatible with any other tool that understands ERC-4626 (like yield aggregators).

## ERC-2612 (Permit): signing instead of a transaction for approval

The problem: with a regular ERC-20, for another contract (say, a decentralized exchange) to move your tokens, you first had to send a separate transaction (`approve`) and then the main transaction (`transferFrom`) — meaning two transactions and paying gas twice.

ERC-2612 lets the user provide just a **signature** (no transaction, no gas cost), and the destination contract itself accepts that signature as authorization. This noticeably improved the user experience and made it cheaper.

## EIP-712: signing structured, readable data

The problem: signing raw byte data was displayed in the wallet as an unintelligible hexadecimal string — the user genuinely couldn't tell what exactly they were signing, which created an opening for phishing attacks.

EIP-712 defined a standard so that data is displayed in the wallet in a **structured, readable** form (like a form with clear fields, e.g., "To: 0x..., Amount: 100"), so the user actually understands what they're confirming and signing.

## An important note on enforcement and adoption

There is no central authority that "enforces" a standard. Even once a standard is fully accepted, its real-world implementation depends entirely on developers, wallets, and exchanges **voluntarily** following it. This is exactly why ERC-20 succeeded: because everyone — wallets, explorers, exchanges — supported it, it now feels effectively mandatory, not because there's an official law behind it.

## What about other chains?

Every non-EVM blockchain has its own token standard, because their virtual machine and accounting model differ:

- **Solana** → the **SPL Token Standard**; instead of a separate contract for each token, everything uses a shared Token Program.
- **TRON** → **TRC-20**; nearly a copy of ERC-20 with minor changes.
- **BNB Smart Chain** → **BEP-20**; effectively a copy of ERC-20 under a different name (since BSC is also EVM-compatible).
- **Cosmos ecosystem** → **CW-20** on CosmWasm.

An interesting pattern: most new chains trying to easily attract Ethereum developers copy the exact ERC-20/721 model and just rename it, because this standard has effectively become a **shared industry language**, not something exclusive to Ethereum.

---

**The underlying pattern:** every standard covered in this article emerged because of a genuine **compatibility or coordination problem** — either exchanges couldn't easily work with different tokens, or a new type of asset (like NFTs) needed a different data model, or the existing user experience had security or cost issues. A standard is a "shared language" that lets everything in the ecosystem work together without requiring manual coordination between projects.
