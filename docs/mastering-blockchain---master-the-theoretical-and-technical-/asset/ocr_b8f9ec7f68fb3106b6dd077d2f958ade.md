This will produce very long output; if everything goes well it will produce a zcashd binary file. Note that this command takes nproc as the parameter, which is basically a command that finds the number of cores or processors in the system and displays that number. If you don't have that command then replace nproc with the number of processors in your system.

Once the build is completed, the next step is to configure Zcash. This is achieved by creating a configuration file with the name zcash.conf in the ~/.zcash/ directory.

A sample configuration file is shown as follows:

```
addnode=mainnet.z.cash
rpcuser=drequinox
rpcpassword=xxxxxxoJNo4o5c+F6E+J4P2C1D5iz1zIKPZJhTzdW5A=
gen=1
genproclimit=8
equihashsolver=tromp
```

The preceding configuration enables various features. The first line adds the mainnet node and enables mainnet connectivity. rpcuser and rpcpassword are the username and password for the RPC interface. gen = 1 is used to enable mining. genproclimit is the number of processors that can be used for mining. The last line enables a faster mining solver; this is not required if you want to use standard CPU mining.

Now Zcash can be started using the following command:

```
./zcashd --daemon
```

Once started this will allow interaction with the RPC interface via the Zcash-cli command-line interface. This is almost the same as the bitcoin command-line interface. Once the Zcash daemon is up-and-running, various commands can be run to query different attributes of Zcash. Transactions can be viewed locally by using the CLI or via a blockchain explorer. A blockchain explorer for Zcash is available at: https://explorer.zcha.in/.

# Address generation

New Z addresses can be generated using the command below:

```
$:~/zcash/src$ ./zcash-cli z_getnewaddress
zcPDBKuuwHJ4gqT5Q59zAMXDHhFoi hyTC1aLE5Kz4GwgUXfCBWG6SDr45SFLUsZhpc dvHt7nFmC3iQcn37rKBcVRa93DYrA
```