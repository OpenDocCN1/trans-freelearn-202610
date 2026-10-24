# ## Party

This implementation is built using Rust and developed by EthCore. EthCore is a company that works on the development of the parity client. Parity can be downloaded from `https://ethcore.io/parity.html`.

# ## Light clients

SPV clients download only a small subset of the blockchain. This allows low resource devices, such as mobile phones, embedded devices, or tablets, to be able to verify the transactions. A complete ethereum blockchain and node are not required in this case and SPV clients can still validate the execution of transactions. SPV clients are also called light clients. This idea is similar to bitcoin SPV clients. There is a wallet available from Jaxx (`https://jaxx.io/`), which can be installed on iOS and Android, which provides the **SPV (Simple Payment Verification)** functionality.

# ## Installation

The following installation procedure describes the installation of various Ethereum clients on Ubuntu systems. Instructions for other operating systems are available on Ethereum Wikis. As Ubuntu systems will be used in examples later on, only installation on Ubuntu has been described here.

**Geth client** can be installed by using the following command on an Ubuntu system:

```
```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install -y ethereum
```
```

After installation is completed. Geth can be launched simply by issuing the `geth` command at the command prompt, as it comes preconfigured with all the required parameters to connect to the live Ethereum network (mainnet):

```
```bash
geth
```
```