
Zcash uses an efficient PoW scheme named **Asymmetric PoW (Equihash)**, which is based on the Generalized Birthday Problem. It allows very efficient verification. It is a memory-hard and ASIC-resistant function. A novel idea (initial slow mining) has been introduced with Zcash, which means that the block reward increases gradually over a period of time until it reaches the 20,000th block. This allows for initial scaling of the network and experimentation by early miners, and adjustment by Zcash developers if required. The slow start did have an impact on price due to scarcity as the price of ZEC on its first day of launch reached roughly 25,000 USD. A slightly modified version of the Digishield difficulty adjustment algorithm has been implemented in Zcash. The formula is shown as follows:

```
(next difficulty) = (last difficulty) x SQRT [ (150 seconds) / (last solve time) ]
```

A snapshot of various attributes of Zcash (after an initial slow start) is shown as follows:

| Attribute | Value |
|---|---|
| Name | Zcash |
| Launch date | 28/10/16 |
| Main purpose | Currency |
| Currency Code | ZEC |
| Maximum coins | 21 million |
| Block time | 10 minutes |
| Consensus facilitation algorithm | Proof of Work (equihash) |
| Difficulty adjustment algorithm | DigiShield V3 (modified) |
| Mining hardware | CPU, GPU |
| Difficulty adjustment period | 1 block |

2. Next, run the commands to clone Zcash from git as shown in the following screenshot:

```
$ git clone https://github.com/zcash/zcash.git
```

This command will clone the Zcash git repository locally. The output is shown in the following screenshot:

![](img/62ab880d2302f959d34db648cb0bec5e_217_0.png)

3. The next step is to download proving and verifying keys, by using the following command from the screenshot:

![](img/62ab880d2302f959d34db648cb0bec5e_218_0.png)

4. Once this command runs it will download around 911 MBs of keys into the ~/.zcash-params/ directory. The directory contains files for proving and verifying keys:

```
drequinox@drequinox-OP7010:~/.zcash-params$ pwd
/home/drequinox/.zcash-params
drequinox@drequinox-OP7010:~/.zcash-params$ ls -ltr
-rw-rw-r-- 1 drequinox drequinox      1449 Oct 24 16:46 sprout-verifying.key
-rw-rw-r-- 1 drequinox drequinox 910173851 Oct 24 16:46 sprout-proving.key
```

5. Once the preceding commands are completed successfully, the source code can be built using the following command:

```
./zcash/build.sh -j$(nproc)
```

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

## Address generation

New Z addresses can be generated using the command below:

```
$:~/zcash/src$ ./zcash-cli z_getnewaddress
zcPDBKuuwHJ4gqT5Q59zAMXDHhFoi hyTC1aLE5Kz4GwgUXfCBWG6SDr45SFLUsZhpc dvHt7nFmC3iQcn37rKBcVRa93DYrA
```

Running the Zcash-cli command with the `getinfo` parameter produces the output shown in the following screenshot. It displays valuable information such as blocks, difficulty, and balance:

![](img/62ab880d2302f959d34db648cb0bec5e_220_0.png)

New T addresses can be generated using the following command:

```
drequinox@drequinox-OP7010:~/zcash/src$ ./zcash-cli getnewaddress
t1XRCGMAw36yPVCcxDUIrxvZcsAAuGdsS8Nny
```

## GPU mining

Other than CPU mining, a GPU mining option is also available. There is no official GPU miner yet; however open source developers have produced various proofs of concepts and working miners. The Zcash Company held an open competition to encourage developers to build and submit CPU and GPU miners. No winning entry has been announced as of the time of writing. Readers can get more information by visiting the website, `https://zcashminers.org/`.

There is another mining: using cloud mining contracts available from various online cloud mining providers. The cloud mining service providers perform mining on the customers' behalf. In addition to cloud mining contracts, miners can use their own equipment to mine via mining pools using stratum or other protocols. One key example is Zcash pool by nice hash. Using this pool, miners can sell their hash power. An example of building and using a CPU miner on a Zcash mining pool is shown as follows.

## Downloading and compiling nheqminer

The following steps can be used to download and compile `nheqminer` on an Ubuntu Linux distribution:

```
sudo apt-get install cmake build-essential libboost-all-dev
git clone https://github.com/nicehash/nheqminer.git
cd nheqminer/nheqminer
mkdir build
cd build
cmake ..
make
```

Once all the steps are completed successfully, nheqminer can be run using the following command:

```
./nheqminer -l eu -u <btc address> -t <number of threads>
```

Nheqminer releases are available for Windows at:

```
https://github.com/nicehash/nheqminer/releases.
```

Nheqminer takes several parameters such as location (`-l`), username (`-u`), and the number of threads to be used for mining (`-t`).

A sample run of Windows miner for Zcash is shown as follows:

![](img/62ab880d2302f959d34db648cb0bec5e_221_0.png)

Using the BTC address to receive pay-outs for selling hash power

![](img/62ab880d2302f959d34db648cb0bec5e_222_0.png)

Using Zcash T address to receive pay-outs for selling hash power

This completes the introduction to Zcash; readers can explore more about Zcash online as it is very volatile at the moment and things can change very quickly. One thing is for sure; Zcash's zero knowledge proofs are a major innovation and they pave the way for future applications that require inherent privacy, such as banking, medicine, or the law.

## Summary

In this chapter, readers have been introduced to the overall cryptocurrency landscape. A few altcoins have been discussed in detail, especially Zcash and Namecoin. Cryptocurrencies are a very active area for research, especially around scalability, privacy, and security aspects. Some research has also been conducted in order to invent new difficulty retargeting algorithms in order to thwart the threat of centralization in cryptocurrencies. Further research can be carried out in the areas of privacy and especially scalability. Readers now should be able to appreciate the concept of altcoins and various motivations behind them. Some practical aspects, such as mining and starting a new currency project, have also been discussed, which hopefully will give readers a strong foundation and enable them to further explore these areas. Altcoins are a fascinating field of research and open a lot of possibilities for a decentralized future.
