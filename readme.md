Decentralised Land Records Storage System


Group 6
Akash Bhale – 2021A3PS0854H
Anamay Trivedi- 2021AAPS2217H
Sriharsha Tippavajhala- 2021AAPS0717H
Pranay G- 2021A3PS2725H
Amit Narayan Satpathy – 2021AAPS2127H




Project Description
The goal of this project is to implement a blockchain application that utilizes blockchain fundamentals for secure transaction management. The application includes functionalities such as adding transactions, mining blocks, and viewing transactions for users. Additionally, it incorporates HMAC-based Challenge-Response Authentication for secure user authentication without revealing the secret key.

Problem Statement
Blockchain technology offers new tools for authentication and authorization in the digital world, eliminating the need for many centralized administrators. In this project, we aim to leverage blockchain to create a secure and decentralized transaction management system.

Blockchain Fundamentals
A blockchain is a digital and distributed ledger of transactions, recorded and replicated in real-time across a network of computers or nodes. Every transaction must be cryptographically validated via a consensus mechanism executed by the nodes before being permanently added as a new "block" at the end of the "chain." Blockchain eliminates the need for a central authority to approve transactions, making it a peer-to-peer trustless mechanism.

Quick Introduction to HMAC
Challenge-Response Authentication with HMAC (Hash-based Message Authentication Code) is implemented in scenarios where a user needs to prove their knowledge of a secret key without revealing the key itself. The HMAC mechanism involves generating a random challenge, sending it to the verifier, receiving a random bit, and creating a response using HMAC with the challenge and the received bit.

Implementation Details
createBlock(): This method is responsible for creating a new block in the blockchain. It includes attributes such as the previous hash, transaction list, proof of work, nonce, and timestamp. The block is mined using a proof-of-work algorithm to ensure its validity.
verifyTransaction(): This method verifies transactions before they are added to a block. As part of the verification process, HMAC is used to verify at least one attribute of the transaction, ensuring its integrity and authenticity.
mineBlock(): This method is equivalent to mining a block in the blockchain. It involves solving a proof-of-work challenge to add pending transactions to the blockchain securely.
viewUser(): This method lists all successful transactions against a specific user. It provides users with visibility into their transaction history within the blockchain.



Since it is a land records storage system, we have a buyer and seller and attributes for a transaction like 
amount,
land_size,
land_location,
time,
hmac.


Methods and Attributes Description:

Libraries Used:
hashlib: This library provides various hash functions, including SHA-256, which is used for hashing in the blockchain implementation.
time: The time module is used to handle timestamp generation for blocks and transactions.
json: The json module is used for JSON serialization and deserialization.
hmac: The hmac module is used for generating HMAC (Hash-based Message Authentication Code) for secure authentication.
random: The random module is used for generating random numbers, which are utilized in various parts of the application.
Flask: Flask is a micro web framework for Python used to build web applications. It is utilized for creating the user interface and handling HTTP requests in the blockchain application.
render_template: This function is used to render HTML templates in Flask applications.
request: The request object is used to access incoming request data in Flask routes.
Thread: The Thread class from the threading module is used to create a separate thread for continuous mining of blocks.

Attributes:
HMAC_KEY: A secret key used for HMAC authentication.
CHALLENGE_LENGTH: Length of the challenge in bytes for HMAC authentication.

Block Class:

Attributes:
prev_hash: Hash of the previous block in the blockchain.
transaction_list: List of transactions included in the block.
nonce: A number that is incremented during mining to find the proof of work.
proof: Proof of work for the block.
timestamp: Timestamp indicating when the block was created.
block_hash: Hash of the current block.

Methods:
get_hash(): Generates the hash of the block using SHA-256.
mine_block(difficulty): Mines the block by finding a nonce that satisfies the proof-of-work difficulty.
to_dict(): Converts the block object to a dictionary for serialization.
__str__(): Returns a string representation of the block.

Transaction Class:

Attributes:
buyer: Name of the buyer.
seller: Name of the seller.
amount: Amount of the transaction.
land_size: Size of the land involved in the transaction.
land_location: Location of the land.
time: Timestamp of the transaction.
hmac: HMAC generated for transaction authentication.

Methods:
to_string(): Returns a string representation of the transaction.
get_hmac(secret_key): Generates HMAC for the transaction using a secret key.
to_dict(): Converts the transaction object to a dictionary for serialization.
todict(): Returns transaction details as a dictionary.
verifytransaction(): Verifies the transaction using HMAC authentication.

Verifier Class:

Methods:
verify(): Verifies the transaction using HMAC authentication by comparing the expected HMAC with the computed HMAC.

Blockchain Class:

Attributes:
chain: List of blocks in the blockchain.
unconfirmed_transactions: List of transactions awaiting confirmation.
users: List of users in the blockchain network.

Methods:
genesis_block(): Generates the genesis block (initial block) of the blockchain.
create_block(proof): Creates a new block in the blockchain with the provided proof of work.
mine_pending_transactions(mining_reward_address): Mines pending transactions and adds them to the blockchain.
add_transaction(transaction): Adds a transaction to the blockchain after verification.
proof_of_work(): Finds the proof of work for mining a block.
print_blocks(): Prints all blocks in the blockchain.
view_users(): Prints all users in the blockchain network.
user_transactions(user): Retrieves all transactions associated with a specific user.
make_transaction(buyer, seller, amount, land_size, land_location): Creates and adds a transaction to the blockchain.
is_transaction_in_blockchain(transaction): Checks if a transaction is already present in the blockchain.

Helper Functions:
mine_indef(blockchain): Runs an infinite loop to continuously mine pending transactions in a separate thread.
generate_challenge(): Generates a random challenge for HMAC authentication.
create_response(challenge, bit): Creates a response using HMAC with the challenge and a random bit.
verify_response(challenge, bit, response): Verifies the response using HMAC authentication.

Steps to Run the Code:

Clone the repository:
git clone <repository_url>

Navigate to the project directory:
cd <project_directory>

Install dependencies:
pip install -r requirements.txt

Run the Flask application:
python <filename>.py

