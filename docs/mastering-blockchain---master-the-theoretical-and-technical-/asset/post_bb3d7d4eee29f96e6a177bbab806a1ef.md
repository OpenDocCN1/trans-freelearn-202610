
The list of all symbols with their meanings used in the paper is provided here with the anticipation that it will make reading the paper easier. Once symbol meanings are known, it becomes quite easy to understand and appreciate the concepts and specifications described in the yellow paper.

## Useful symbols

| Symbol | Meaning | Symbol | Meaning |
|---|---|---|---|
| ≡ | Is defined as | ≤ | Less than or equal to |
| = | Is equal to | σ | Sigma, World state |
| ≠ | Is not equal to | μ | Mu, Machine state |
| || ... || | Length of | Υ | Upsilon, Ethereum state transition function |
| ∈ | Is an element of | Π | Block level state transition function |
| ∉ | Is not an element of | . | Sequence concatenation |
| ∀ | For all | ∃ | There exists |
| ∪ | Union | ^ | Contract creation function |
| ∧ | Logical AND | Δ | Increment |
| : | Such that |  |  |
| {} | Set |  |  |
| () | Function of tuple |  |  |
| [] | Array indexing |  |  |
| ∨ | Logical OR |  |  |
| > | Is greater than |  |  |
| + | Addition |  |  |
| - | Subtraction |  |  |
| Σ | Summation |  |  |
| { | Describing various cases of if , otherwise |  |  |
| [...] | Floor, lowest element |  |  |

## Supporting protocols

There are various supporting protocols that are in development in order to support the complete decentralized ecosystem. This includes whisper and Swarm protocols. In addition to the contracts layer, which is the core blockchain layer, there are additional layers that need to be decentralized in order to achieve a complete decentralized ecosystem. This includes decentralized storage and decentralized messaging. Whisper, being developed for ethereum, is a decentralized messaging protocol, whereas Swarm is a decentralized storage protocol. Both of these technologies are being developed currently and have been envisaged to provide the basis for a fully decentralized web. In the following section, both technologies are discussed in detail.

### Whisper

Whisper provides decentralized peer-to-peer messaging capabilities to the ethereum network. In essence, whisper is a communication protocol that nodes use in order to communicate with each other. The data and routing of messages are encrypted within whisper communications. Moreover, it is designed to be used for smaller data transfers and in scenarios where real-time communication is not required. Whisper is also designed to provide a communication layer that cannot be traced and provides “dark communication” between parties. Blockchain can be used for communication, but that is expensive and consensus is not really required for messages exchanged between nodes. Therefore, whisper can be used as a protocol that allows

Whisper is already available with geth and can be enabled using the --shh option while running the geth ethereum client.

### Swarm

Swarm is being developed as a distributed file storage platform. It is a decentralized, distributed, and peer-to-peer storage network. Files in this network are addressed by the hash of their content. This is in contrast to the traditional centralized services, where storage is available at a central location only. This is developed as a native base layer service for the Ethereum web 3.0 stack. Swarm is integrated with DevP2P, which is the multiprotocol network layer of Ethereum. Swarm is envisaged to provide a DDOS (Distributed Denial of service)-resistant and fault-tolerant distributed storage layer for Ethereum Web 3.0. Both whisper and Swarm are under development and, even though Proof of Concept and alpha code has been released for Swarm, there is no stable production version available yet.

The following figure gives a high level overview of how Swarm and whisper fit together and work with blockchain:

![](img/62ab880d2302f959d34db648cb0bec5e_294_0.png)

Diagrams shows blockchain, whisper and Swarm

## Applications developed on Ethereum

There are various implementations of DAOs and smart contracts in Ethereum, most notably, the DAO, which was recently hacked and required a hard fork in order for funds to be recovered. The DAO was created to serve as a decentralized platform to collect and distribute investments.

Augur is another DAPP that has been implemented on Ethereum, which is a decentralized prediction market. Various other decentralized applications are listed on `http://dapps.etherecasts.com/`.

## Scalability and security issues

Scalability in any blockchain is a fundamental issue. Security is also of paramount importance. Issues such as privacy and confidentiality have caused some adaptability issues, especially in the financial sector. However, a great deal of research is being conducted in these areas. A more detailed discussion regarding all blockchain-related issues will be carried out in Chapter 12, *Scalability and other challenges*.

## Summary

This chapter started with a discussion on the history of Ethereum, the motivation behind Ethereum development, and Ethereum clients. Then, you were introduced to the core concepts of the Ethereum blockchain, such as state machine model, world and machine state, accounts, and types of accounts. Moreover, a detailed introduction to the core components of the **Ethereum virtual machine (EVM)** was also presented. Other concepts such as blocks, block structure, gas, and messages were also introduced and discussed in detail. The later sections of the chapter introduced the practical installation and management of ethereum clients. Two most popular clients, geth and parity, were discussed. Further development-specific discussion on these clients will be carried out in the next chapter, where development using Ethereum is discussed. Finally, supporting protocols and topics related to challenges faced by Ethereum were presented. Ethereum is under continuous development and new improvements are being made by a dedicated community of developers regularly. Ethereum improvement proposals, available at `https://github.com/ethereum/EIPs`, are also an indication of the magnitude of research and keen interest by the community in this technology. Moreover, a recently launched initiative, **Enterprise Ethereum Alliance (EAA)** is aiming to develop enterprise grade Ethereum platform which will be capable of meeting enterprise level business requirements. With research being carried out on topics such as scalability, optimization, throughput, capacity, and security, it is envisaged that over time, Ethereum will evolve into a more robust, user-friendly, and stable blockchain ecosystem.

## 8

## Ethereum Development

This chapter introduces the concepts, techniques, and tools related to Ethereum development. Several examples will be introduced in this chapter to complement the theoretical concepts provided in earlier chapters. This chapter will mainly cover the setup of the development environment and how to create smart contracts using Ethereum blockchain. Detailed walkthrough examples will be provided that will help you understand how Ethereum and other supporting tools can be used to develop and deploy smart contracts on the blockchain.

## Setting up a development environment

The first task is to set up a development environment. The upcoming section introduces the ethereum setup for Test Net and Private Net. Test Net is called Ropsten and is used by developers or users as a test platform to test smart contracts and other blockchain-related proposals. The Private Net option in Ethereum allows the creation of an independent private network that can be used as a distributed ledger between participating entities and for the development and testing of smart contracts. While there are other clients available for Ethereum, such as Parity, which was discussed in the previous chapter, geth is the leading client for Ethereum and the common tool of choice, as such this chapter will use `geth` in the examples.

### Test Net (Ropsten)

The Ethereum Go client, geth, can be connected to the test network using the following command:

```
geth --TestNet
```

A sample output is shown in the following screenshot. The screenshot shows the type of the network chosen and various other pieces of information regarding the blockchain download.

A blockchain explorer for test net is located at `https://testnet.etherscan.io/` and can be used to trace transactions and blocks on the Ethereum test network.

![](img/62ab880d2302f959d34db648cb0bec5e_297_0.png)

### Setting up a Private Net

Private Net allows the creation of an entirely new blockchain. This is different from Test Net or Main Net in the sense that it uses its own genesis block and Network ID. In order to create Private Net, three components are needed:

- 1. Network ID.
- 2. Genesis file.
- 3. Data directory to store blockchain data. Even though data directory is not strictly required to be mentioned, if there is more than one blockchain already active on the system, then data directory should be specified so that a separate directory is used for the new blockchain.
