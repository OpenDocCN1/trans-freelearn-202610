The solidity online compiler can be used to run and test this contract. The Application Binary Interface (ABI) required for interacting with the contract is also available in the Interface field as shown in the following screenshot:

![](bbox=[0.105, 0.16, 0.895, 0.5])

Solidity online compiler

There are two methods by which Raspberry node can connect to the private blockchain via the web3 interface. The first is where the raspberry device is running its own geth and maintains its own ledger but with resource-constrained devices it is not possible to run a full geth node, or even a light node in a few circumstances. In that case, the web3 provider can be initialized to connect to the appropriate RPC channel. This will be shown later in the client JavaScript Node.js programme. A comparison of both of these approaches is shown in the following diagram: