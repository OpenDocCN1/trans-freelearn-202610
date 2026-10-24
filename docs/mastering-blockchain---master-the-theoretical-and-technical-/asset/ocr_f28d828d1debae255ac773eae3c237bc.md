Initial block download can use blocks first or the headers-first method to synchronize blocks depending on the version of the bitcoin core client. The blocks-first method is very slow and was discontinued since version 0.10.0.

Since version 0.10.0, the initial block download method named headers-first was introduced. This resulted in major performance improvement and the blockchain synchronization that used to take days to complete started taking only a few hours. The core idea is that the new node-first asks peers for block headers and validates them. Once this is completed, blocks are requested in parallel from all available peers as the blueprint of the complete chain is already downloaded in the form of the block header chain.

In this method, when the client starts up, it checks whether the block chain is fully synchronized already if the header chain is already synchronized; if not, which is the case the first time the client starts up, it requests headers from other peers using the getHeaders message. If the block chain is fully synchronized, it listens for new blocks via Inv messages, and if it already has a fully synchronized header chain, then it requests blocks using Getdata protocol messages. The node also checks whether the header chain has more headers than blocks and then it requests blocks by issuing the Getdata protocol message.

![](bbox=[0.27, 0.5, 0.68, 0.88])

Bitcoin Core Client >= 0.10.0 header and block synchronization. IBD = Initial block download and sync node means the node from where the blocks are being requested from