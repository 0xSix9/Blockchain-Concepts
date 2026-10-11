# Understanding Blobs on Ethereum: How EIP-4844 Reshaped Layer-2 Scaling
![blob](https://github.com/0xSix9/blob/blob/2d5aff522deddba99d7fbfebf30bf2a7341844c2/img/blob.png)
## Introduction

For years, Ethereum's biggest weakness was cost. As the network grew more popular, transaction fees climbed, and Layer-2 rollups — which bundle thousands of transactions and post compressed proofs back to Ethereum — still had to pay steep prices to publish their data as regular "calldata" on the main chain. In March 2024, the Dencun upgrade introduced a solution: **blobs**, defined by Ethereum Improvement Proposal 4844 (EIP-4844), also known as "proto-danksharding." Blobs created a cheaper, purpose-built lane for rollup data, and in doing so, transformed the economics of scaling Ethereum.

## What Exactly Is a Blob?

A blob is a large chunk of binary data — exactly 131,072 bytes (128 KB) — attached to a new kind of transaction called a **Type 3 (blob-carrying) transaction**. Structurally, each blob is made up of 4,096 "field elements" of 32 bytes each, and it's secured using KZG polynomial commitments, a cryptographic technique that lets the network verify a blob's availability without every node having to permanently store or even directly read its contents.

Crucially, blob data is **not accessible to the Ethereum Virtual Machine (EVM)**. Smart contracts can't read blob contents directly; they can only reference a blob via its cryptographic commitment hash. This is a deliberate design choice: blobs exist purely for data availability, not for computation, which is exactly what rollups need — a place to publish transaction data so it can be verified and reconstructed if necessary, without burdening every Ethereum node with it forever.

Another key feature is that blobs are **temporary**. Rather than being stored permanently like calldata, blobs are pruned by consensus clients after roughly 18 days. This is enough time for anyone who needs to challenge or verify a rollup's data to do so, while keeping the long-term storage burden on the network manageable.

## Why Blobs Matter: The Fee Problem They Solved

Before EIP-4844, rollups posted their transaction data as calldata within ordinary Ethereum transactions. Calldata is stored permanently on-chain and competes for space with all other Ethereum activity, meaning rollups paid the same congested gas market as everyone else. This made L2 transactions far more expensive than they needed to be.

Blobs introduced a **separate, independent fee market**. Instead of competing with regular transactions for blockspace, blob transactions have their own base fee that rises and falls based on blob-specific supply and demand, governed by an EIP-1559-style mechanism. When blocks consistently carry more blobs than the target amount, the blob base fee rises; when usage falls below target, it decreases — often toward a minimal 1 wei during quiet periods.

The result, when Dencun launched, was dramatic: Layer-2 transaction costs on networks like Optimism, Arbitrum, and Base dropped by more than 95% within days.

## How Blob Capacity Has Evolved

At launch, Ethereum blocks targeted **3 blobs** per block, with a hard maximum of **6 blobs** (0.375 MB target, 0.75 MB max). This was intentionally conservative — a way to test the mechanism without straining node hardware and network bandwidth.

Demand, however, grew quickly. By late 2024, blocks were regularly bumping against the 3-blob target, and demand spikes (such as a November 2024 surge tied to increased L2 activity) pushed the blob base fee up sharply, showing that the initial capacity was too tight for a maturing rollup ecosystem.

Ethereum's roadmap responded with successive capacity increases:

- **EIP-7691**, deployed with the **Pectra** upgrade (May 2025), raised the target to 6 blobs and the maximum to 9 blobs per block.
- The **Fusaka** upgrade (December 2025) introduced **PeerDAS** (Peer Data Availability Sampling), a technique that lets individual nodes verify blob availability by sampling only small portions of the data rather than downloading it in full. This made it safe to scale blob counts further without every node bearing the full bandwidth cost.
- Following Fusaka, Ethereum began using **Blob Parameter Only (BPO) forks** — lightweight forks that can adjust blob target and maximum values without a full network upgrade — to push capacity from 6/9 up toward a target of 14 and maximum of 21 blobs per block.

The long-term vision, full **Danksharding**, aims to eventually support around 128 blobs per block, at which point Ethereum's data availability capacity would rival that of dedicated data-availability layers.

## The Bigger Picture: Ethereum's Rollup-Centric Roadmap

Blobs are the centerpiece of Ethereum's strategy to scale through Layer 2s rather than by simply making the base layer itself process more transactions. Instead of Ethereum directly executing millions of transactions, rollups handle execution off-chain and periodically publish compressed data back to Ethereum for security and availability. Blobs make that publishing step cheap and scalable, which is why blob throughput is treated as one of the most important scaling levers on Ethereum's roadmap — alongside execution-layer improvements planned for the upcoming **Glamsterdam** upgrade.

The rise of blobs has also fueled competition and complementary innovation in the broader data-availability space, with alternative solutions like Celestia and EigenDA offering rollups additional options beyond Ethereum's native blob market.

## Conclusion

EIP-4844's blobs represent one of the most consequential upgrades in Ethereum's recent history — not because they change what Ethereum can compute, but because they redefine how cheaply and efficiently the network can make rollup data available and verifiable. What began as a conservative 3-blob target in March 2024 has, through Pectra, Fusaka, and successive Blob Parameter Only forks, grown substantially, with much more scaling still to come on the road toward full Danksharding. For users of Layer-2 networks, this translates directly into lower fees and a more scalable Ethereum ecosystem overall.
