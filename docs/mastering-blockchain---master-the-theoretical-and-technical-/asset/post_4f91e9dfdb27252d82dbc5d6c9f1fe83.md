
+   4. `PaymentChannelFund`: This transaction is used to add more funds to an existing channel. Similar to `PaymentChannelClaim` transaction, this can also be used to modify the expiration time of the payment channel.

## Order related

This type of transaction includes following two fields:

-   1. `OfferCreate`: This transaction represents a limit order, which represents an intent for exchange of currency. It results in creating an Offer node in the consensus ledger if it cannot be completely fulfilled.
-   2. `OfferCancel`: This is used to remove a previously created offer node from the consensus ledger, indicating withdrawal of the order.

## Account and security related

This type of transaction include the fields listed as follows. Each field is responsible for performing a certain function.

-   1. `AccountSet`: This transaction is used to modify the attributes of an account in the Ripple consensus ledger.
-   2. `SetRegularKey`: This is used to change or set the transaction signing key for an account. An account is identified using a base-58 Ripple address derived from the account's master public key.
-   3. `SignerListSet`: This can be used to create a set of signers for use in multi-signature transactions.
-   4. `TrustSet`: This is used to create or modify a trust line between accounts.

A transaction in Ripple is composed of various fields that are common to all transaction types. These fields are listed as follows with description:

-   1. `Account`, address of the initiator of the transaction.
-   2. `AccountTxnID`, this is an optional field which contains the hash of another transaction.
-   3. `Fee`, Amount of XRP.
-   4. `Flags`, optional flags for the transaction.
-   5. `LastLedgerSequence`, highest sequence number of the ledger in which the transaction can appear.
-   6. `Memos`, optional arbitrary information.

## Interledger layer

This layer provides interoperability and routing services. This layer contains protocols such as **Interledger protocol (ILP)**, **Interledger quoting protocol (ILQP)**, and **Interledger control protocol (ILCP)**. ILP packet provides the final target of the transaction in a transfer. ILQP is used in making quote requests by the senders before the actual transfer. ILCP is used to exchange data related to routing information and payment errors between connectors on the payment network.

## Ledger layer

This layer contains protocols that enable communication and execution of payment transactions between connectors. *Connectors* are basically objects that implement the protocol for forwarding payments between different ledgers. It can support various protocols such as Simple Ledger protocol, various blockchain protocols, legacy protocols, and different proprietary protocols.

Ripple connect consists of various Plug and Play modules that allow connectivity between ledgers by using the ILP. It enables the exchange of required data between parties before the transaction, visibility, fee management, delivery confirmation, and secure communication using Transport layer security. A third-party application can connect to the Ripple network via various connectors.

Overall, Ripple is a solution that is targeted for financial industry and makes real-time payments possible without any settlement risk. As this is a very feature-rich platform, covering all aspects of it are not possible in this chapter. Ripple and very rich documentation for the platform are available at https://ripple.com/.

## Stellar

Stellar is a payment network based on blockchain technology and a novel consensus model called **Federated Byzantine Agreement (FBA)**. FBA works by creating quorums of trusted parties. **Stellar Consensus Protocol (SCP)** is an implementation of FBA.

Key issues identified in the Stellar whitepaper are the cost and complexity of current financial infrastructure. This limitation warrants the need for a global financial network that addresses these issues without compromising the integrity and security of the financial transaction. This requirement has resulted in the invention of **Stellar Consensus Protocol (SCP)** which is a provably safe consensus mechanism.

It has four main properties: *decentralized control*, which allows participation by anyone without any central party; *low latency*, which addresses the much desired requirement of fast transaction processing; *flexible trust*, which allows users to choose which parties they trust for a specific purpose, and finally, *asymptotic security*, which makes use of digital signatures and hash functions for providing the required level of security on the network.

The Stellar network allows transfer and representation of the value of an asset by its native digital currency, called Lumens, abbreviated as XLM. Lumens are consumed when a transaction is broadcasted on the network, which also serves as a deterrent against **Denial of Service (DOS)** attacks.

At its core, the Stellar network maintains a distributed ledger that records every transaction and is replicated on each Stellar server. The consensus is achieved by verifying transactions between servers and updating the ledger with updates. The Stellar ledger can also act as a distributed exchange order book by allowing users to store their offers to buy or sell currencies.

There are various tools, SDKs, and software that make up the Stellar network. The core software is available at https://github.com/stellar/stellar-core.

## Rootstock

Before discussing Rootstock in detail, it's important to define and introduce some concepts that are fundamental to the design of Rootstock. These concepts include sidechains, drivechains, and two-way pegging. The concept of the sidechain was originally developed by Blockstream.

Two way pegging is a mechanism by which value (coins) can transfer between one blockchain to another and vice versa. There is no real transfer of coin between chains. The idea revolves around the concept of locking the same amount and value of coins in a bitcoin blockchain (main chain) and unlocking the equivalent amount of tokens in the secondary chain.

Bearing this definition in mind, sidechains can be defined as described in the following section.

### Sidechain

This is a blockchain that runs in parallel with a main blockchain and allows transfer of value between them. This means that tokens from one blockchain can be used in the sidechain and vice versa. This is also called a pegged sidechain because it supports two-way pegged assets.

## Drivechain

This is a relatively new concept, where control on unlocking the locked bitcoins (in mainchain) is given to the miners who can vote when to unlock them. This is in contrast to sidechains, where consensus is validated though Simple payment verification mechanism in order to transfer the coins back to the mainchain.

Rootstock is a smart contract platform which has a two-way peg into bitcoin blockchain. The core idea is to increase the scalability and performance of the bitcoin system and enable it to work with smart contracts. Rootstock runs a Turing complete deterministic virtual machine called Rootstock Virtual Machine (RVM). It is also compatible with the Ethereum virtual machine and allows solidity-compiled contracts to run on Rootstock. Smart contracts can also run under the time-a tested security of bitcoin blockchain. The Rootstock blockchain works by merge mining with bitcoins. This allows RSK blockchain to achieve the same security level as bitcoin. This is especially true for preventing double spends and achieving settlement finality. It allows scalability, up to 100 transactions per second.

RSK has recently released a test network called Turmeric. It is available at http://www.rsk.co/.

## Quorum

This is a blockchain solution built by enhancing the existing Ethereum blockchain. There are several enhancements such as transaction privacy and a new consensus mechanism that has been introduced in Quorum. Quorum has introduced a new consensus model known as QuorumChain, which is based on a majority voting and time based mechanism. Another feature called Constellation is also introduced which is a general purpose mechanism for submitting information and allows encrypted communication between peers. Furthermore, permissioning at node level is governed by smart contracts. It also provides a higher level of performance compared to public Ethereum blockchains.

Several components make up the Quorum blockchain ecosystem. These are listed in the following section.

## Transaction manager

This component enables access to encrypted transaction data. It also manages local storage and communication with other Transaction managers on the network.
