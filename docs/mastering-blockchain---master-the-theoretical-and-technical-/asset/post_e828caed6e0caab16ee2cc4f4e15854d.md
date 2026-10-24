
Furthermore, when a command is typed, it can be autocompleted by pressing tab twice. If two tabs are pressed, then the list of available methods is also displayed. This is shown in the following screenshot:

![](img/62ab880d2302f959d34db648cb0bec5e_304_0.png)

In addition to the previously mentioned command, in order to get a list of available methods, after typing any command, ';' (semicolon) is entered. An example is shown in the next screenshot, which shows a list of all the methods available for net:

![](img/62ab880d2302f959d34db648cb0bec5e_304_1.png)

There are a few other commands that can be used to query the private network. Some examples are shown as follows:

- Get the current gas price:
  
  ```text
  > eth.gasPrice
  20000000000
  ```

- Get the latest block number:
  
  ```text
  > eth.blockNumber
  587
  ```

- Get a list of compilers. Note that if no output is shown, it means that no solidity compiler is installed; solidity compiler installation details are provided later in the chapter:

```
> web3.eth.getCompilers()
["Solidity"]
```

## Running Mist on Private Net

It is possible to run Mist on Private Net by issuing the following command. This binary is usually available in the home folder after the installation of /opt/Ethereum:

```
$ ./Ethereum\ Wallet --rpc ~/.ethereum/privatenet/geth.ipc
```

This will allow a connection to the running Private Net geth session, and it provides all features, such as wallet, account management, and contract deployment on Private Net via Mist.

![](img/62ab880d2302f959d34db648cb0bec5e_306_0.png)

Once Ethereum is launched, it will show the interface shown here, indicating clearly that it's running in the **PRIVATE-NET** mode.

![](img/62ab880d2302f959d34db648cb0bec5e_307_0.png)

Mist on Private Net

Mist can also run over the network using RPC. This is useful if `geth` is running on a different node and Mist on another. This can be achieved by running Mist with the flag shown here:

```text
--rpc http://127.0.0.1:8545
```

## Deploying contracts using Mist

It is very easy to deploy new contracts using Mist. Mist provides an interface where contracts can be written in solidity and then deployed on the network.

In the exercise, a simple contract that can perform various simple arithmetic calculations on the input parameter will be used. Steps on how to use Mist to deploy this contract are shown here. As solidity has not been introduced yet, the aim here is to allow users to experience the contract deployment and interaction process. More information on coding and solidity will be provided later in the chapter, after which it will become easy to understand the code shown. Those of you who are already familiar with JavaScript or any other similar language will find the code almost self-explanatory.

The example contract source code is shown as follows:

```
pragma solidity ^0.4.0;
contract SimpleContract2
{
    uint x;
    uint z;
    function addition(uint x) returns (uint y)
    {
        z=x+5;
        y=z;
    }
    function difference(uint x) returns (uint y)
    {
        z=x-5;
        y=z;
    }
    function division(uint x) returns (uint y)
    {
        z=x/5;
        y=z;
    }

    function currValue() constant returns (uint)
    {
        return z;
    }
}
```

This code can simply be copied into Mist under the contracts section, as shown here. On the left-hand side, the source code can be copied; once verified and when no syntax errors are detected, the option to deploy the contract will appear in the drop-down menu on the right-hand side where it says SELECT CONTRACT TO DEPLOY. Simply select the contract and press the Deploy button at the bottom of the screen.

![](img/62ab880d2302f959d34db648cb0bec5e_308_0.png)

Mist will ask for the password of the account and will show a window similar to the one in the following screenshot:

![](img/62ab880d2302f959d34db648cb0bec5e_309_0.png)

Enter the password and click on **SEND TRANSACTION** to deploy the contract.

Once deployed and mined successfully, it will appear in the list of transactions in Mist, as shown here:

![](img/62ab880d2302f959d34db648cb0bec5e_310_0.png)

