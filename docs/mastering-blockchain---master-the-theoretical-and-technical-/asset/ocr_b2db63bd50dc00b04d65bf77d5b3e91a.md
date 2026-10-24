In the next section, you will be introduced to language solidity. This is a brief introduction to solidity, which should provide the base knowledge required in order to write the contracts. The syntax is very similar to C and JavaScript, and it is quite easy to program.

# Introducing solidity

Solidity is a domain-specific language of choice for programming contracts in Ethereum. There are, however, other languages, such as serpent, Mutan, and LLL but solidity is the most popular at the time of writing this. Its syntax is closer to JavaScript and C. Solidity has evolved into a mature language over the last few years and is quite easy to use, but it still has a long way to go before it can become advanced and feature-rich like other well-established languages. Nevertheless, this is the most widely used language available for programming contracts currently.

It is a statically typed language, which means that variable type checking in solidity is carried out at compile time. Each variable, either state or local, must be specified with a type at compile time. This is beneficial in the sense that any validation and checking is completed at compile time and certain types of bugs, such as interpretation of data types, can be caught earlier in the development cycle instead of at run time, which could be costly, especially in the case of the blockchain/smart contracts paradigm. Other features of the language include inheritance, libraries, and the ability to define composite data types.

Solidity is also a called contract-oriented language. In solidity, contracts are equivalent to the concept of classes in other object-oriented programming languages.

# Types

Solidity has two categories of data types: value types and reference types.

# Value types

These are explained in detail here.

# Boolean

This data type has two possible values, true or false, for example:

```
bool v = true;
```

This statement assigns the value true to v.