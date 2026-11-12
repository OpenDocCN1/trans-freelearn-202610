
A simplified microservices-based architecture would look as follows:

## Microservices-Based Architecture

![](img/a3b1eecf3c87c814267f2877e8a8be1a_110_0.png)

Figure 3.5 – An example of a microservices-based architecture

## API-led Architecture

The **API-led architecture** is an API strategy where all external and internal services are exposed as managed APIs, regardless of how they were implemented (microservices, SOA services, web services driven out of a monolithic application, or based on other architectures). **Managed APIs**, in today’s modern terms, do more than just provide governance capabilities such as security policies, throttling, versioning, and automatic service discovery. The principle has extended beyond this to include developer portals where they can experiment with APIs before using them, productivity tools, and a mechanism to register and pay for API usage. In this approach, APIs are usually organized into three different layers:

- System APIs: These APIs are used to access core systems and services. They provide a simplified insulating layer between the service consumer and the underlying system or service.

- Process APIs: These APIs are used to interact with, transform, and shape the data coming from the underlying system APIs or from other process APIs, effectively breaking down data silos. They have no dependency on the source systems where the data came from, nor on the target systems where the data will be delivered. Both system APIs and process APIs can be used to connect to existing microservices as well as other enterprise services, depending on the use case.

- Experience APIs: These APIs are used to allow easy access and data consumption for the end user or application. They typically communicate with one or more process APIs to deliver specific functionality.

Microservices are known to create many endpoints, which are normally difficult to control and monetize. The API-led architecture aims to create an API strategy that governs the way the different enterprise services interact with each other, as well as with external consumers, by utilizing the capabilities of lightweight standards such as REST and combining them with modern API gateway capabilities.

Microservices are typically consumed by applications. The API-led architecture aims to turn these applications into a smaller and lighter set of APIs. This can help enterprises take steps toward the API economy. For example, an enterprise could create a set of APIs on top of its rich set of services, which are built with different technologies and based on different architectures, and then utilize an API manager to expose these services externally and internally with different applicable policies and subscription mechanisms.

Moreover, this approach is also seen as an enabler for rapid application development since you can reuse APIs that are built on top of different business processes. MuleSoft Anypoint Platform is a tool that enables enterprises to deliver API-led integration architecture.

## Event-driven Architecture

Event-driven architecture is an approach to software development that utilizes events to communicate and trigger actions in integrated and decoupled applications. An event is simply a change in the status of a particular object. For example, a change in the customer status value could fire a customer status change event that would trigger a set of actions in integrated systems, such as starting a marketing journey.

The event-driven architecture inherits some principles from the messaging integration style, as mentioned earlier. The event-driven architecture has three main components, that is, event producers, event routers, and event consumers. The producers publish events to the router, the routers handle filtering and pushing the events to the subscribed consumers, and the consumers receive the event, parse it, transform it into a format suitable for their needs, and then use it, typically to update their own version of the data or to fire subsequent logic. Stream-processing platforms, modern ESBs, or event-routing buses such as CometD are usually used as routers.

Now that you have covered an important topic of modern integration approaches, you can move on to another key set of architectural concepts. Cryptography is one of the foundations on which modern digital communications are built. It has been used for centuries but flourished in the digital era. Cryptography concepts are closely related and intensively used in integration. In the next section, you will go through some general cryptography concepts, understand the key cryptography algorithm types, and discuss some of their use cases in detail.

## Cryptography: Understanding the General Concepts

Cryptography has a tight relationship with other architectural domains, such as integration and identity and access management (IAM). It also has strong relationships with data. As a Salesforce Architect, you need to have a general understanding of the value of cryptography and different types of cryptography algorithms and a high-level understanding of the process. This will help you understand the intricacies of the day-to-day activities that take place in Salesforce implementation projects, such as securing an integration channel using Transport Layer Security (TLS) or two-way TLS (also known as mutual authentication), or how authentication tokens are digitally signed.

**Encryption** is the process of converting original readable data (also known as plaintext) into a form that cannot be read by unauthorized parties (also known as ciphertext). Encryption is not a method of preventing others from interfering with data; rather, it is a mechanism that denies access to the readable form of the data to unauthorized parties. In other words, an attacker would end up with a version of the data that is not useful and cannot be converted back into a useful format in a reasonable timeframe.

Take ancient encryption algorithms as an example. The transposition cipher was a classic and very basic way to encrypt messages sent with messengers across unsafe territories. In a transposition cipher, the order of a word's letters is rearranged based on specific logic. A statement such as hello world could end up as ehllo owrdl. If the message falls into the wrong hands, they can still destroy or tamper with it, but ideally, they should not be able to read it.

Another example of ancient encryption is substitution ciphers, which rely on replacing letters or symbols with other letters or symbols based on a rule known only to the message generator and the authorized parties. For example, London becomes Mpoeop by replacing each letter with the next letter in the English alphabet. Typically, the message can be easily decrypted once you know two things, that is, the algorithm and the key. In this example, the algorithm is the substitution cipher and the key is the letter order in the English alphabet + 1.

In present times, these algorithms are simple to crack using computers. Today's cryptography algorithms are much more sophisticated and harder to break.

Until modern times, cryptography and encryption mainly referred to the same thing. However, during World War I, there was a leap in the techniques that were used for cryptography. First, rotor cipher machines were used, and then more computational capabilities were developed during World War II.

Modern cryptography relies on computer science practices and mathematical theory. They are designed to provide a mechanism that protects the contents of the encrypted data by rendering attempts to break them infeasible. Theoretically, all encryption algorithms can be broken, given enough time and considering the available computational power (the most basic form of attack is brute force, which, in turn, tries every possible key until the right one is found). However, considering that this process could be infeasible to achieve makes these algorithms computationally secure. For example, to break AES-256 using brute force, an attacker would need to try an average of 2^255 keys. It is estimated that
