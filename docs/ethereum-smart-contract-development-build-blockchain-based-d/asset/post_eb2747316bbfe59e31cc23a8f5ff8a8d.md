
# Understanding a blockchain—a developer and trader's perspective

From a software developer's perspective, a blockchain is just a giant Merkle tree. Take a moment now, pause your reading. Let this information sink in. I repeat with another layer of clarity.

A blockchain is a giant Merkle tree, where each new block embeds the hash of the previous block as well as the root hash of the present block-action, eventually forming a chain of hash-blocks. It might be not very obvious, but such a Merkle tree structure does not require a central server; each block can come from physically separate clients. Hence, a blockchain is also a distributed system. I can feel you are starting to feel confused right now. Just look at Figure 1.5 to mitigate some of your confusion. We will discuss timestamp and nonce later, when we explore inside a block.

![](img/8c91673d39176d2e910ac52c4bc35d8b_42_0.png)

Figure 1.5: Blockchain as a giant Merkle tree

You might argue that blockchain looks more like a linked list with a Merkle tree hanging on each block, but it would be a fundamentally wrong perception because here we cannot backpropagate to the original data value. A more appropriate statement would be that it is very difficult to backpropagate to get back the actual values **x0, x1, x2,** and **x3**, using the root hash of the block. Yet the advantage lies in the fact that these hashes are immutable. You cannot revert a blockchain, in principle, once it is formed.

Before discussing the trader's perspective of a blockchain, it would be a good time to realize what a bitcoin blockchain is and what an Ethereum blockchain is and how they are different in principle and practice. For beginners who are reading the preceding lines, I would like to state that, in blockchain community, it is an agreed standard to represent a cryptocurrency with small cap while representing the technology, protocol, and ecosystem of that currency with large cap. So, bitcoin or Ethereum represents the virtual cryptocurrency, while Bitcoin or Ethereum represents the tech stack, protocol, and ecosystem surrounding the respective virtual cryptocurrency.

Let's look at the over-simplified diagram of blockchains in *Figure 1.6*. All blockchains have some kind of root value **R**. So, when I send 2 bitcoins (BTC) to a friend, I actually do a transaction. Let's call it **T1**.

![](img/8c91673d39176d2e910ac52c4bc35d8b_43_0.png)

Figure 1.6: Bitcoin and Ethereum blockchains

This transaction, **T1**, a transfer of value, will be hashed together with the previous root **R** and create a new root, **R1**. So, this is almost like a Merkle tree (in fact, it is a Merkle tree) where the data blocks, that is, the leaves of the Merkle tree, are storing bits of data representing a transaction, which in turn get hashed into a new root hash. Then, another transaction T2 comes in and it gets hashed with root R1 to form yet another new root R2. This is how a bitcoin blockchain works, keeping a permanent store of all transactions which have taken place since the start of the original hash root (also called the genesis block). Now, if we try to cheat the system by stating that my transaction T1 was of 100 BTC (in reality, we sent only 2 BTC), the fake transaction will be checked by blockchain community, by hashing it with R that will generate an entirely new root hash value R'1, completely different from R1. This rise in contradiction will falsify our 100 BTC claim. This is what makes bitcoin as a cryptocurrency so elegant in preventing the double-spend problem.

An interesting aspect that now arises is how an Ethereum blockchain works. We saw that a bitcoin blockchain is a decentralized application for transferring value and verifying transactions. An Ethereum blockchain, on the other hand, can even execute codes. As software developers, most of you might be familiar with states of a code in runtime.

In simple terms, it is the configuration of a piece of code at any given instant of time. An Ethereum blockchain acts as a decentralized application platform, which stores each state of a code while in execution (that is, during runtime) and creates a hash chain out of it. Again, take a moment to let this concept sink in. So, when we execute a code on an Ethereum blockchain, each and every state (S1, S2) of the executed code will merge with the roots R' and R'1 respectively and will be publicly visible. Any type of code glitches will be captured and stored on the public blockchain and will remain there for eternity. In the future, we could see all the previous states of all the codes that ever got executed on the Ethereum blockchain. An obvious question arises about how such blockchains can ever scale, with the exponential growth of runtime logs when many such codes will run across the world over the Ethereum platform. Think about it.

Meanwhile, we move on to understand a blockchain from a trader's perspective. A trader of financial instruments or a banker views currency as something which serves the three key functions:

- Medium of exchange
- Store of value
- Unit of account

Cryptocurrency for a trader is no different from physical currencies, even though it has only a virtual presence and has no central authority of control. Blockchain, from a trader's perspective, is viewed as a distributed ledger, where the distributed consensus serves as the apparent central authority for such virtual currencies.

A Blockchain being immutable serves as a permanent record of any transfer of value that has ever taken place in the past, while cryptographically preserving the privacy of the identity of clients at both ends of any transaction.

It needs to be emphasized that both of these perspectives of a blockchain are not mutually exclusive. These apparently different perspectives are not like comparing apples with oranges. They both exist in unison and synergize to give rise to a decentralized economy, which we will explore in later chapters. Now, you can refer to Figure 1.7, to understand how a blockchain can be positioned within the financial-technological ecosystem:

![](img/8c91673d39176d2e910ac52c4bc35d8b_46_0.png)

Figure 1.7: Blockchain in fin-tech ecosystem

## Inside a block

Let's now discuss the contents of a block of bitcoin blockchain. This will help us in understanding and comparing Ethereum, which we will take up in chapter 2, Grokking Ethereum. A block is a very interesting concept, because a block gives blockchain its properties. It is the basic element of a blockchain.

Therefore, it is important to know what a block consists of. Figure 1.8 lists the basic parts of a bitcoin block. First of all, we have a magic number, also called bitcoin network ID.

| Field | Description | Size |
|---|---|---|
| Magic no | value always 0xD9B4BEF9 | 4 bytes |
| Block size | number of bytes following up to end of block | 4 bytes |
| Block header | consists of 6 items | 80 bytes |
| Transaction counter | positive integer VI = Varint | 1 - 9 bytes |
| Transactions | the (non empty) list of transactions | <Transaction counter>-many transactions |

Figure 1.8: Parts of a bitcoin block (Bitcoin wiki)

It is four bytes long and is an arbitrary number that signals that this is a bitcoin block. The magic number is not something specific to bitcoin. All nodes communicate using transmission control protocol.

In TCP, different types of data packets use different magic numbers to identify themselves. It is like declaring our gender while filling out a passport to identify ourselves as male, female, or transgender. In our context, a block is actually a sequence of 0 and 1. When any machine reads such a data sequence and it encounters the binary version of 0xD9B4BEF9, it will identify the data as a bitcoin block. So, this number is the same for all bitcoin blocks. Now, why was 0xD9B4BEF9 chosen for bitcoin blocks?

As per the comments found in the bitcoin main.cpp files (bitcoin implementations were written in C++ language), the magic number was chosen in such a way that the characters are rarely used in upper ASCII, not valid as UTF-8, and produce a large four-byte integer at any alignment.

Next, we have the block size which is also four bytes and tells us how long this block is with all of its transactions. As of July 2017, the size of an entire bitcoin block can be a maximum of 1 MB. We then have the block header, which is 80 bytes and it is the most interesting part; we will talk about its contents in a short while. The next one is the transaction counter, which is an integer that tells us how many transactions this block has and next we have a list of transactions, which simply contains all the transactions that are in this block.

For example, if we have the transaction counter as 20, we have 20 transactions in the block. The transaction counter is one to nine bytes. So, these were the basic parts of the block.

So, let's go ahead and look up what the bitcoin block header consists of. Here, in Figure 1.9, we have the header parts. The header has a version that simply tells us which version of the format we are on currently.

For example, the current bitcoin version is 2.

| Field | Purpose | Updated when... | Size (Bytes) |
|---|---|---|---|
| Version | Block version number | You upgrade the software and it specifies a new version | 4 |
| hashPrevBlock | 256-bit hash of the previous block header | A new block comes in | 32 |
| hashMerkleRoot | 256-bit hash based on all of the transactions in the block | A transaction is accepted | 32 |
| Time | Current timestamp as seconds since 1970-01-01T00:00 UTC | Every few seconds | 4 |
| Bits | Current target in compact format | The difficulty is adjusted | 4 |
| Nonce | 32-bit number (starts at 0) | A hash is tried (increments) | 4 |

Figure 1.9 Bitcoin block header (Bitcoin wiki)

Now, if for some reason a bitcoin block from the version 1 client comes, it will be ignored by blockchain. In the future, there might be a change in the size and format of a bitcoin block. Then, we would have to change this version in each block to keep them relevant in blockchain. The version
