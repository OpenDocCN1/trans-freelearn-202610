
## Advanced Topics and the Road Ahead

This chapter deals with select advanced topics related to Ethereum and blockchains. We discuss five broad topics to further your research interests and academic pursuits. We begin with a section on common design patterns that can be applied during smart contract development at enterprise level. Then we move on to the topic of decentralized, autonomous corporations and societies. We devote an entire section to the **Ethereum Improvement Proposal (EIP)**, where we discuss the life cycle of a proposal. Consortium blockchain concepts are revisited with case studies. We conclude the chapter with a unique topic called Tangle and provide indicators to explore the emerging field of graph-based, distributed ledger technology with special emphasis on IOTA.

After studying this chapter, you will be able to:

- Identify different design patterns
- Understand the concept of DACs and DAS
- Understand the Ethereum Improvement Proposal life cycle
- Discuss consortium blockchains using case studies
- Appreciate the concepts behind Tangle and IOTA

### Common design patterns

Recently, my dad retired and found comfort in his passion of cooking. He has his own set of spices that he does not even share with my mom. From chicken to chickpeas, whatever dishes he prepares have a typical hint of his signature masala mix. He argues that the flavor is not about the spices he uses, but the pattern he applies to mix his spices while designing his recipes.

So, what is a design pattern? In software engineering parlance, it is generally a piece of pseudo-code that represents a generic solution that can be reused while developing applications to avoid reinventing-the-wheel scenarios. A common example would be the functionality of passcode-based user-authentication. Most applications require this basic functionality and they are almost similar in design. A user has to first register with the application. The application then sends an OTP or confirmation link to set up a new passcode and registers a new user. Once a registered user has to log in, they have to provide a username and password to the application. The application then checks its registry database for this username and password and permits or blocks the user accordingly.

Software design patterns are broadly classified into three categories, as shown in *Figure 11.1*:

- **Structural:** Such patterns deal with relationships between entities, making it easier for these entities to work together
- **Creational:** Such patterns make it easier to create objects in a way that suits a given situation
- **Behavioral:** Such patterns are used to communicate flexibly between two or more entities.

![](img/8c91673d39176d2e910ac52c4bc35d8b_345_0.png)

Figure 11.1: Software design patterns

These three categories are also called the **Gang of Four (GoF)** design patterns, which refers to its four authors, namely Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides. *Figure 11.2* briefly explains each GoF design pattern.

**Creational patterns**

| Pattern | Description |
| --- | --- |
| Abstract Factory | Allows for the creation of objects without specifying their concrete type |
| Builder | Used to create complex objects |
| Factory Method | Creates objects without specifying the exact class to create |
| Prototype | Creates a new object from an existing object |
| Singleton | Ensures only one instance of an object is created |

**Structural patterns**

| Pattern | Description |
| --- | --- |
| Adapter | Allows for two incompatible classes to work together by wrapping an interface around one of the existing classes |
| Bridge | Decouples an abstraction so two classes can vary independently |
| Composite | Takes a group of objects into a single object |
| Decorator | Allows for an object's behavior to be extended dynamically at run time |
| Facade | Provides a simple interface to a more complex underlying object |
| Flyweight | Reduces the cost of complex object models |
| Proxy | Provides a placeholder interface to an underlying object to control access, reduce cost, or reduce complexity |

**Behavioral patterns**

| Pattern | Description |
| --- | --- |
| Chain of Responsibility | Delegates commands to a chain of processing objects |
| Command | Creates objects which encapsulate actions and parameters |
| Interpreter | Implements a specialized language |
| Iterator | Accesses the elements of an object sequentially without exposing its underlying representation |
| Mediator | Allows loose coupling between classes by being the only class that has detailed knowledge of their methods |
| Memento | Provides the ability to restore an object to its previous state |
| Observer | Is a publish/subscribe pattern which allows a number of observer objects to see an event |
| State | Allows an object to alter its behavior when its internal state changes |
| Strategy | Allows one of a family of algorithms to be selected on-the-fly at run-time |
| Template Method | Defines the skeleton of an algorithm as an abstract class, allowing its sub-classes to provide concrete behavior |
| Visitor | Separates an algorithm from an object structure by moving the hierarchy of methods into one object |

Figure 11.2: GoF design pattern summary

In the following section, we will discuss some frequently used design patterns by smart contract developers on the Ethereum blockchain:

#### Restricting access

Access restriction is a common functionality for smart contracts. In earlier chapters, we discussed how external contracts exploited the access related vulnerability in the Parity library contract to self-destruct the entire wallet library and block $300 million-worth of ethers. We can never restrict any human or computer from reading the contract's state. We can make it a bit difficult by using encryption.

The only viable thing for a developer to keep in mind is that the state of a contract is public, and then design it in a way without problems. We can, however, restrict the number of entities allowed to make modifications to a contract by using function modifiers on the entity addresses. Such addresses can be maintained on an **access control list (ACL)**, but with a caveat: the read-only access will be available to anyone for a public blockchain.

Modifiers are used to change the body of a function. If such modifiers are used, they prepend a check that only passes if the function is called from a certain address.

This design implementation is a kind of structural design pattern.

#### Token systems

These are related to behavioral design patterns. Token systems are analogous to the template method, where the skeleton of an algorithm is defined as an abstract class, allowing the sub-classes to provide concrete behavior.

In the previous chapters, we discussed how the ERC20 standard is implemented for Ethereum-based token systems for compatibility. ERC20 is not a piece of code, software, or technology. It is a technical specification.

Figure 11.3 depicts a sample ERC20 standard contract interface with the recommended function definitions:

```
contract ERC20 {
    function totalSupply() constant returns (uint totalSupply);
    function balanceOf(address _owner) constant returns (uint balance);
    function transfer(address _to, uint _value) returns (bool success);
    function transferFrom(address _from, address _to, uint _value) returns (bool success);
    function approve(address _spender, uint _value) returns (bool success);
    function allowance(address _owner, address _spender) constant returns (uint remaining);
    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _owner, address indexed _spender, uint _value);
}
```

Figure 11.3: ERC20 token system

#### Factory pattern

This design pattern is a kind of creational design pattern. Using this pattern, objects can be created without specifying the exact class of creation.

Figure 11.4 is a simple hub (`Barrel`) that deploys contracts (`Whisky`) from a template and keeps track of the contracts created.

`Whisky` is part of the source file so `Barrel` can "see it" during compilation. `Whisky`'s bytecode will become part of `Barrel` so the `newWhisky()` invocation knows what to do.

It is not necessary to deploy the template (`Whisky`) once the hub (`Barrel`) has been deployed.

We can create as many of the latter as needed by calling a function in the former.
