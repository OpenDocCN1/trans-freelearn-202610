This will create a directory named geth-linux-arm7-1.5.6-2a609af5 and will extract geth binary and related files into that directory. geth binary can be copied into /usr/bin or the appropriate path on Raspbian to make it available from anywhere in the operating system. When the download is finished, the next step is to create the genesis block.

The same genesis block needs to be used that was created previously in Chapter 8, Ethereum Development. The genesis file can be copied from the other node on the network. This is shown in the following screenshot. Alternatively, an entirely new genesis block can be generated. This was discussed in detail in Chapter 8, Ethereum Development.

![](bbox=[0.1, 0.3, 0.9, 0.48])

Genesis file

Once the genesis.json file is copied onto the Raspberry Pi, the following command can be run in order to generate the genesis block. It is important that exactly the same genesis block is used that was generated previously otherwise the nodes will effectively be running on separate networks:

```
$ ./geth init genesis.json
```

This will show the output similar to the one shown in the following screenshot:

![](bbox=[0.1, 0.68, 0.9, 0.77])

Initialize genesis file