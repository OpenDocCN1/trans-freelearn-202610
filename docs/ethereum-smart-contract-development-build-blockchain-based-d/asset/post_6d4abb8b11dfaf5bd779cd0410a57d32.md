
applicable to communication networks as depicted in *Figure 2.2:*

![](img/8c91673d39176d2e910ac52c4bc35d8b_62_0.png)

(a) Centralized.

(b) Decentralized.

(c) Distributed networks.

**Figure 2.2:** Centralized, decentralized, and distributed architecture

Now, we need to discuss whether Ethereum is decentralized. You might have heard someone say that Ethereum is not decentralized. They might be partially correct. Even Vitalik Buterin, one of the founders of Ethereum, says so. You might be thinking that this is completely crazy. Of course, Ethereum ought to be decentralized. It is a blockchain technology, and the whole idea of a blockchain technology is that it should be decentralized.

However, the answer to this is not a simple yes or no. It is a bit more complex than that. Vitalik Buterin has analyzed this notion of decentralization, which we will discuss right now.

Decentralization consists of at least three important axes: the architectural axis, the political axis, and the logical axis. A system such as Ethereum can either be centralized or decentralized along these three orthogonal axes. It can also sometimes be the case that it is hard to draw a line and say that this system is centralized or decentralized as a whole. It is just hard to decide in practical terms.

Let us start with what we mean by the architectural axis. If a system is centralized in the architectural axis and I remove a couple of nodes/computing terminals from the system, it would collapse. Conversely, in a system that is decentralized on the architectural axis, even if I remove the majority of the nodes/computing terminals, the system would still function as intended. So, in that sense, Ethereum is decentralized on the architectural axis, because we can remove many computing nodes from the system but the whole network will still function. However, if you have a website that you host on a server or a network of servers where each one needs all of the others, and you remove one server, the servers and the website will obviously no longer function as this was a centralized system on the architectural axis.

Let us move on. The second axis is the political axis.

For example, a centralized system on the political axis could be a company or an enterprise where we have a CEO and maybe we have a board of directors, and they decide on the course of the company. However, Ethereum is decentralized on the political axis, because even the Ethereum Foundation cannot make everyone use their clients or force everyone to follow a protocol.

Now, the last axis of discussion is the logical axis. So, a system that is centralized on the logical axis acts as a single entity, as a monolithic object, while a system that is decentralized on the logical axis can be divided into several parts, and each of those parts would behave and function as intended.

For example, the BitTorrent protocol and the BitTorrent system is decentralized on the logical axis because we can split the system into several parts, and the computers in those split parts will still be able to use the BitTorrent protocol as intended. However, here is where Ethereum is not decentralized. In fact, Ethereum is centralized when it comes to the logical axis, because Ethereum acts as a single computer. You execute your contracts on Ethereum and on this one huge computer that is running on the blockchain. So, in that sense, Ethereum is centralized. If we split Ethereum into two different parts, then they would no longer function as intended because the whole idea of Ethereum is that it should be a single computer—it should act as a single computer. In that way, Ethereum is centralized on the logical axis.

*Figure 2.3* illustrates all three notions of decentralization for Ethereum:

![](img/8c91673d39176d2e910ac52c4bc35d8b_64_0.png)

| Axis | Decentralized | Centralized |
| --- | --- | --- |
| Architectural | Yes | No |
| Political | Yes | No |
| Logical | No | Yes |

Figure 2.3: Notion of decentralization for Ethereum

The question still remains: how does Ethereum, as a distributed computing platform, achieve such a level of centralized logical abstraction, despite having a fully decentralized architecture? To answer that, we need to get familiar with various components of the Ethereum ecosystem and try to construct its technological stack. The next section does just that. So, let us dive in.

## The Ethereum ecosystem

In Chapter 1, *Blockchain Basics*, we studied how an Ethereum blockchain is represented as a giant Merkle tree with the leaves of the tree being the state of execution of a code. We also learned that blockchains are distributed ledgers with a web 3.0 framework.

*Figure 2.4* depicts such a framework for an Ethereum technological stack. Things are going to get tricky now, so please pay close attention:

![](img/8c91673d39176d2e910ac52c4bc35d8b_65_0.png)

Figure 2.4: Web 3.0 tech stack for Ethereum, Source: Ethereum stack exchange

![](img/8c91673d39176d2e910ac52c4bc35d8b_67_0.png)

Figure 2.5: Mist browser analogy

## Decentralized applications

Now, let us discuss the next layer: **decentralized applications (DApps)**.
These are basically applications that get executed over a decentralized peer-to-peer network and have an open-source code. It can be a simple logic-based program that adds two numbers, or it can be something quite complex, such as a microblogging service.

Being a foodie, I often imagine a Mist browser as a five-star hotel's weekend brunch buffet table (along with gas burners) on which different kinds of DApps are spread out, ready to be devoured by hungry software developers and clients. The only thing is that these DApps, rather than coming from a single centralized kitchen employing many chefs, are procured in a potluck manner by different software developers. They are even made to pay for the gas burner to keep their DApps palatable. The best part is that the developers must share the recipe and ingredients for free. *Figure 2.6* shows such DApps on a Mist browser.

## Middleware

Mist and DApps are mainly part of the frontend stack of Ethereum. Let us now move into backend mechanisms. First, we start with the **middleware**. The middleware of Ethereum has a flat hierarchy, as there is no central authority, such as a server. It consists of three prime components of equal importance:

- **Swarm**: Ethereum's decentralized storage, mainly for static files
- **Whisper**: The walkie-talkie of DApps, that is an identity-based messaging system
- **EVM**: An isolated, on-the-fly, human-to-machine code translator, that is in a sandboxed runtime environment

How does an app run without a server? We can figure this out by understanding that the servers in a web 2.0 framework do quite a lot more than they were intended to do in order to keep the client device light. Not only do they serve static web pages, they store private information, administer user authentication, and compute all the complex analytics. All the client side does is to load and display the message to the user. A serverless architecture in web 3.0, on the other hand, allows a much more modular approach, in which different computing devices and different protocols handle specific tasks; some on the client side and some in specialized nodes deployed on a peer-to-peer network. Therefore, all the data logic comprising logs about what is getting saved, who is saving it and resolving conflicts are handled by smart contracts on the blockchain; static files are served via Swarm and real-time communication is taken care of by Whisper, as shown in *Figure 2.7*. The client device keeps the user authentication and runs the application interface, that is, the Mist wallet plus the browser:

![](img/8c91673d39176d2e910ac52c4bc35d8b_71_0.png)

Figure 2.7: Serverless architecture, Source: blog.ethereum.org

Figure 2.8 illustrates how these modules work in unison with DApps and the underlying blockchain in a serverless architecture. Such a setup also encourages innovation. As the interfaces are detached from the data, anybody can develop a new interface for the same application, creating a more vibrant and competitive ecosystem:
