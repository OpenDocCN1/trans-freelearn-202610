
## Output parameters of a function
Output parameters of a function are declared in the form of `<data type> <parameter name>`. This example shows a simple function returning a `uint` value:

```solidity
contract myContract
{
    function getValue() returns (uint z)
    {
        z=x+y;
    }
}
```

A function can return multiple values. In the preceding example function `getValue` only returns one value, but a function can return up to 14 values of different data types. The names of the unused return parameters can be omitted optionally.

## Internal function calls
Functions within the context of the current contract can be called internally in a direct manner. These calls are made to call the functions that exist within the same contract. These calls result in simple JUMP calls at the EVM byte code level.

## External function calls
External function calls are made via message calls from a contract to another contract. In this case, all function parameters are copied to the memory. If a call to an internal function is made using the `this` keyword, it is also considered an external call. The `this` variable is a pointer that refers to the current contract. It is explicitly convertible to an address and all members for a contract are inherited from the address.

## Fall back functions
This is an unnamed function in a contract with no arguments and return data. This function executes every time ether is received. It is required to be implemented within a contract if the contract is intended to receive ether; otherwise, an exception will be thrown and ether will be returned. This function also executes if no other function signatures match in the contract. If the contract is expected to receive ether, then the fall back function should be declared with the `payable` modifier. The `payable` is required; otherwise, this function will not be able to receive any ether. This function can be called using the `address.call()` method as, for example, in the following:

```solidity
function ()
{
    throw;
}
```

In this case, if the fallback function is called according to the conditions described earlier; it will call throw, which will roll back the state to what it was before making the call. It can also be some other construct than throw; for example, it can log an event that can be used as an alert to feed back the outcome of the call to the calling application.

## Modifier functions
These functions are used to change the behavior of a function and can be called before other functions. Usually, they are used to check some conditions or verification before executing the function. `_;` (underscore) is used in the modifier functions that will be replaced with the actual body of the function when the modifier is called. Basically, it symbolizes the function that needs to be guarded. This concept is similar to guard functions in other languages.

## Constructor function
This is an optional function that has the same name as the contract and is executed once a contract is created. Constructor functions cannot be called later on by users, and there is only one constructor allowed in a contract. This implies that no overloading functionality is available.

## Function visibility specifiers (access modifiers)
Functions can be defined with four access specifiers as follows:

- **External:** These functions are accessible from other contracts and transactions. They cannot be called internally unless the `this` keyword is used.
- **Public:** By default, functions are public. They can be called either internally or using messages.
- **Internal:** Internal functions are visible to other derived contracts from the parent contract.
- **Private:** Private functions are only visible to the same contract they are declared in.

## Other important keywords/functions
`throw` is used to stop execution. As a result, all state changes are reverted. In this case, no gas is returned to the transaction originator because all the remaining gas is consumed.

# Layout of a solidity source code file

# Version pragma
In order to address compatibility issues that may arise from future versions of the solidity compiler version, pragma can be used to specify the version of the compatible compiler as, for example, in the following:

```solidity
pragma solidity ^0.5.0
```

This will ensure that the source file does not compile with versions smaller than 0.5.0 and versions starting from 0.6.0.

# Import
Import in solidity allows the importing of symbols from the existing solidity files into the current global scope. This is similar to import statements available in JavaScript, as for example, in the following:

```solidity
import "module-name";
```

# Comments
Comments can be added in the solidity source code file in a manner similar to C-language. Multiple line comments are enclosed in `/*` and `*/`, whereas single line comments start with `//`.

An example solidity program is as follows, showing the use of pragma, import, and comments:

```solidity
pragma solidity ^0.4.0; //specify compiler version
/*
This is a simple value checker contract
that checks the value provided and returns boolean value
based on the condition expression evaluation
*/
import "dev.oraClize.it/api.sol";
contract valueChecker {
    uint price=10;
    // This is price variable decared and initialized with value 10.
    event valueEvent(bool returnValue);
    function Matcher (uint8 x) returns (bool)
    {
        if (x>=price)
        {
            valueEvent(true);
            return true;
        }
    }
}
```

Sample solidity program as shown in browser solidity

This completes a brief introduction to the solidity language. The language is very rich and under constant improvement. Detailed documentation and coding guidelines are available online.

# Introducing Web3
Web3 is a JavaScript library that can be used to communicate with an Ethereum node via RPC communication. Web3 works by exposing methods that have been enabled over RPC. This allows the development of user interfaces that make use of the web3 library in order to interact with the contracts deployed over the blockchain.

In order to expose the methods via `geth`, the following command can be used:

```bash
$ geth --datadir .ethereum/privatenet/ --networkid 786 --rpc --rpcapi 'web3,eth,debug' --rpcport 8001 --rpcportdomain 'http://localhost:7777'
```

Note the `--rpcapi` flag that allows the web3, eth and debug methods.

This is a powerful library and can be explored further by attaching a `geth` instance. Later in the section, you will be introduced to the concepts and techniques of making use of web3 via JavaScript/HTML frontends.

The geth instance can be attached using the following command:

```bash
$ geth attach ipc:.ethereum/privatenet/geth.ipc
```

Once the geth JavaScript console is running, web3 can be queried, for example:

![](img/62ab880d2302f959d34db648cb0bec5e_334_0.png)

A simple contract can be deployed using geth and interacted with using web3 via the command-line interface that geth provides (console or attach). The following are the steps to achieve that. As an example, the following source code will be used:

```solidity
pragma solidity ^0.4.0;
contract valueChecker {
    uint price=10;
    event valueEvent(bool returnValue);
    function Matcher (uint8 x) returns (bool)
    {
        if (x>=price)
        {
            valueEvent(true);
            return true;
        }
    }
}
```

Now open the geth console that has been opened previously and follow these steps:

1. Declare a variable named `simplecontractsource` and assign the program code to it:

```javascript
> var simplecontractsource = "pragma solidity ^0.4.0; contract valueChecker { uint price=10;event valueEvent(bool returnValue); function Matcher (uint8 x) returns (bool) { if (x>=price) {valueEvent(true); return true; } } }"
```

This will display the following output:

```javascript
undefined
```

Note that the source code is required to be in a single line, which means that there should be no line breaks. This can be achieved in Linux using the following command:

```bash
$ tr --delete '\n' < valuechecker.sol > valuecheckersingleline.sol
```

In the preceding example, `valuechecker.sol` is the file that has the new line `\n` character, and `valuecheckersingleline.sol` is the output file produced after removing the new line character from the file. The code can then be copied and pasted from the file into the geth JavaScript console.
