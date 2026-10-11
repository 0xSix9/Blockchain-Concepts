# Digital Signature Algorithms and ECDSA: How Blockchain Transactions Are Signed
![ECDSA](https://github.com/0xSix9/ecdsa/blob/07744c7bf39ea0e54456bf84742743b854b4635d/img/ecdsa.png)
Digital signatures are one of the fundamental cryptographic technologies behind modern blockchains.

Whenever a user sends a transaction from a cryptocurrency wallet, the network needs a way to answer an important question:

> **How can we prove that this transaction was actually authorized by the owner of the account?**

This is where **digital signatures** come into play.

One of the most important digital signature algorithms used in blockchain systems is **ECDSA**, which stands for **Elliptic Curve Digital Signature Algorithm**.

In this article, we will start with the concept of digital signatures and then understand how ECDSA works and why it is important in blockchain systems.

---

## 1. What Is a Digital Signature?

A digital signature is a cryptographic mechanism used to prove that a message or transaction was created and authorized by the owner of a particular private key.

It provides three important properties:

### 1.1 Authentication

A digital signature can prove that the message was signed by the owner of the corresponding private key.

### 1.2 Integrity

A signature helps ensure that the signed data has not been modified after it was signed.

If someone changes the transaction, the signature will no longer be valid.

### 1.3 Non-Repudiation

In the traditional cryptographic sense, a valid digital signature provides evidence that the holder of the private key authorized the signed data.

However, in blockchain systems, the exact legal meaning of non-repudiation depends on the surrounding system and jurisdiction.

---

# 2. Digital Signatures Are Not Encryption

One common misunderstanding is that digital signatures are used to encrypt data.

They are not.

**Encryption** is primarily used to provide confidentiality.

For example:

```text
Plaintext
   ↓
Encryption
   ↓
Ciphertext
```

The goal is to prevent unauthorized people from reading the data.

Digital signatures have a different purpose:

```text
Message
   ↓
Digital Signature
   ↓
Proof of Authorization
```

The goal is to prove who authorized the data and verify that the data has not been modified.

---

# 3. Digital Signature Algorithms

A **Digital Signature Algorithm** is a cryptographic algorithm that allows a user to create and verify digital signatures.

A typical digital signature system contains three main operations:

```text
Key Generation
      ↓
Signature Generation
      ↓
Signature Verification
```

Let's look at each one.

---

# 4. Key Generation

The first step is generating a key pair:

* Private Key
* Public Key

The private key must remain secret.

The public key can be shared with other participants.

Conceptually:

```text
Private Key
     │
     ▼
Public Key
```

The public key is mathematically related to the private key, but it should not be possible to efficiently derive the private key from the public key.

This one-way relationship is fundamental to public-key cryptography.

---

# 5. Signature Generation

Suppose Alice wants to authorize a transaction.

She first creates the transaction data.

For example:

```text
Send 1 BTC
From: Alice
To: Bob
```

The transaction is then processed using a cryptographic hash function.

Conceptually:

```text
Transaction
     ↓
Hash Function
     ↓
Message Hash
```

The message hash is then used together with Alice's private key to generate a digital signature.

```text
Message Hash + Private Key
            ↓
     Signature Algorithm
            ↓
      Digital Signature
```

The important point is that Alice does **not** reveal her private key.

She only publishes the signature.

---

# 6. Signature Verification

Now the network receives:

* The transaction
* Alice's public key
* The digital signature

The network can use the public key to verify the signature.

Conceptually:

```text
Transaction
     │
     ▼
  Hashing
     │
     ▼
Message Hash
     │
     │
     ├───────────────┐
     │               │
     ▼               ▼
Signature       Public Key
     │               │
     └───────┬───────┘
             ▼
       Verification
             │
       ┌─────┴─────┐
       ▼           ▼
     Valid       Invalid
```

If the signature is valid, the network can conclude that the transaction was authorized by someone possessing the corresponding private key and that the signed transaction data has not been altered.

---

# 7. What Is ECDSA?

**ECDSA** stands for:

> **Elliptic Curve Digital Signature Algorithm**

It is a digital signature algorithm based on **elliptic curve cryptography (ECC)**.

ECDSA combines the concept of digital signatures with the mathematical properties of elliptic curves.

It can be represented as:

```text
Digital Signature Algorithms
            │
            ▼
          ECDSA
            │
            ▼
Elliptic Curve Cryptography
```

ECDSA is not a hash function and it is not an encryption algorithm.

It is specifically a **digital signature algorithm**.

---

# 8. Why Elliptic Curves?

Elliptic curve cryptography provides strong security with relatively small key sizes compared with some older public-key cryptography systems.

The security of ECC relies on difficult mathematical problems involving elliptic curves.

The important idea is that certain mathematical operations are easy to perform in one direction but computationally infeasible to reverse at the required scale.

This makes it possible to create a relationship such as:

```text
Private Key
     │
     ▼
Elliptic Curve Operation
     │
     ▼
Public Key
```

Computing the public key from the private key is practical.

Trying to recover the private key from the public key is computationally infeasible when the cryptographic parameters are secure.

---

# 9. The Basic Mathematical Idea Behind ECDSA

ECDSA uses an elliptic curve defined over a finite field.

A simplified form of an elliptic curve is:

```text
y² = x³ + ax + b
```

In actual cryptographic systems, the calculations are performed over a finite field rather than ordinary real numbers.

ECDSA also uses a specific base point on the curve, commonly represented as:

```text
G
```

A private key can be represented as a random integer:

```text
d
```

The corresponding public key is calculated using elliptic curve point multiplication:

```text
Q = dG
```

Where:

* `d` = private key
* `G` = generator/base point
* `Q` = public key

The important property is that calculating `Q` from `d` is easy, while recovering `d` from `Q` is computationally infeasible under the security assumptions of the curve.

---

# 10. How ECDSA Generates a Signature

At a high level, ECDSA signature generation works using:

* The private key
* The message hash
* A cryptographically secure random or deterministic nonce
* Elliptic curve operations

Suppose we have:

```text
Private Key = d
Message Hash = z
```

ECDSA generates two signature values:

```text
(r, s)
```

The pair `(r, s)` represents the digital signature.

A simplified conceptual flow is:

```text
Message
   ↓
Hash Function
   ↓
Message Hash
   ↓
ECDSA + Private Key
   ↓
Signature (r, s)
```

The private key is never included in the signature.

---

# 11. The Nonce in ECDSA

One of the most important components of ECDSA is the **nonce**, commonly represented as:

```text
k
```

The nonce is a secret value used during signature generation.

It is extremely important that the nonce is never reused improperly.

If the same nonce is reused with different messages, an attacker may be able to recover the private key.

Conceptually:

```text
Private Key
     +
Message Hash
     +
Secret Nonce
     ↓
ECDSA
     ↓
(r, s)
```

This is one of the most important security considerations when implementing ECDSA.

---

# 12. How ECDSA Verifies a Signature

The verifier does not need the private key.

Instead, it uses:

* The message
* The signature `(r, s)`
* The public key

The message is hashed again:

```text
Message
   ↓
Hash Function
   ↓
Message Hash
```

Then the ECDSA verification algorithm uses the message hash, signature, public key, and elliptic curve parameters to determine whether the signature is valid.

Conceptually:

```text
Message
   ↓
Hash
   ↓
Message Hash
        +
Signature (r, s)
        +
Public Key
        ↓
   ECDSA Verify
        ↓
   Valid / Invalid
```

If the signature is valid, the verifier accepts it.

---

# 13. ECDSA in Blockchain

Digital signatures are extremely important in blockchain systems because blockchains need a mechanism for users to authorize transactions without revealing their private keys.

Consider a simple transaction:

```text
Alice → Bob
Send 1 Coin
```

The process can be simplified as:

```text
Transaction
     ↓
Hash
     ↓
Message Hash
     ↓
ECDSA + Alice's Private Key
     ↓
Digital Signature
     ↓
Broadcast to Network
```

Nodes can then verify the transaction using Alice's public key.

```text
Transaction
     +
Signature
     +
Public Key
     ↓
Verification
     ↓
Valid?
```

If the signature is invalid, the transaction can be rejected.

---

# 14. ECDSA and Cryptocurrency Wallets

Cryptocurrency wallets do not simply store coins.

In many blockchain systems, a wallet manages cryptographic keys that allow a user to authorize transactions.

A simplified model is:

```text
Private Key
     ↓
Public Key
     ↓
Address
```

When a user wants to send assets, the wallet uses the private key to create a digital signature.

The private key remains under the user's control.

The blockchain network only needs the information necessary to verify the signature.

This is why protecting the private key is critical.

If an attacker obtains the private key, they may be able to create valid signatures and authorize transactions.

---

# 15. ECDSA in Bitcoin

Bitcoin uses elliptic-curve-based digital signatures.

Historically, Bitcoin uses **ECDSA with the secp256k1 curve** for its traditional signature scheme.

The basic relationship is:

```text
Private Key
     ↓
secp256k1
     ↓
Public Key
     ↓
Bitcoin Address
```

When a transaction is authorized, the corresponding private key is used to produce a signature.

Bitcoin later introduced **Schnorr signatures** through Taproot, so ECDSA is not the only signature scheme used in Bitcoin today.

---

# 16. ECDSA in Ethereum

Ethereum also uses **ECDSA with the secp256k1 elliptic curve** for externally owned accounts (EOAs).

When an Ethereum user sends a transaction, the transaction is signed using the account's private key.

Conceptually:

```text
Ethereum Transaction
        ↓
Transaction Data
        ↓
Hashing
        ↓
ECDSA
        ↓
Signature
        ↓
Broadcast
```

The network can then verify that the transaction was authorized by the corresponding account.

---

# 17. ECDSA Does Not Mean "The Blockchain Is Encrypted"

It is important to understand the difference between these concepts.

A blockchain can use several different cryptographic and networking technologies, but each one has a different job.

### Hash Function

Used to create a fixed-size fingerprint of data and help detect changes.

### Digital Signature

Used to prove that a transaction or message was authorized by the holder of a private key.

### ECDSA

Used to create and verify digital signatures.

### Public Key

Used by others to verify a digital signature.

### Private Key

Used by the owner to create a digital signature. It must remain secret.

### Consensus Mechanism

Used by the blockchain network to agree on the valid state of the blockchain.

Therefore, **ECDSA does not encrypt blockchain transactions**. Its job is to create and verify digital signatures.

---

# 18. ECDSA vs Hash Functions

ECDSA and hash functions are often used together, but they are fundamentally different.

A hash function takes data and produces a fixed-size output:

```text
Data
 ↓
Hash Function
 ↓
Hash
```

A digital signature algorithm uses cryptographic keys to create and verify signatures:

```text
Message Hash
     +
Private Key
     ↓
ECDSA
     ↓
Signature
```

So the two technologies complement each other.

A blockchain transaction may be hashed first and then signed.

---

# 19. Why Can't We Just Use the Public Key?

The public key is intentionally public.

If someone could create a valid signature using only the public key, anyone could authorize transactions on behalf of the owner.

The security model therefore depends on this relationship:

```text
Private Key
     │
     │ creates
     ▼
Signature
     │
     │ verified using
     ▼
Public Key
```

The private key creates the proof.

The public key verifies the proof.

---

# 20. A Simple Real-World Analogy

Imagine Alice has a unique physical stamp.

The stamp is private and only Alice possesses it.

Alice uses the stamp to authorize a document.

Other people cannot create the same stamp, but they can recognize whether the stamp on a document is authentic.

Digital signatures work in a similar conceptual way.

```text
Private Key
    ↓
Creates Signature

Public Key
    ↓
Verifies Signature
```

The cryptographic version is much more mathematically sophisticated than a physical stamp, but the analogy helps explain the basic idea.

---

# 21. The Complete Blockchain Transaction Flow

We can now combine everything we have learned.

Suppose Alice wants to send cryptocurrency to Bob.

### Step 1 — Create Transaction

```text
Alice → Bob
Amount: 1 Coin
```

### Step 2 — Hash Transaction Data

```text
Transaction
     ↓
Hash Function
     ↓
Message Hash
```

### Step 3 — Sign With Private Key

```text
Message Hash
     +
Private Key
     ↓
ECDSA
     ↓
Signature
```

### Step 4 — Broadcast

Alice sends the transaction and signature to the network.

```text
Transaction
+
Signature
+
Required Public-Key Information
        ↓
      Network
```

### Step 5 — Verify

Nodes verify the signature.

```text
Transaction
+
Signature
+
Public Key
        ↓
   ECDSA Verify
        ↓
   Valid / Invalid
```

### Step 6 — Accept or Reject

If the signature is valid and the transaction satisfies the blockchain's other rules, the transaction can continue through the network's transaction-processing and consensus mechanisms.

---

# 22. The Most Important Security Rule

The most important rule when working with ECDSA is:

> **Never reveal your private key.**

The public key is designed to be shared.

The signature is designed to be shared.

The private key must remain secret.

If someone obtains your private key, they may be able to generate valid signatures and control the assets or accounts associated with that key.

---

# 23. ECDSA in the Bigger Cryptography Picture

It is useful to understand where ECDSA fits into the larger cryptographic landscape.

```text
Cryptography
│
├── Symmetric Cryptography
│
├── Asymmetric / Public-Key Cryptography
│   │
│   ├── Encryption
│   │
│   └── Digital Signatures
│       │
│       ├── ECDSA
│       ├── EdDSA
│       └── Schnorr
│
└── Hash Functions
```

ECDSA is therefore one specific digital signature algorithm within the broader field of cryptography.

---

# 24. ECDSA vs Other Signature Algorithms

ECDSA is not the only digital signature algorithm.

Other important signature schemes include:

* ECDSA
* EdDSA
* Schnorr signatures
* RSA signatures

Different blockchain systems may use different signature schemes depending on their design requirements.

For example:

```text
Bitcoin
→ ECDSA + Schnorr

Ethereum
→ ECDSA

Solana
→ Ed25519 / EdDSA
```

This distinction is particularly important when learning blockchain development because different ecosystems can use different cryptographic primitives.

---

# 25. ECDSA vs EdDSA

ECDSA and EdDSA are both digital signature algorithms based on elliptic-curve cryptography, but they use different mathematical constructions and curves.

### ECDSA

* **Type:** Digital signature algorithm
* **Example curve:** secp256k1
* **Used by:** Bitcoin and Ethereum
* **Purpose:** Create and verify digital signatures

### EdDSA

* **Type:** Digital signature algorithm
* **Example curve:** Ed25519
* **Used by:** Solana
* **Purpose:** Create and verify digital signatures

The important point is that **ECDSA is not the universal signature algorithm for every blockchain**.

Different blockchains make different cryptographic design choices.

---

# 26. Final Mental Model

The easiest way to remember ECDSA is:

```text
Private Key
     ↓
   ECDSA
     ↓
Digital Signature
     ↓
Blockchain Network
     ↓
Public Key
     ↓
Verification
     ↓
Valid / Invalid
```

Or even more simply:

```text
Private Key → Sign

Public Key → Verify
```

That is the core idea.

ECDSA provides a way for a user to prove cryptographically that they control a private key associated with a public key, without revealing the private key itself.

This makes digital signatures a fundamental component of many blockchain systems.

---

# Conclusion

Digital signatures are one of the most important building blocks of blockchain technology.

They allow users to authorize transactions while keeping their private keys secret. They also provide mechanisms for verifying that signed data has not been modified.

**ECDSA (Elliptic Curve Digital Signature Algorithm)** is one of the most widely known digital signature algorithms in blockchain technology. It uses elliptic-curve cryptography to generate and verify signatures and has been an important part of systems such as Bitcoin and Ethereum.

The fundamental relationship to remember is:

```text
Private Key
     ↓
Creates Signature

Public Key
     ↓
Verifies Signature
```

Once this concept is understood, many other blockchain concepts become easier to understand, including wallets, transactions, account ownership, transaction authorization, public and private keys, and cryptographic verification.

The most important distinction is also worth remembering:

> **The private key proves control by allowing the owner to create a signature, while the public key allows everyone else to verify that signature.**

And that is the fundamental role of digital signatures in blockchain systems.
