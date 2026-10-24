# ## First node setup

First, `geth` needs to be started on the first node using the following command:

```
```bash
$ geth --datadir .ethereum/privatenet/ --networkid 786 --maxpeers 5 --rpc --rpcapi web3,eth,debug,personal,net --rpcport 9001 --rpccorsdomain "*" --port 30301 --identity "drequinox"
```
```

![](bbox=[0.104, 0.241, 0.895, 0.513])

Once `geth` is started up it should be kept running and another `geth` instance should be started from the Raspberry Pi node.

# ## Raspberry Pi node setup

On Raspberry Pi, the following command is required to be run in order to start `geth` and sync it with other nodes (in this case only one node). The following is the command:

```
```bash
$ ./geth --networkid 786 --maxpeers 5 --rpc --rpcapi web3,eth,debug,personal,net --rpccorsdomain "*" --port 30302 --identity "raspberry"
```
```

This should produce the output similar to the one shown in the following screenshot. When the output contains the row displaying **Block synchronization started** it means that the node has connected successfully to its peer.