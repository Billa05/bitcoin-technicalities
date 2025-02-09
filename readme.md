## Index

1. [Cryptographic Hash Functions](#cryptographic-hash-functions)
2. [Digital Signatures](#digital-signatures)
3. [Bitcoin Addresses](#bitcoin-addresses)
4. [Wallets](#wallets)
5. [Transactions](#transactions)
6. [Proof-of-Work](#proof-of-work)
7. [Peer-to-Peer Network](#peer-to-peer-network)


# Cryptographic Hash Functions

## Course Structure
- **Cookie Tokens:** A simple but flawed system used as a foundation.
- **Progressive Enhancements:** Each chapter improves the system by addressing a weakness and introducing Bitcoin concepts.
- **Bitcoin Core:** By chapter 8, the "cookie token" system transforms into Bitcoin.
- **Labs:** Hands-on exercises involving:
  - Setting up a Bitcoin full node.
  - Verifying transactions.
- **Topics Covered:** 
  - Cryptographic hash functions  
  - Digital signatures  
  - Bitcoin addresses  
  - Wallets  
  - Transactions  
  - Blockchain  
  - Proof of work  
  - Peer-to-peer networks  
  - Segregated witness  
  - Soft forks  

## Cookie Tokens System
- **Central Authority (Lisa):** 
  - Manages a public spreadsheet tracking token balances.
  - Users can request Lisa to transfer tokens.
  - Only Lisa has write access.
- **Transactions:** 
  - Lisa verifies sender’s balance before updating the spreadsheet.
- **Token Creation:** 
  - Lisa rewards herself by creating new tokens.
  - Token creation rate decreases over time (similar to Bitcoin halving).
- **Flaws:** 
  - Centralized system—Lisa has full control.
  - Potential dishonesty.
  - Verification issues as the system scales.

## Cryptographic Hash Functions
- **Definition:** 
  - A function that takes an input of any size and produces a fixed-size output (hash).
- **Properties:**
  - **Deterministic:** Same input always results in the same output.
  - **Sensitive to Change:** Even a tiny change in input results in a drastically different output.
  - **Pre-image Resistance:** Given a hash, it is computationally infeasible to find the original input.
  - **Collision Resistance:** Finding two different inputs that produce the same hash is extremely difficult.
  - **Second Pre-image Resistance:** Given an input, finding another input with the same hash is infeasible.
- **Analogy:** 
  - Like a shredder: You can easily destroy a document (hash it), but reconstructing it is nearly impossible.
- **Use Case: Integrity Checks**
  - Hashes verify data integrity by comparing the original and later hash values.
  
### Key Takeaways:
- **Cryptographic Hash Functions:**
  - Helps in detecting tampering with data.
  - Input can be anything, but the output is always a fixed-length 256-bit number.
  - Any minor change in input results in a completely different hash.
- **Censorship Resistance:** 
  - A core principle behind Bitcoin.
- **Money Supply:** 
  - The total amount of currency in circulation.
- **Halving:** 
  - The periodic reduction in the rate at which new Bitcoin (or cookie tokens) are created.
- **Digital Signatures:**  
  - A mechanism to prove authenticity (covered in later sessions).

## Conclusion
- Cryptographic hash functions and digital signatures are fundamental to Bitcoin.
- Understanding these concepts is crucial for grasping how Bitcoin achieves security and decentralization.

---

# Digital Signatures

## Digital Signatures: An Overview
- **Key Pairs:**  
  - A **private key** (a large random number) is generated first.  
  - A **public key** is derived from the private key using a one-way function.  
  - The private key must remain secret, while the public key can be shared freely.  
- **Signing:**  
  - The sender uses their **private key** and the message to create a **digital signature**.  
  - The signature is unique to both the message and the private key.  
- **Verification:**  
  - The recipient verifies the signature using the **sender’s public key** and the message.  
  - If the verification succeeds, the message is confirmed to be from the sender.  
- **Analogy:**  
  - The private key is like a pen that signs a document.  
  - The public key is like a tool used to verify the authenticity of that signature.  

## Digital Signatures in Cookie Tokens
- **Identity Verification:**  
  - Lisa requires users to provide digital signatures for payment requests.  
  - This ensures the sender is who they claim to be.  
- **Process:**  
  1. **Key Pair Generation and Sharing:**  
     - Users generate a **private key** and derive a **public key**.  
     - They share the public key with Lisa, who stores it in a table.  
  2. **Signing Transactions:**  
     - When making a payment, the user signs a message (transaction details) with their private key.  
  3. **Verification by Lisa:**  
     - Lisa receives the message and its signature.  
     - She verifies the signature using the sender’s public key.  
     - If valid, she checks the sender’s balance and processes the transaction.  

## Relationship Between Private and Public Keys
- **One-Way Function:**  
  - The public key is derived from the private key.  
  - It is **computationally infeasible** to derive the private key from the public key.  
- **Encryption and Decryption:**  
  - **Encrypting with a public key** → Only the corresponding private key can decrypt (used for confidential communication).  
  - **Encrypting with a private key** → Anyone with the public key can decrypt (used for digital signatures).  

## Signing and Verification in Detail
- **Signing:**  
  - The message is **hashed** using a cryptographic hash function to generate a fixed-size output.  
  - The hash is then **encrypted with the private key**, creating the digital signature.  
  - Directly encrypting the message would be inefficient because of its variable size.  
- **Verification:**  
  - The recipient **hashes the received message**.  
  - They **decrypt the signature** using the sender’s public key to obtain the original hash.  
  - If both hashes match, the signature is valid, confirming the sender’s authenticity.  

## Example: Digital Signatures in Emails
- **Scenario:**  
  - A person, X, sends an email saying: **"Pay my salary, I am X."**  
  - X **hashes** the email content and **encrypts the hash** with their private key (this becomes the signature).  
  - The recipient verifies the signature by:  
    1. **Decrypting the signature** using X’s public key (available to everyone).  
    2. **Hashing the received message** independently.  
    3. **Comparing both hashes**—if they match, the message is verified as coming from X.  

## Key Concepts Introduced
- **Private Key:** A secret number used to create digital signatures.  
- **Public Key:** A number derived from the private key, used to verify signatures.  
- **One-Way Function:** A function that is easy to compute in one direction but infeasible to reverse.  
- **Prevention of Private Key Collision:**  
  - Private keys are **256-bit random numbers**, making collisions **extremely unlikely**.  
  - The number of possible private keys is **2^256**, which is larger than the number of atoms in the universe.  

## Conclusion
- Digital signatures ensure the authenticity and integrity of transactions.  
- They play a **critical role in Bitcoin** by allowing users to sign transactions securely.  
- Understanding **hashing and digital signatures** is fundamental to Bitcoin's security model.  

---

#  Bitcoin Addresses

## 1. Bitcoin Addresses: Purpose and Functionality
- **Purpose:**  
  - Bitcoin addresses are used to receive payments.  
  - They act as **unique identifiers** representing the destination of a transaction.  
- **Privacy:**  
  - Instead of directly using public keys, Bitcoin uses addresses to **obfuscate the recipient's identity**.  
  - This makes it more difficult to track transactions and link them to individuals.  
- **Legacy Format:**  
  - Early Bitcoin addresses start with **"1"** and are derived directly from public keys.  

## 2. Improving Privacy in Cookie Tokens
- **Problem:**  
  - In the cookie token system, real names are used in Lisa's transaction ledger, **compromising privacy**.  
- **Solution:**  
  - Instead of names, use **public keys** to identify users.  
  - This prevents casual observers from linking transactions to specific individuals.  
- **Payment Process Using Public Keys:**  
  1. The sender (e.g., a company) requests Lisa to transfer tokens to the recipient’s **public key**.  
  2. The sender signs the request with their **private key**.  
  3. Lisa **verifies the signature** using the sender's public key, checks their balance, and executes the transaction.  

## 3. Public Key Hashes: Shortening Public Keys
- **Problem:**  
  - Public keys are **long and cumbersome** to use directly in transactions.  
- **Solution:**  
  - Use a **hashed version of the public key** (called a Public Key Hash) to create shorter, easier-to-use addresses.  
- **Hashing Process:**  
  - The public key is first hashed using **SHA-256**.  
  - The result is hashed again using **RIPEMD-160**, producing a **20-byte public key hash**.  
- **Why Two Hash Functions?**  
  - Provides **additional security**: If one hash function is compromised, the other still offers protection.  
  - This practice will be phased out in future upgrades (e.g., **Taproot**).  

## 4. Addresses: Checksummed Public Key Hashes
- **Problem:**  
  - If users make typos when entering public key hashes, they could **lose funds permanently**.  
- **Solution:**  
  - Instead of using raw public key hashes, Bitcoin uses **addresses** with built-in error detection.  
- **Base58Check Encoding:**  
  1. Add a **version byte** (e.g., `0x00` for the first address type) to the public key hash.  
  2. Hash the result **twice with SHA-256** and take the **first four bytes** as a **checksum**.  
  3. Append the checksum to the versioned hash.  
  4. Encode the final result using **Base58 encoding** to create the address.  
- **Base58 Encoding:**  
  - Similar to Base64 but avoids **lookalike characters** (`0`, `O`, `I`, `l`) to **reduce errors**.  
- **Error Prevention:**  
  - The checksum **detects typos**, preventing users from accidentally sending Bitcoin to invalid addresses.  

## 5. Key Concepts Introduced
- **Public Key Hash:** A **shortened** representation of a public key created by **double-hashing** it.  
- **RIPEMD-160:** A cryptographic hash function used alongside **SHA-256** to produce **compact** public key hashes.  
- **Base58Check Encoding:** A process that encodes public key hashes into Bitcoin addresses with a **checksum** for error detection.  
- **Unspendable Output:** A transaction output that **cannot be spent** due to an invalid or missing public key hash.  

## 6. Summary
- Bitcoin **addresses** increase privacy and **reduce the complexity** of handling raw public keys.  
- A **public key** is **hashed twice** (SHA-256 + RIPEMD-160) to create a **shorter identifier**.  
- The **Base58Check encoding** process ensures that addresses include a **checksum**, preventing errors.  
- These improvements make Bitcoin transactions **more private, secure, and user-friendly**.  

---

# Wallets

## 1. Bitcoin Wallets: Functionality and Purpose
- **Multiple Wallets:**  
  - A user can have multiple wallets, which can enhance security.  
- **Key Management:**  
  - Wallets generate and store **private keys**, essential for signing transactions.  
- **Backup Importance:**  
  - It is **crucial to back up private keys** to prevent loss of funds.  
- **Address Rotation:**  
  - It is recommended to use a **new address for each transaction** for privacy.  
- **Backup Challenge:**  
  - Frequent backups are **difficult** if a new private key is generated for each transaction.  

## 2. Hierarchical Deterministic (HD) Wallets
- **Purpose:**  
  - HD wallets solve the backup issue by deriving all private keys from a **single seed**.  
- **Seed Generation:**  
  - A randomly generated seed is the foundation of the wallet.  
- **Master Key Derivation:**  
  - The seed is hashed using **HMAC-SHA512**, producing a **512-bit output**:  
    - **Left 256 bits → Private Key**  
    - **Right 256 bits → Chain Code**  
- **Child Key Derivation:**  
  - A **child extended private key** is derived using:  
    - **Parent Chain Code + Parent Public Key + Child Index**  
    - Hashed using **HMAC-SHA512**, producing another **512-bit output**:  
      - **Left 256 bits → New Private Key Component**  
      - **Right 256 bits → New Chain Code**  
    - **Child Private Key = Parent Private Key + Left 256 bits**  

## 3. Chain Code: What Is It?  
- A **deterministic secret value** used to derive **child keys**.  
- Prevents child keys from being **brute-forced** without access to the parent key.  

## 4. Backing Up the Seed  
- **Mnemonic Phrases:**  
  - A **human-readable** way to back up the randomly generated seed.  
  - The random number is **converted to a mnemonic phrase** for easier storage.  
- **Mnemonic Generation Process:**  
  1. **Checksum Calculation:**  
     - The random number is **hashed**, and the **first few bits** of the hash are appended to the binary representation.  
  2. **Bit Splitting:**  
     - The resulting binary is split into **11-bit groups**.  
  3. **Word List Mapping:**  
     - Each **11-bit group** is converted into a **word** from a predefined list of **2,048 words**.  
- **Seed Derivation:**  
  - The mnemonic phrase is passed through **PBKDF2 (Password-Based Key Derivation Function 2)** to generate the final **seed**.  

## 5. Extended Keys (xprv and xpub)
- **Extended Private Key (xprv):**  
  - Contains the **private key + chain code**.  
- **Extended Public Key (xpub):**  
  - Contains the **public key + chain code**.  
  - Can derive **child public keys** but **not private keys**.  
- **Notation:**  
  - **M:** Master Public Key  
  - **m:** Master Private Key  
  - **M/n:** Child Public Key  
  - **m/n:** Child Private Key  

## 6. Deriving Child Extended Public Keys
- **Public Key Derivation Process:**  
  1. **Hash (Chain Code + Parent Public Key + Child Index) → 512-bit output**  
  2. **Left 256 bits → Used as a private key to generate a new public key**  
  3. **New Public Key = Parent Public Key + Derived Public Key**  
- **Note:** The **chain code remains the same** for both private and public key derivation.  

## 7. Security Issue: Public-Private Key Compromise
- **Problem:**  
  - If an attacker gains **M/1 (Public Key) + m/1/1 (Private Key)**, they can derive **m/1 (Parent Private Key)**.  
- **How?**  
  - Using **M/1 + Chain Code + Child Index**, they can hash and derive:  
    - **Left 256 bits + m/1 = m/1/1**  
    - Rearranging: **m/1 = m/1/1 - Left 256 bits**  
- **Worst Case Scenario:**  
  - If an attacker steals **xpub (Master Extended Public Key) + m/1/1**, they can derive the **Master Private Key**, compromising all accounts.  

## 8. Hardened Derivation: Preventing Key Compromise
- **Solution:**  
  - Instead of using **public keys** for child key derivation, use **private keys**.  
- **Process:**  
  - **Hash (Parent Private Key + Chain Code + Child Index) → Left 256 bits + Parent Private Key = Child Private Key**  
- **Why Is It Secure?**  
  - Since private keys are required for hashing, an attacker **cannot compute the hash** without access to the private key.  

## 9. Key Concepts Introduced
- **HD Wallet:**  
  - A wallet that generates a **tree of keys** from a single **seed**, making backups simpler.  
- **xprv (Extended Private Key):**  
  - A private key that can derive **child private keys**.  
- **xpub (Extended Public Key):**  
  - A public key that can derive **child public keys**, useful for **secure address generation**.  
- **Mnemonic Phrase:**  
  - A **human-readable backup** of the wallet seed.  
- **Hardened Derivation:**  
  - A method that prevents an attacker from deriving private keys from public keys.  
- **Key Stretching:**  
  - A security technique (e.g., PBKDF2) that makes **brute-force attacks harder**.  

## 10. Summary
- Wallets **store private keys**, **generate addresses**, and **send transactions**.  
- HD wallets use a **single seed** to derive a **hierarchical structure of keys**, simplifying backups.  
- **Mnemonic phrases** provide an easy way to **securely back up the wallet**.  
- **xpub can be safely shared** to generate receiving addresses but should never be used for private key operations.  
- **Hardened derivation** ensures that even if some private keys are compromised, attackers cannot derive parent keys.  


---

# Transactions

## 1. Understanding Bitcoin Transactions  
- **Purpose:**  
  - Transactions replace the need for a central authority (e.g., Lisa managing a spreadsheet).  
  - Each transaction is **independently verifiable**, preventing fraud or unauthorized modifications.  

## 2. Transaction Structure  
- **Inputs:**  
  - Reference outputs of **previous transactions** to indicate the source of funds.  
  - Components of an input:  
    1. **Transaction ID (TxID):**  
       - The **double SHA-256 hash** of the previous transaction.  
    2. **Output Index:**  
       - Specifies **which output** from the referenced transaction is being spent.  
    3. **Sender’s Signature:**  
       - Proves ownership of the UTXO (Unspent Transaction Output).  
    4. **Sender’s Public Key:**  
       - Used to verify the signature.  

- **Outputs:**  
  - Define the new allocation of Bitcoin.  
  - Components of an output:  
    1. **Amount:**  
       - The number of bitcoins being sent.  
    2. **Recipient’s Public Key Hash:**  
       - The hash of the recipient’s **public address**, ensuring that only they can spend the output.  

## 3. Unspent Transaction Output (UTXO) Set  
- **What is UTXO?**  
  - A **record of all spendable transaction outputs**, ensuring accurate balance tracking.  
- **How does it work?**  
  - Every transaction **destroys previous outputs (spends them) and creates new outputs**.  
  - The UTXO set is **updated** by:  
    - **Removing** spent outputs.  
    - **Adding** new unspent outputs.  
- **Why use UTXOs instead of accounts?**  
  - Prevents **double-spending** and simplifies transaction verification.  

## 4. Verifying Inputs: Script Execution  
- **Each input contains a script:**  
  - **PubKey Script (Locking Script):**  
    - Stored in the **previous transaction’s output**.  
    - Specifies the conditions needed to spend the output.  
  - **Signature Script (Unlocking Script):**  
    - Stored in the **current transaction’s input**.  
    - Provides the required **public key and signature** to satisfy the pubkey script.  
- **Execution Flow:**  
  - The **signature script** is placed on a stack and executed with the **pubkey script** to validate the transaction.  

## 5. Advanced Scripts: Multisignature Transactions  
- **Multisig Transactions:**  
  - Require **multiple signatures** to authorize spending.  
- **Pay-to-Script-Hash (P2SH):**  
  - Uses a **hashed script** instead of a direct public key hash.  
  - Allows more complex spending conditions, e.g., **2-of-3 multisig wallets**.  
- **How P2SH Works:**  
  - The output contains **only the script hash**, keeping it compact.  
  - The input must include the **full redeem script and valid signatures** to unlock the funds.  

## 6. Soft Fork and Forward Compatibility  
- **Soft Fork:**  
  - A **backward-compatible** rule change.  
  - Old nodes can still validate new transactions, ensuring network continuity.  
- **Example:**  
  - **P2SH was introduced via a soft fork**, enabling advanced scripts without breaking older Bitcoin nodes.  

## 7. Summary  
- Transactions **destroy previous outputs and create new ones**, ensuring no double-spending.  
- UTXOs **track who owns how much** instead of account balances.  
- Scripts define **spending conditions**, enabling features like multisig wallets.  
- **P2SH allows more flexibility**, making complex conditions easier to implement.  
- **Soft forks maintain backward compatibility**, allowing gradual protocol upgrades.  

---

# Proof-of-Work  

## 1. Why Proof-of-Work (PoW)?  
### The Problem: Centralized Censorship  
- If Lisa controls block creation, she can **censor transactions** she dislikes.  

### The Solution: Decentralized Block Creation  
- Instead of a single entity, **multiple miners** (e.g., Tom, She) participate.  
- Users broadcast transactions to **all miners**.  
- Miners **compete** to create the next block.  

## 2. The Challenge: Preventing Chaos  
- Without rules, miners would generate **blocks too frequently**, leading to **instability**.  

### Initial Attempt: Lottery System  
- Each miner picks a **random number**, and the winner creates the block.  
- **Problem:** Miners can **cheat** the system by manipulating random numbers.  

## 3. The Real Solution: Proof-of-Work (PoW)  
- Instead of a **lottery**, miners must **perform computational work** to create a valid block.  

### How Proof-of-Work Works  
- Blocks contain a **target difficulty** (a predefined number).  
- Miners try different **nonces** (random values) until they find a **valid hash**.  
- A valid hash is one where:  
  ```plaintext
  Hash(Block Header) ≤ Target```

- This process requires significant **computational effort**, ensuring fairness.

## 4. Verifying Proof-of-Work

- **Verifying is easy:**
    1. Hash the **block header**.
    2. If the result is **less than the target**, the block is valid.

## 5. Block Creation Process

1. **Create a block template** with pending transactions.
2. **Set the target** in the block header.
3. **Start with a nonce = 0**.
4. **Hash the block header**.
5. If the hash meets the target → **Block is valid**.
6. If not, increment the nonce and **repeat**.

## 6. Benefits of Proof-of-Work

✅ **Decentralization:** Anyone can participate in mining.  
✅ **Anonymity:** Hard to trace miners.  
✅ **Security:** Manipulating the blockchain is computationally expensive.

## 7. Forks and Chain Selection

- Sometimes, two miners find **valid blocks simultaneously**, creating a **fork**.
- Bitcoin resolves forks using the **strongest chain rule**:
    - The chain with the **most accumulated PoW** is considered valid.
    - Miners **switch to the strongest chain**.

### Lottery vs. Proof-of-Work Comparison

|Feature|Lottery System|Proof-of-Work System|
|---|---|---|
|Target|555 out of 1 million|9260b9 × 2²⁰⁰ out of 2²⁵⁶|
|Frequency|Every second|Every 0.02 microseconds|
|Best Chain|Longest chain|Strongest chain (most PoW)|

## 8. Hash Rate & Difficulty Adjustment

### What is Hash Rate?

- The **hash rate** is the number of hashes a miner can compute per second.

### Problem: Blocks Arrive Too Fast

- If the total hash rate increases, blocks are mined **faster than every 10 minutes**.

### Solution: Difficulty Adjustment

- Bitcoin **adjusts the mining difficulty** every **2016 blocks (~2 weeks)** to maintain **10-minute block times**.
- If blocks are mined **too quickly**, difficulty **increases** (harder to mine).
- If blocks are mined **too slowly**, difficulty **decreases** (easier to mine).

## 9. Timestamp Rules

- Blocks contain **timestamps** to prevent manipulation.
- Rules:
    1. Must be **≤ 2 hours in the future**.
    2. Must be **higher than the median of the last 11 blocks**.

## 10. Double Spending Attacks

- A user **spends the same coins twice** by creating a **competing chain**.
- PoW makes this difficult but **not impossible**.

### Double-Spending Attack Conditions

- The attacker needs to **mine an alternative chain** that **becomes longer** (stronger) than the honest chain.
- Requires significant **hash power**.
- **More confirmations = lower attack success probability**.

### Probability of Success

- The chance of an attacker **overtaking z confirmations** is calculated using probability formulas.

### Practicality

- **High cost and risk** make double spending **unprofitable** in most cases.

## 11. Transaction Fees (Incentive for Miners)

### Problem: Empty Blocks

- Miners might prefer **empty blocks** for faster propagation.

### Solution: Transaction Fees

- Users **attach fees** to transactions.
- Miners **prioritize transactions** with the **highest fees**.

### How Transaction Fees Work

- Formula:
    
    ```plaintext
    Fee = Sum of Inputs - Sum of Outputs
    ```
    
- Fees are added to the **coinbase transaction** as a miner reward.

## 12. Transaction Selection & Block Size Limit

- **Mempool:** A waiting area for unconfirmed transactions.
- **Fee per Byte:** Miners prioritize transactions based on **fee per byte** (not absolute fee).
- **Block Size Limit:** Bitcoin blocks are limited to **1MB**.
- **Profit Maximization:** Miners select transactions that **maximize total fees** within the block size limit.

## 13. Mining Profitability

- Miners must balance **electricity costs vs. rewards** (subsidy + fees).
- If mining costs **exceed** rewards, miners **stop mining**.

## 14. Block Subsidy & Halving

- **Block Subsidy:** The reward for mining a block.
- **Halving:** Every **4 years (~210,000 blocks)**, the subsidy is **halved**.
- **Transition to Fees:** Over time, transaction fees will replace subsidies as the primary miner reward.

---

## 15. Summary

✅ **Proof-of-Work secures Bitcoin** by making block creation computationally expensive.  
✅ **Miners compete to find valid blocks**, ensuring decentralization.  
✅ **Forks are resolved by selecting the chain with the most accumulated PoW**.  
✅ **Difficulty adjustment maintains a steady block time of ~10 minutes**.  
✅ **Transaction fees incentivize miners and will become the primary reward as block subsidies decrease**.  
✅ **Double spending is possible but impractical due to high costs**.


---

# Peer-to-Peer Network  

## 1. The Problem: Centralized Censorship (Again)  
- Even with **Proof-of-Work (PoW)**, a centralized **shared folder admin** could still **censor blocks**.  

## 2. The Solution: Peer-to-Peer (P2P) Network  
- A **network of interconnected full nodes** replaces the shared folder.  
- Nodes **propagate blocks and transactions** to their peers.  
- **Censorship resistance:** To censor, **many nodes** must collude, making it highly improbable.  

## 3. Data Propagation  
### Block Propagation  
1. **Miner mines a block**.  
2. Miner sends a **HEADERS message** (block header) to peers.  
3. Peers verify the **PoW in the header**.  
4. Peers request full block data using **GETDATA**.  
5. Miner responds with **BLOCK message** (full block data).  
6. Block propagates across the network.  

### Transaction Propagation  
1. **Wallet sends a transaction** to a node.  
2. Node **broadcasts an INV message** (inventory) to peers.  
3. Peers request full transaction data via **GETDATA**.  
4. Node responds with **TX message** (full transaction data).  
5. Transaction propagates across the network.  

## 4. Communication Between Peers  
- **Protocol:** Nodes communicate using **Transmission Control Protocol (TCP)**.  
- **Messaging system:** Nodes exchange messages using a **structured protocol**.  

### Example: INV Message  
- A node sends an **INV message** to inform peers about **new blocks or transactions**.  
- Peers respond with **GETDATA** to request the full data.  

## 5. Lightweight Wallet Integration  
- **Lightweight wallets** connect to **full nodes** instead of downloading the entire blockchain.  
- Full nodes provide:  
  - **Relevant transactions**.  
  - **Merkle proofs** (to verify inclusion in a block).  

## 6. Setting Up a New Full Node  
### Steps:  
6. **Software Download & Verification**  
   - Download **Bitcoin Core**.  
   - Verify integrity using the **digital signature** from Bitcoin Core developers.  
7. **Peer Discovery**  
   - Find initial peers via **hardcoded addresses** or **DNS seeds**.  
8. **Blockchain Synchronization**  
   - Download the blockchain from **connected peers**.  
9. **Normal Operation**  
   - Once fully synchronized, the node acts as a **full participant** in the network.  

## 7. Secure Software Verification  
### Steps:  
10. **Download Bitcoin Core binary** and **SHA256SUMS.ASC signature file**.  
11. **Obtain Bitcoin Core's signing public key**.  
12. **Verify signature** using the public key.  
13. **Compute SHA256 hash** of the downloaded binary.  
14. **Compare** computed hash with the **one in the signature file**.  

## 8. Peer Discovery Methods  
- **Hardcoded Addresses:** Predefined list of IPs in Bitcoin Core.  
- **DNS Seeds:** Query DNS servers to discover Bitcoin nodes.  
- **Manual Input:** Manually add trusted peers.  

## 9. Connecting to Peers  
15. **Establish TCP connection** to a peer’s IP and port.  
16. **Exchange version messages** to negotiate protocol version.  
17. **Verack Message:** Acknowledge version agreement.  
18. **Getaddr:** Request addresses of additional peers.  

## 10. Blockchain Synchronization  
19. **Getheaders:** Request block headers from a peer.  
20. **Headers:** Peer responds with batch of block headers.  
21. **Verify PoW** in headers.  
22. **Repeat steps 1-3** until all headers are downloaded.  
23. **Getdata:** Request full block data for headers.  
24. **Block:** Peer sends full block data.  
25. **Repeat steps 5-6** until fully synced.  

---

## 11. Summary  
✅ **Bitcoin’s P2P network prevents censorship** by distributing data across many nodes.  
✅ **Transactions and blocks propagate through a structured message system**.  
✅ **Nodes use TCP and a messaging protocol** to communicate.  
✅ **New nodes join the network via peer discovery & blockchain synchronization**.  
✅ **Secure software verification prevents malware-infected downloads**.  
✅ **Decentralized peer discovery & data propagation ensure Bitcoin remains trustless and robust**.  