List of transactions after creation in Mist

Once the contract is available, it can be interacted with using the execute transaction and calling available functions via Mist.

![](img/62ab880d2302f959d34db648cb0bec5e_310_1.png)

Interaction with the contract using read and write options in Mist

In the preceding screenshot, the **READ FROM CONTRACT** and **WRITE TO CONTRACT** options are available. Also, the function that has been exposed by the contract can be seen on the right-hand side. Once the required function is selected, the appropriate value is entered for the function and the account (execute from) is selected; press execute in order to execute the transaction, which will result in calling the selected function of the contract.

This process is shown in the following screenshot:

![](img/62ab880d2302f959d34db648cb0bec5e_311_0.png)

As shown in the screenshot, enter the appropriate password for the account and then press **SEND TRANSACTION** to send the transaction to the contract.

## Development tools and clients

There are a number of tools available for Ethereum development. The following diagram shows the taxonomy of various development tools, clients, IDEs, and development frameworks for Ethereum:

![](img/62ab880d2302f959d34db648cb0bec5e_312_0.png)

In this chapter, the main focus will be on geth, browser solidity, solidity, solc, and truffle. Rest of the elements will be discussed briefly.

## Languages

Contracts can be programmed in a variety of languages. There are four languages that can be used in order to write contracts:

- **Mutan**: This is a Go-style language, which was deprecated in early 2015 and is no longer used.
- **LLL**: This is a Lisp-like language, hence the name LLL. This is also not used anymore.
- **Serpent**: This is a simple and clean Python-like language. It is actively used for contract development.
- **Solidity**: This language has now become almost a standard for contract writing for Ethereum. This language is the focus of this chapter and is discussed in detail in later sections.

## Compilers

Compilers are used to convert high-level contract source code into the format that the Ethereum execution environment understands. The solidity compiler is the most common one in use and is discussed here.

## Solc

The solidity compiler converts from a high-level solidity language into **Ethereum Virtual Machine (EVM)** bytecode so that it can be executed on the blockchain by EVM.

The solidity compiler on a Linux Ubuntu operating system can be installed using the following commands:

```
$ sudo apt-get install solc
```

If PPAs are not already installed, those can be installed by running the following command:

```
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
```

In order to verify the existing version of the solidity compiler and verify that it is installed, the following command can be used:

```
$ solc --version
solc, the solidity compiler commandline interface
Version: 0.4.6+commit.2dabdf0.Linux.g++
```

Solc supports a variety of functions. A few examples are shown as follows:

- Display contract in a binary format.

![](img/62ab880d2302f959d34db648cb0bec5e_313_0.png)

Solidity compiler binary output

- Estimate gas:

```
imran@drequinox-OP7010:~$ solc --gas contract1.sol
====== SimpleContract ======
Gas estimation:
construction:
  97 + 54600 = 54697
external:
  division(uint256):  230
  addition(uint256):  231
  difference(uint256): 253
```

# internal:

Note that contrat1.sol is shown as an example only; the file can contain any smart contract solidity code. The code of the file is not shown here.

Solc is used internally by web3 from the geth console in order to compile the contract. The syntax is shown here, and contractsourcecode is the solidity source code:

```
web3.eth.compile.solidity(contractsourcecode)
```

This is discussed in detail later in the chapter when you will be introduced to contract development.

## Integrated Development Environments (IDEs)

There are various IDEs available for solidity development. Most of the IDEs are available online and are presented via web interfaces. Browser solidity is most commonly used for smaller contracts and is discussed here.

## Browser solidity

Browser solidity is the web-based environment for the development and testing of contracts using solidity. It does not run on a live blockchain; in fact, it is a simulated environment in which contracts can be deployed, tested, and debugged. It is available at https://ethereum.github.io/browser-solidity. An example interface is shown as follows:

![](img/62ab880d2302f959d34db648cb0bec5e_314_0.png)

Browser solidity
