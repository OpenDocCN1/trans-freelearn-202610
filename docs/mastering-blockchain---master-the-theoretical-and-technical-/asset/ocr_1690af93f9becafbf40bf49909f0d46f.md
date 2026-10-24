# Starting up a node in regtest

The regtest mode (regression testing mode) can be used to create a local blockchain for testing purposes.

The following commands can be used to start up a node in the reg test mode:

```
bitcoind -regtest -daemon
Bitcoin server starting
```

Blocks can be generated using the following command:

```
bitcoin-cli -regtest generate 200
```

Relevant log messages can be viewed in the .bitcoin/regtest directory on a Linux system under debug.log.

![](bbox=[0.11, 0.49, 0.9, 0.64])

After block generation, the balance can be viewed as follows:

```
drequinox@drequinox-OP7010:~/.bitcoin/regtest$ bitcoin-cli -regtest
getbalance
8750.00000000
```

The node can be stopped using this:

```
drequinox@drequinox-OP7010:~/.bitcoin$ bitcoin-cli -regtest stop
Bitcoin server stopping
```

# Starting up a node in live mainnet

Bitcoind is the core client software that can be run as a daemon, and it provides the JSON RPC interface.