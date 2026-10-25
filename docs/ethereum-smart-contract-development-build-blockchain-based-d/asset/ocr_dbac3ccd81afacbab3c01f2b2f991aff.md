# Understanding a blockchain—a developer and trader's perspective

From a software developer's perspective, a blockchain is just a giant Merkle tree. Take a moment now, pause your reading. Let this information sink in. I repeat with another layer of clarity.

A blockchain is a giant Merkle tree, where each new block embeds the hash of the previous block as well as the root hash of the present block-action, eventually forming a chain of hash-blocks. It might be not very obvious, but such a Merkle tree structure does not require a central server; each block can come from physically separate clients. Hence, a blockchain is also a distributed system. I can feel you are starting to feel confused right now. Just look at Figure 1.5 to mitigate some of your confusion. We will discuss timestamp and nonce later, when we explore inside a block.

![](bbox=[0.14, 0.59, 0.86, 0.81])

Figure 1.5: Blockchain as a giant Merkle tree

You might argue that blockchain looks more like a linked list with a Merkle tree hanging on each block, but it would be a fundamentally wrong