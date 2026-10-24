
Once the contract is written and simulated in browser Solidity, the next step is to use Truffle to initialize a new project and deploy and test it on the PrivateNet (ID 786), already created in earlier sections.

The first step is to create a separate directory for the project:

```
~$ mkdir ideapatent
~$ cd ideapatent/
```

The next step is to initialize Truffle and create a new project:

```
~/ideapatent$ truffle init
```

Once the sample project is created, remove the sample contracts:

```
~/ideapatent/contracts$ rm MetaCoin.sol ConvertLib.sol
```

Under the contracts folder, create a file named PatentIdea.sol and put the source code shown earlier in the file.

Edit truffle.js to point to the localhost HTTP endpoint:

```
rpc:
  {
    host: "localhost",
    port: 8001
  }
```

Under the ~/ideapatent/migrations folder, edit the 2_deploy_contracts.js file so that it looks like the following:

```
module.exports = function(deployer)
{
  deployer.deploy(PatentIdea);
  deployer.autolink();
};
```

This file is changed in order to specify the name of the contract to be deployed. Take note of `deployer.deploy(PatentIdea);`.

Next, run the compilation using Truffle, as shown here:

```
~/ideapatent$ truffle compile
Compiling Migrations.sol...
Compiling PatentIdea.sol...
Writing artifacts to ./build/contracts
```

## Oracles

As discussed in Chapter 6, Smart Contracts, Oracles are real-world data feeds into smart contracts. There are various services available in order to provide Oracles for smart contracts. A rather prominent one is Oraclize, which is available at http://www.oraclize.it/. This is especially useful if the smart contract needs, for example, live prices from a third-party source or any other real-life data, such as weather conditions in a particular city. There are many use cases where Oracles can provide a trusted data feed to smart contracts in order to enable them to make decisions according to real-life events. Oraclize makes it easier for smart contracts to access the Internet in order to get the required data.

In order to utilize Oraclize on Ethereum, a transaction needs to be sent to the Oraclize contract along with the appropriate payment and the query. As a result, Oraclize will retrieve the results based on the query provided in the request transaction and send it back to the contract address. Once the transaction is sent back to the contract, the call-back method or fallback function will be called.

At a practical level in Solidity, first, the Oraclize library needs to be imported and then all methods that have been inherited from it can be used. Currently, Oraclize is available to be used only on the PrivateNet (Ropsten) and Live Main Net Ethereum blockchain.

Oraclize processing can be visualized as shown in the following diagram:

![](img/62ab880d2302f959d34db648cb0bec5e_374_0.png)

The skeleton structure of a Solidity contract using Oraclize looks like the one shown here. Note that import works only on the development environment provided on the Web by Oraclize; usually, this file needs to be imported manually:

```
import "dev.oraclize.it/api.sol";
contract MyOracleContract is usingOraclize
{
function MyOracleContract() {
}
```

A sample request looks like what is shown in the following example:

```
oraclize_query("URL", "api.somewebsite.net/price?stock=XYZ");
```

Oraclize can also make use of the TLS notary in order to ensure that the feed is secure and provably honest.

## Deployment on decentralized storage using IPFS

As discussed in Chapter 1, Blockchain 101, in order to fully benefit from decentralized platforms, it is desirable that you decentralize the storage and communication layer. Traditionally, the web content is served via centralized servers, but that part can also be decentralized using distributed file systems.

The HTML content shown in the earlier examples can be stored on a distributed and decentralized IPFS network in order to achieve enhanced decentralization.

IPFS is available at https://ipfs.io/.

### Installing IPFS

IPFS can be installed by following this process:

1. Download the IPFS package using the following command:

   ```
   $ curl https://dist.ipfs.io/go-ipfs/v0.4.4/go-ipfs_v0.4.4_linux-amd64.tar.gz -O
   ```

2. Decompress the gz file:

   ```
   $ tar xvfz go-ipfs_v0.4.4_linux-amd64.tar.gz
   ```

3. Move the ipfs file to an appropriate folder in order to make it available in the path:

   ```
   $ mv go-ipfs/ipfs /usr/local/bin/ipfs
   ```

4. Initialize the IPFS node:

   ```
   imran@drequinox-OP7010:~$ ipfs init
   initializing ipfs node at /home/imran/.ipfs
   generating 2048-bit RSA keypair...done
   peer identity: Qmbc726pLS9nUQjUbeJUXcFXAGaXPD41jAszXniChJz62
   to get started, enter:
       ipfs cat /ipfs/QmYwAPJzv5CzsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme
   ```

5. Enter the following command to ensure that IPFS has been successfully installed:

   ![](img/62ab880d2302f959d34db648cb0bec5e_376_0.png)

   Successful IPFS installation

