
## Crypto Enclave

As the name suggests, this component is responsible for providing cryptographic services to ensure transaction privacy. It is also responsible for performing key management functions.

## QuorumChain

This is the key innovation in Quorum. It is a Byzantine Fault-tolerant consensus mechanism which allows verification and circulation of votes via transactions on the blockchain network. In this scheme, a smart contract is used to manage the consensus process and nodes can be given voting rights to vote on which new block should be accepted. Once an appropriate number of votes is received by the voters, the block is considered valid. Nodes can have two roles, namely Voter or Maker. The Voter node is allowed to vote, whereas the Maker node is the one that creates a new block. A node can have either rights, none or only one.

## Network manager

This component provides an access control layer for the permissioned network.

A node in the quorum network can take several roles, for example, a Maker node that is allowed to create new blocks. Transaction privacy is provided using cryptography and the concept that certain transactions are meant to be viewable only by their relevant participants. This idea is similar to Corda's idea of private transactions that was discussed in the last chapter. As it allows both public and private transactions on the blockchain, the state database has been divided into two databases representing private and public transactions. As such, there are two separate Patricia-Merkle trees that represent the private and public state of the network. A private contract state hash is used to provide consensus evidence in private transactions between transacting parties.

Transaction in a quorum network consists of various elements such as the recipient, the digital signature of the sender, which is used to identify the transaction originator, optional ether amount, the optional list of participants that are allowed to see the transaction, and a field that contains a hash in case of private transactions.

A transaction goes through several steps before it can reach its destination. These steps are described as follows in detail:

- 1. User applications (DAPPs) send the transaction to the quorum node via an API exposed by the blockchain network. This also contains the recipient address and transaction data.
- 2. The API then encrypts the payload and applies any other necessary cryptographic algorithm in order to ensure privacy of the transaction, and is sent to the Transaction manager. The hash of the encrypted payload is also calculated at this step.
- 3. After receiving the transaction, the Transaction manager validates the signature of the transaction sender and stores the message.
- 4. The hash of the previously encrypted payload is sent to the Quorum node.
- 5. Once the Quorum node starts to validate a block that contains the private transaction, it requests more relevant data from the Transaction manager.
- 6. Once this request is received by the Transaction manager, it sends the encrypted payload and relevant symmetric keys to the requestor quorum node.
- 7. Once the Quorum node has all the data, it decrypts the payload and sends it to the EVM for execution. This is how Quorum achieves privacy with symmetric encryption on the blockchain, while it is able to use native Ethereum protocol and EVM for message transfer and execution respectively.
- 8. A similar concept, but quite different in a few aspects, has been proposed before in the form of Hydrachain, which is based on Ethereum blockchain and allows creation of permissioned distributed ledgers.

Quorum is available for download at https://github.com/jpmorganchase/quorum.

## Tezos

Tezos is a generic self-amending cryptographic ledger, which means that it not only allows decentralized consensus on the state of the blockchain but also allows consensus on how the protocol and nodes are evolved over time. Tezos has been developed to address limitations in bitcoin protocol such as issues arising from hard forks, cost, and mining power centralization due to Proof of Work, limited scripting ability, and security issues. It has been developed in a purely functional language called OCaml.

The architecture of Tezos distributed ledger is divided into three layers: the network layer, consensus layer, and transaction layer. This decomposition allows protocol to be evolved in a decentralized fashion. For this purpose, a generic network shell is implemented in Tezos that is responsible for maintaining the blockchain, which is represented by a combination of consensus and transaction layer. This shell provides an interface layer between the network and the protocol. A concept of seed protocol has also been introduced, which is used as a mechanism to allow stakeholders on the network to approve any changes to the protocol. Tezos blockchain starts from a seed protocol compared to a traditional blockchain that starts from a genesis block.

This seed protocol is responsible for defining procedures for amendments in the blockchain and even the amendment protocol itself. The reward mechanism in Tezos is based on a **Proof of Stake (PoS)** algorithm, therefore there is no mining requirement.

Contract script language has been developed in Tezos for writing smart contracts, which is a stack-based Turing complete language. Smart contracts in Tezos are formally verifiable, which allows the code to be mathematically proven for its correctness.

Tezos code is available at https://github.com/tezos/tezos.

## Storj

Existing models for cloud-based storage are all centralized solutions, which may or may not be as secure as users expect them to be. There is a need to have a cloud storage system that is secure, highly available, and above all decentralized. Storj aims to provide blockchain based, decentralized, and distributed storage. It is a cloud shared by the community instead of a central organization. It allows execution of storage contracts between nodes that act as autonomous agents. These agents (nodes) execute various functions such as data transfer, validation, and perform data integrity checks. The core concept is based on **Distributed Hash Tables (DHT)** -Kademlia, however this protocol has been enhanced by adding new message types and functionalities in Storj. It also implements a peer to peer **publish/subscribe (pub/sub)** mechanism known as Quasar, which ensures that messages successfully reach the nodes that are interested in storage contracts. This is achieved via a bloom filter-based storage contract parameters selection mechanism called **topics.**

Storj stores files in an encrypted format spread across the network. Before the file is stored on the network, it is encrypted using AES-256-CTR symmetric encryption and is then stored piece by piece in a distributed manner on the network. This process of dissecting the file into pieces is called **sharding** and results in increased availability, security, performance, and privacy of the network. Also if a node fails the shard is still available because by default a single shard is stored at three different locations on the network.

It maintains a blockchain, which serves as a shared ledger and implements standard security features such as public/private key cryptography and hash functions similar to any other blockchain. As the system is based on hard drive sharing between peers, anyone can contribute by sharing their extra space on the drive and get paid with Storj's own cryptocurrency called **Storjcoinx (SJCX)**. SJCX was developed as a *counterparty* asset and makes use of bitcoin blockchain for transactions.

Storj code is available at https://github.com/Storj/.

## Maidsafe

This is another distributed storage system similar to Storj. Users are paid in Safecoin for their storage space contribution to the network. This mechanism of payment is governed by *proof of resource*, which ensures that the disk space committed by a user to the network is available, if not then the payment of Safecoin will drop accordingly. The files are encrypted and divided into small portions before being transmitted on to the network for storage. Another concept of **opportunistic caching** has been introduced with Maidsafe, which is a mechanism to create copies of frequently accessed data physically closer to where the access requests are coming from, which results in high performance of the network. Another novel feature of the SAFE network is that it automatically removes any duplicate data on the network, thus resulting in reduced storage requirements. Moreover, the concept of **churning** has also been introduced, which basically means that data is constantly moved across the network so that the data cannot be targeted by malicious adversaries. It also keeps multiple copies of data across the network to provide redundancy in case a node goes offline or fails.

## BigChainDB

This is a scalable blockchain database. It is not strictly a blockchain itself, but complements blockchain technology by providing a decentralized database. At its core it's a distributed database but with the added attributes of a blockchain such as decentralization, immutability, and handling of digital assets. It also allows usage of NoSQL for querying the database. It is intended to provide a database in a decentralized ecosystem where not only processing is decentralized (blockchain) or the file system is decentralized (for example, IPFS) but the database is also decentralized. This makes the whole decentralized application ecosystem decentralized. This is available at https://www.bigchaindb.com/.

## Multichain

Multichain has been developed as a platform for the development and deployment of private blockchains. It is based on bitcoin code and addresses security, scalability, and privacy issues. It is a highly configurable blockchain platform that allows users to set different blockchain parameters. It supports control and privacy via a granular Permissioning layer. Installation of Multichain is very quick and links to installation files are available at http://www.multichain.com/download-install/.
