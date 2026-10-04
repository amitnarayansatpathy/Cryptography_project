# Cryptography_project
3_1 Cryptography course project

# Decentralised Land Records Storage System

> A blockchain-powered web application for secure, tamper-proof land transaction management — built using Python and Flask.

---

##  Team — Group 6 (BITS Pilani, Hyderabad Campus | CSF 407 Cryptography)

---

##  Project Overview

Traditional land record systems rely on centralised authorities, making them vulnerable to fraud, tampering, and single points of failure. This project leverages **blockchain technology** to build a decentralised, peer-to-peer land records management system where transactions are cryptographically secured, immutable, and transparent.

The application enables users to:
- Submit land transactions (buyer, seller, amount, land size, location)
- Authenticate each transaction via **HMAC-based Challenge-Response Authentication**
- Mine pending transactions into blocks using **Proof of Work**
- View full transaction histories for any user

---

##  Objectives

- Gain hands-on experience with core blockchain development concepts
- Apply real-world cryptographic mechanisms (SHA-256, HMAC) to a practical problem
- Eliminate the need for centralised intermediaries in land record management
- Ensure transaction **integrity**, **authenticity**, and **non-repudiation**

---

##  System Architecture

```
┌─────────────────────────────────────────┐
│             Flask Web Interface          │
│   /addtransaction  /mine  /view          │
└────────────────────┬────────────────────┘
                     │
         ┌───────────▼───────────┐
         │      BlockChain       │
         │  - chain[]            │
         │  - unconfirmed_txns[] │
         └───────────┬───────────┘
                     │
     ┌───────────────┼───────────────┐
     │               │               │
  ┌──▼──┐      ┌─────▼─────┐   ┌────▼────┐
  │Block│      │Transaction│   │Verifier │
  │     │      │+ HMAC sig │   │+ HMAC   │
  └─────┘      └───────────┘   │  check  │
                                └─────────┘
```

---

##  Security Design

### SHA-256 Block Hashing
Each block stores a **SHA-256 hash** of its contents (transactions, nonce, proof, timestamp, previous hash). Any tampering with historical data invalidates all subsequent block hashes, making the chain immutable.

### Proof of Work (PoW)
Blocks are mined with a difficulty of **2 leading zeros** — the miner increments a nonce until the block hash satisfies this condition, securing the chain against rapid block injection.

### HMAC-Based Transaction Verification
Every transaction is signed with an **HMAC-SHA256** digest at creation time (using a shared secret key). Before a transaction enters the unconfirmed pool, the `Verifier` class recomputes the HMAC independently and compares it to the stored value — rejecting any tampered transaction.

### Challenge-Response Authentication Protocol
On top of HMAC verification, a **Challenge-Response** protocol is executed for every `add_transaction` call:
1. A random 16-byte **challenge** is generated
2. A random **bit** (0 or 1) is generated
3. An **HMAC response** is computed over `challenge + bit`
4. The response is verified using `hmac.compare_digest()` — preventing timing attacks

Only if both HMAC verification **and** the challenge-response check pass is the transaction accepted.

---

##  Project Structure

```
decentralised-land-records/
│
├── server.py               # Core application: Blockchain, Block, Transaction, Verifier, Flask routes
├── templates/
│   ├── index.html          # Add transaction UI
│   ├── mine.html           # Mine block UI
│   └── view.html           # View user transactions UI
├── requirements.txt        # Python dependencies
└── README.md
```

---

##  Core Components

### `Block`
| Attribute | Description |
|-----------|-------------|
| `prev_hash` | Hash of the preceding block |
| `transaction_list` | List of `Transaction` objects in the block |
| `nonce` | Incremented during PoW mining |
| `proof` | Proof-of-work value |
| `timestamp` | Block creation time |
| `block_hash` | SHA-256 hash of the block |

**Key Methods:** `get_hash()`, `mine_block(difficulty)`, `to_dict()`

---

### `Transaction`
| Attribute | Description |
|-----------|-------------|
| `buyer` | Name of the buyer |
| `seller` | Name of the seller |
| `amount` | Transaction amount |
| `land_size` | Size of the land |
| `land_location` | Location of the land |
| `time` | Timestamp of the transaction |
| `hmac` | HMAC-SHA256 signature of the transaction |

**Key Methods:** `verifytransaction()`, `to_dict()`, `get_hmac(secret_key)`

---

### `Verifier`
Independently recomputes the HMAC of a transaction and compares it to the stored value to confirm integrity before the transaction is accepted.

---

### `BlockChain`
The central ledger. Manages the chain, unconfirmed transactions, mining, and transaction lookup.

| Method | Description |
|--------|-------------|
| `genesis_block()` | Creates the initial block |
| `add_transaction(tx)` | Verifies + authenticates, then queues transaction |
| `mine_pending_transactions(addr)` | Mines all queued transactions into a new block + awards miner |
| `user_transactions(user)` | Retrieves all transactions for a user |
| `proof_of_work()` | Computes valid PoW for the next block |

---

##  Flask Web Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Home page — transaction submission form |
| `/addtransaction` | POST | Submits a new land transaction |
| `/mine` | GET | Mining page |
| `/mineblock` | POST | Mines all pending transactions |
| `/view` | GET | Transaction viewer page |
| `/viewchain` | POST | Fetches transactions for a user |
| `/viewuser` | GET | Fetches transactions via query param `?name=` |

---

##  Libraries Used

| Library | Purpose |
|---------|---------|
| `hashlib` | SHA-256 hashing for blocks and transactions |
| `hmac` | HMAC-SHA256 for transaction authentication |
| `json` | Serialization of blocks and transactions |
| `time` | Timestamps for blocks and transactions |
| `random` | Nonce generation, PoW, challenge-response bits |
| `flask` | Web framework for the user interface |
| `threading` | Background continuous mining thread (optional) |

---

##  Getting Started

### Prerequisites
- Python 3.8+
- pip

### Installation

```bash
# 1. Clone the repository
git clone <repository_url>
cd decentralised-land-records

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the Flask application
python server.py
```

The app will be available at `http://127.0.0.1:5000/`

---

##  Usage

1. **Add a Transaction** — Navigate to `/` and fill in buyer, seller, price, land size, and location. The transaction is HMAC-signed and challenge-response authenticated before being queued.
2. **Mine a Block** — Navigate to `/mine`, enter a miner reward address, and submit. All pending transactions are mined into a new block using Proof of Work.
3. **View Transactions** — Navigate to `/view` and enter a user's name to retrieve all their confirmed (and pending) land transactions.

---

##  Future Work

- **Smart Contracts** — Automate land transfer conditions and dispute resolution
- **Decentralised Governance** — Multi-node consensus for a true peer-to-peer network
- **Scalability** — Optimise PoW difficulty and transaction throughput for larger networks
- **Enhanced UI** — More intuitive dashboards, real-time block explorer
- **Persistent Storage** — Replace in-memory chain with a database backend

---