6. Start the IPFS daemon:

   ```
   imran@drequinox-OP7010:~$ ipfs daemon
   Initializing daemon...
   Swarm listening on /ip4/127.0.0.1/tcp/4001
   Swarm listening on /ip4/192.168.0.17/tcp/4001
   Swarm listening on /ip4/86.15.44.209/tcp/4001
   Swarm listening on /ip4/86.15.44.209/tcp/41608
   Swarm listening on /ip6/::1/tcp/4001
   API server listening on /ip4/127.0.0.1/tcp/5001
   Gateway (readonly) server listening on /ip4/127.0.0.1/tcp/8080
   Daemon is ready
   ```

7. Copy files onto IPFS using the following command:

   ```
   ~/sampleproject/build$ ipfs add --recursive --progress .
   added QmVdyD1yuqcf32e8NhMVEWSuMfyvcj17w3DkUt6BgeAtx7 build/app.css
   added QmSypiNEiUx6Sq7moAVCsgQhSY3Bh9ziwXJaxqSG5Pcp build/app.js
   added QmaJWMjD767GvuwuaIpt5tck9dTVCZPJa9sDcr8vdcJ8pY build/contracts/ConvertLib.sol.js
   added QmQdz9eG2Qd5kwaU86kWebDGPqXBWj1Dmv9MN4BRzt2srf build/contracts/MetaCoin.sol.js
   added QmWpvBjXTP4HutEsYUh3JLD18VYp73SKNJi4aX1T6jwcmG build/contracts/Migrations.sol.js
   added QmQs7j6NpA1NMueTXKyswLaHKq3XDUCRay3VrC392Q4JDK build/index.html
   added QmPvWzyTEfLQnozDTfgdAAF4W9BUB2cDq5KUUrpHrukseA build/contracts
   added QmUNLLsPACCZ1vLxQVkXqqLX5R1X345qqfHbsf67hva3Nn build/images
   added QmSxpucr6J9rX3XQ3MBG8cVzLCrQFFKmMkTmpcNpjbtf3j build
   ```

8. Now it can be accessed in the browser as follows:

   ![](img/62ab880d2302f959d34db648cb0bec5e_377_0.png)

   Browser accessing web pages via IPFS

   Note that the URL is pointing to the IPFS filesystem.

9. Finally, in order to make the changes permanent, the following command can be used:

   ```
   /build$ ipfs pin add QmSxpucr6J9rX3XQ3MBG8cVzLCrQFFKmMkTmpcNpjbtf3j
   pinned QmSxpucr6J9rX3XQ3MBG8cVzLCrQFFKmMkTmpcNpjbtf3j recursively
   ```

The preceding example demonstrated how IPFS can be used to provide decentralized storage for the web part (user interface) of smart contracts.

IPFS can be used with blockchains in another way. As storage is a big issue for blockchains, it is desirable that you are able to save large amounts of data somewhere else and place the links to that data in the blockchain transaction. This way, there will be no need to store large amounts of data on the blockchain and bloat it as a result. IPFS can be used to achieve exactly that by placing the data on IPFS and then storing the IPFS links in blockchain transactions to reference the stored data.

Ethereum's own Swarm protocol is also under heavy development and works on similar principles. However, Swarm is currently under development and IPFS is more developed comparatively and seems a better choice at the moment. IPFS works very well and may well become the platform of choice for the decentralized storage of deployments. Swarm allows users to run a light client by storing all the blockchain data on it. This is available with the current version of `geth`, and a detailed guide is available at https://swarm-guide.readthedocs.io/en/latest/introduction.html. As this is under heavy development, only a light introduction has been given for this technology as it is likely to evolve very quickly.

For decentralized communication in Ethereum, the Whisper protocol provides the decentralized communication layer. This will serve as an identity-based messaging layer for Ethereum. Both Swarm and Whisper are envisaged to be enabling technologies for Web 3.0.

## Permissioned distributed ledgers

The concept of permissioned distributed ledgers is fundamentally different to a public blockchain. The key idea behind distributed ledgers is that they are permissioned as opposed to an open public blockchain. DLTs do not perform any mining as all the participants are already vetted and known to the network and there is no requirement for mining to secure the network. There is also no concept of digital currency on private permissioned distributed ledgers because the aim of the permissioned blockchain is different from a public blockchain. In a public blockchain, access is open to everyone and requires some form of incentive and network effect in order to grow; on the contrary, in permissioned DLTs, there are no such requirements. It is possible to build permissioned DLTs using Ethereum in private consortium settings, especially to work within existing financial systems. The key benefit of distributed ledger systems is that they are much faster, governable, and possibly interoperable with the existing financial systems.
