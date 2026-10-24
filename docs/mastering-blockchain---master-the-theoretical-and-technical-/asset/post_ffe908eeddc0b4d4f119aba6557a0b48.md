
```
name: "valueEvent",
type: "event"
}],
compilerOptions: "--combined-json bin,abi,userdoc,devdoc --add-std --optimize", compilerVersion: "0.4.6",
developerDoc:
{
methods: {}
},
language: "Solidity",
languageVersion: "0.4.6",
source: "pragma solidity ^0.4.0; contract valueChecker { uint price=10; event valueEvent(bool returnValue); function Matcher (uint8 x) returns (bool) { if (x>=price) { valueEvent(true); return true; } } }",
userDoc: {
methods: {}
}
}
}
}
```

This file contains various elements. The most important is **ABI (Application Binary Interface)**, which can be queried using `geth`, as shown earlier in step 6 during the contract deployment process.

+   3. Create a file named `simplecontract.js`, as shown here:

```
```javascript
if (typeof web3 !== "undefined")
{
  web3 = new Web3(web3.currentProvider);
}
else
{
  web3 = new Web3(new
  Web3.providers.HttpProvider("http://localhost:8001"));
}
console.log("Coinbase: " + web3.eth.coinbase);
var simplecontractaddress = "0x94a1107f2585f0ab931c71f2f8f02e9
f5ab888c0";
simplecontractinstance =
web3.eth.contract(simplecontractcompiled.valueChecker
.info.abiDefinition).at(simplecontractaddress);
var code = web3.eth.getCode(simplecontractaddress);
console.log("Contract balance: " +
web3.eth.getBalance(simplecontractaddress));
console.log("Simple contract code" + code);
function callMatchertrue()
```

```
{
    var txn = simplecontractinstance.Matcher.call(12);{
};
console.log("return value: " + txn);
}
function callMatcherfalse()
{
var txn = simplecontractinstance.Matcher.call(1);{
};
console.log("return value: " + txn);
}
```

This file is the main JavaScript file that contains the code to create a web3 object. It also provides methods that are used to interact with the contract on the blockchain. An explanation of the code is given here.

## Creating a web3 object

```
if (typeof web3 !== 'undefined')
{
    web3 = new Web3(web3.currentProvider);
}
else
{
web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:
    8001"));
}
```

This code first checks whether there is already an available provider; if yes, then it will set the provider to the current provider. Otherwise, it sets the web3 provider to localhost: 8001; this is where the local instance of geth is running.

## Checking availability by calling any web3 method

```
console.log("Coinbase: " + web3.eth.coinbase);
```

This line of code simply uses console.log to print the coinbase by calling the web3.eth.coinbase method. Once this call is successful, it means that the web3 object has been created correctly and HttpProvider is available. Any other call can be used to verify the availability, but as a simple example, web3.eth.coinbase has been used in the preceding example.

## Assigning contract address to a variable

```
var simplecontractaddress = "0x94a1107f2585f0ab931c71f2f8f02e9f5ab888c0";
```

```
```javascript
console.log("return value: " + txn);
}

function callMatcherfalse()
{
var txn = simplecontractinstance.Matcher.call(1);{
};
console.log("return value: " + txn);
}
```
```

Calls can be made using `simplecontractinstance.Matcher.call` and then by passing the value for the argument. Recall the function matcher in solidity code:

```
```solidity
function Matcher (uint8 x) returns (bool)
```
```

It takes one argument x of type `uint8` and returns a Boolean value, either true or false.

Accordingly, the call is made to the contract, as shown here:

```
```javascript
var txn = simplecontractinstance.Matcher.call(12);
```
```

In the preceding example, `console.log` is used to print the value returned by the function call. Once the result of the call is available in the `txn` variable, it can be used anywhere throughout the program, for example, as a parameter for another JavaScript function.

Finally, the HTML file named `index.html` is created with the following code:

```
```html
<html>
<head>
    <title>SimpleContract Interactor</title>
    <script src="./web3.min.js"></script>
    <script src="./simplecontractcompiled.js"></script>
    <script src="./simplecontract.js"></script>
</head>
<body>
    <button onclick="callMatchertrue()">callTrue</button>
    <button onclick="callMatcherfalse()">callFalse</button>
</body>
</html>
```
```

It is recommended that an appropriate web server be running in order to serve the HTML content (`index.html` as an example). Alternatively, the file can browsed from the filesystem but that can cause some issues with larger projects; as good practice, always use a web server. A quick web server in Python can be started using the following command. This server will serve the HTML content from the same directory that it has been run from. Python is not necessary; it can even be an Apache server or any other web container.

```
imran@drequinnox-OP7010:~/simplecontract/app$ python -m SimpleHTTPServer 7777
Serving HTTP on 0.0.0.0 port 7777 ...
```

Simple web server in Python

Now any browser can be used to view the web page served over TCP port 7777. This is shown in the following example. It should be noted that the output shown here is in the browser's console window. The browser's console must be enabled in order to see the output.

![](img/62ab880d2302f959d34db648cb0bec5e_348_0.png)

Interaction with the contract

As the values are hardcoded in the code for simplicity, two buttons have been created in index.html. Both of these buttons call functions with hardcoded values. This is just to demonstrate that parameters are being passed to the contract via web3 and values are being returned accordingly.

There are two functions being called behind the preceding buttons. The callMatchertrue() method has a hardcoded value of 12, which is sent to the contract using this:

```
simplecontractinstance.Matcher.call(12)
```

The return value is printed in the console using the following code, which first invokes the Matcher function and then assigns the value to the txn variable to be printed later in the console:

```
simplecontractinstance.Matcher.call(1)
function callMatchertrue()
{
  var txn = simplecontractinstance.Matcher.call(12);{
};
console.log("return value: " + txn);
}
```

Similarly, the callMatcherfalse() function works by passing a hardcoded value of 1 to the contract using this:

```
simplecontractinstance.Matcher.call(1)
```

The return value is printed accordingly:

```
console.log("return value: " + txn);
function callMatcherfalse()
{
  var txn = simplecontractinstance.Matcher.call(1);{
};
console.log("return value: " + txn);
}
```

This example demonstrates how the web3 library can be used to interact with the contracts on the blockchain.

## Development frameworks

There are various development frameworks now available for Ethereum. As seen in the examples discussed earlier, it can be quite time consuming to deploy the contract via the usual manual means. This is where truffle and similar frameworks such as embark can be used to make the process simpler and quicker. The most widely used framework is called truffle. In the next section, you will be introduced to the truffle framework.

## Truffle

Truffle is a development environment that makes it easier and simpler to test and deploy Ethereum contracts. Truffle provides contract compilation and linking along with an automated testing framework using Mocha and Chai. It also makes it easier to deploy the contracts to any PrivateNet, public, or Test Net Ethereum blockchain. Also, asset pipeline is provided, which makes it easier for all JavaScript files to be processed, making them ready for use by a browser.

## Installation

Before installation, it is assumed that the node is available, which can be queried as shown here. If the node is not available, then the installation of the node is required first in order to install truffle:

```
drequinox@drequinox-OP7010:~/testdapp$ nodejs --version
```

```
v7.2.1
drequinox@drequinox-OP7010:~/testdapp$ node --version
v7.2.1
```

The installation of truffle is very simple and can be done using the following command via npm:

```
$ sudo npm install -g truffle
```

This will take a while; once it is installed, truffle can be used to display help and make sure that it is installed correctly.

![](img/62ab880d2302f959d34db648cb0bec5e_350_0.png)

Alternatively, the repository is available at https://github.com/ConsenSys/truffle, which can be cloned locally to install truffle. Git can be used to clone the repository using the following command:

```
https://github.com/ConsenSys/truffle.git
```

## Initializing truffle

Truffle can be initialized by running the following command. First, create a directory for the project, for example:

```
mkdir testdapp
```

Then, change to testdapp and run the following command:

```
~/testdapp$ truffle init
```
