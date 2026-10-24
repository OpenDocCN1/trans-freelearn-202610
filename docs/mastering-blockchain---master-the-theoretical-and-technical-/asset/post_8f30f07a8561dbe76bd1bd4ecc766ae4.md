
## Call functions

The `call`, `callcode`, and `delegatecall` are provided in order to interact with functions that do not have **Application Binary Interface (ABI)**. These functions should be used with caution as they are not safe to use due to the impact on type safety and security of the contracts.

## Array value types (fixed size and dynamically sized byte arrays)

Solidity has fixed size and dynamically sized byte arrays. Fixed size keywords range from `bytes1` to `bytes32`, whereas dynamically sized keywords include bytes and strings. `bytes` are used for raw byte data and string is used for strings encoded in UTF-8. As these arrays are returned by the value, calling them will incur gas cost. `length` is a member of array value types and returns the length of the byte array.

An example of a static (fixed size) array is as follows:

```
bytes32[10] bankAccounts;
```

An example of a dynamically sized array is as follows:

```
bytes32[] trades;
```

Get `length` of trades:

```
trades.length;
```

## Literals

These are used to represent a fixed value.

## Integer literals

Integer literals are a sequence of decimal numbers in the range of 0-9. An example is shown as follows:

```
uint8 x = 2;
```

## String literals

String literals specify a set of characters written with double or single quotes. An example is shown as follows:

```text
'packt'
"packt"
```

## Hexadecimal literals

Hexadecimal literals are prefixed with the keyword hex and specified within double or single quotation marks. An example is shown as follows:

```text
(hex'AABBCC');
```

## Enums

This allows the creation of user-defined types. An example is shown as follows:

```text
enum Order{Filled, Placed, Expired};
Order private ord;
ord=Order.Filled;
```

Explicit conversion to and from all integer types is allowed with enums.

## Function types

There are two function types: internal and external functions.

## Internal functions

These can be used only within the context of the current contract.

## External functions

External functions can be called via external function calls.

A function in solidity can be marked as a constant. Constant functions cannot change anything in the contract; they only return values when they are invoked and do not cost any gas. This is the practical implementation of the concept of *call* as discussed in the previous chapter.

The syntax to declare a function is shown as follows:

```
function <nameofthefunction> (<parameter types> <name of the variable>)
{internal|external} [constant] [payable] [returns (<return types> <name of the variable>)]
```

## Reference types

As the name suggests, these types are passed by reference and are discussed in the following section.

## Arrays

Arrays represent a contiguous set of elements of the same size and type laid out at a memory location. The concept is the same as any other programming language. Arrays have two members named length and push:

```
uint[] OrderIds;
```

## Structs

These constructs can be used to group a set of dissimilar data types under a logical group. These can be used to define new types, as shown in the following example:

```
Struct Trade
{
uint tradeid;
uint quantity;
uint price;
string trader;
}
```

## Data location

Data location specifies where a particular complex data type will be stored. Depending on the default or annotation specified, the location can be storage or memory. This is applicable to arrays and structs and can be specified using the storage or memory keywords. As copying between memory and storage can be quite expensive, specifying a location can be helpful to control the gas expenditure at times. Calldata is another memory location that is used to store function arguments. Parameters of external functions use calldata memory. By default, parameters of functions are stored in memory, whereas all other local variables make use of storage. State variables, on the other hand, are required to use storage.

## Mappings

Mappings are used for a key to value mapping. This is a way to associate a value with a key. All values in this map are already initialized with all zeroes, for example, the following:

```
mapping (address => uint) offers;
```

This example shows that offers is declared as a mapping. Another example makes this clearer:

```
mapping (string => uint) bids;
bids["packt"] = 10;
```

This is basically a dictionary or a hash table where string values are mapped to integer values. The mapping named bids has a packt string value mapped to value 10.

## Global variables

Solidity provides a number of global variables that are always available in the global namespace. These variables provide information about blocks and transactions. Additionally, cryptographic functions and address-related variables are available as well.

A subset of available functions and variables is shown as follows:

```
keccak256(...) returns (bytes32)
```

This function is used to compute the keccak256 hash of the argument provided to the function:

```
ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)
```

This function returns the associated address of the public key from the elliptic curve signature:

```
block.number
```

This returns the current block number.

## Control structures

Control structures available in solidity are if - else, do, while, for, break, continue, return. They work in a manner similar to how they work in C-language or JavaScript.

## Events

Events in solidity can be used to log certain events in EVM logs. These are quite useful when external interfaces are required to be notified of any change or event in the contract. These logs are stored on the blockchain in transaction logs. Logs cannot be accessed from the contracts but are used as a mechanism to notify change of state or the occurrence of an event (meeting a condition) in the contract.

In a simple example here, the valueEvent event will return true if the x parameter passed to function Matcher is equal to or greater than 10:

```
contract valueChecker {
    uint8 price=10;
    event valueEvent(bool returnValue);
    function Matcher(uint8 x) returns (bool)
    {
        if (x>=price)
        {
            valueEvent(true);
            return true;
        }
    }
}
```

## Inheritance

Inheritance is supported in solidity. The is keyword is used to derive a contract from another contract. In the following example, valueChecker2 is derived from the valueChecker contract. The derived contract has access to all nonprivate members of the parent contract:

```
contract valueChecker
{
    uint8 price=10;
    event valueEvent(bool returnValue);
    function Matcher(uint8 x) returns (bool)
    {
        if (x>=price)
        {
            valueEvent(true);
            return true;
        }
    }
}
contract valueChecker2 is valueChecker
{
```

In the preceding example, `function` is the keyword used to declare the function. `OrderMatcher` is the function name, `uint x` is an optional parameter, `private` is the **access modifier/specifier** that controls access to the function from external contracts, `constant` is an optional keyword used to specify that this function does not change anything in the contract but is used only to retrieve values from the contract instead, and `returns (bool returnvalue)` is the optional return type of the function.

- **How to define a function:** The syntax of defining a function is shown as follows:

```
function <name of the function>(<parameters>) <visibility
specifier> returns (<return data type> <name of the variable>)
{
  <function body>
}
```

- **Function signature:** Functions in solidity are identified by its signature, which is the first four bytes of the keccak-256 hash of its full signature string. This is also visible in browser solidity, as shown in the following screenshot. **D99c89cb** is the first four bytes of 32 byte keccak-256 hash of the function named **Matcher**.

![](img/62ab880d2302f959d34db648cb0bec5e_330_0.png)

In this example function, **Matcher** has the signature hash of **d99c89cb**. This information is useful in order to build interfaces.

- **Input parameters of a function:** Input parameters of a function are declared in the form of `<data type> <parameter name>`. This example clarifies the concept where `uint x` and `uint y` are input parameters of the `checkValues` function:

```
contract myContract
{
  function checkValues(uint x, uint y)
  {
  }
}
```
