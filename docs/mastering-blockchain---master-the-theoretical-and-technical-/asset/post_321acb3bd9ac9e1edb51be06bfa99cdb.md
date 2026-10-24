
# Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books-maybe a mistake in the text or the code-we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the Errata Submission Form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title.

To view the previously submitted errata, go to https://www.packtpub.com/books/content/support and enter the name of the book in the search field. The required information will appear under the Errata section.

# Piracy

Piracy of copyrighted material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors and our ability to bring you valuable content.

# Questions

If you have a problem with any aspect of this book, you can contact us at questions@packtpub.com, and we will do our best to address the problem.

# 1
## Blockchain 101

It is very likely that anyone reading this book has already heard about blockchain and has some basic appreciation of its enormous potential.

With the invention of bitcoin in 2008 the world was introduced to a new concept that is now likely to revolutionize the whole of society. It's something that has promised to impact every industry including but not limited to finance, government, and media. Some describe it as a revolution whereas another school of thought says that it's going to be an evolution and it will take many years before any practical benefits from blockchain come to fruition. This is correct to some extent but in my opinion the revolution has already started; many big organizations all around the world are already writing proofs of concept using blockchain technology as its disruptive potential has now been fully recognized. However, some organizations are still at the preliminary exploration stage but are expected to progress more quickly as the technology is now becoming more mature. It is a technology that has an impact on current technologies too and possesses the ability to change them at a fundamental level.

According to Gartner's technology hype cycle graph shown below, the blockchain technology is currently at the peak of inflated expectations (as of July 2016) and is expected to be ready for mainstream adoption in 5 to 10 years:

![](img/62ab880d2302f959d34db648cb0bec5e_33_0.png)

Source: Gartner (August 2016)

Gartner's hype cycle for emerging technologies

Interest in blockchain technology has soared in the last few years and, once disregarded by some as geek money from a cryptocurrency point of view or as something that was not really considered worthwhile, it is now being researched by the largest companies and organizations around the world with millions of dollars being spent in order to adopt and experiment with this technology. A simple trend search on Google reveals the scale of interest in the blockchain technology over the last few years:

![](img/62ab880d2302f959d34db648cb0bec5e_34_0.png)

Google trends for blockchain

Various benefits of this technology are being envisaged such as decentralized trust, cost savings, transparency, and efficiency. However, there are various challenges too that are an area of active research such as scalability and privacy. Chapter 12, Scalability and Other Challenges is dedicated to a discussion of the limitations and challenges of blockchain technology.

This chapter is an introduction to blockchain technology, its technical foundations, the theory behind it, and various technologies that have been combined together in order to build what is known today as blockchain.

In 2008 a groundbreaking paper Bitcoin: A Peer-to-Peer Electronic Cash System was written on the topic of peer-to-peer electronic cash under the pseudonym Satoshi Nakamoto and introduced the term chain of blocks. This term over the years has now evolved into the word blockchain.

In this chapter, first the theoretical foundations of distributed systems are described, then the precursors of bitcoin (with which blockchain technology was introduced) such as e-cash and hashcash, and then finally the blockchain technology is introduced. This is a logical way of understanding blockchain technology because the roots of blockchain are in distributed systems.

## Distributed systems

Understanding distributed systems is essential in order to understand blockchain because basically blockchain at its core is a distributed system. More precisely it is a decentralized distributed system.

Distributed systems are a computing paradigm whereby two or more nodes work with each other in a coordinated fashion in order to achieve a common outcome and it's modeled in such a way that end users see it as a single logical platform.

A node can be defined as an individual player in a distributed system. All nodes are capable of sending and receiving messages to and from each other. Nodes can be honest, faulty, or malicious and have their own memory and processor. A node that can exhibit arbitrary behavior is also known as a Byzantine node. This arbitrary behavior can be intentionally malicious, which is detrimental to the operation of the network. Generally, any unexpected behavior of a node on the network can be categorized as Byzantine. This term arbitrarily encompasses any behavior that is unexpected or malicious:

![](img/62ab880d2302f959d34db648cb0bec5e_35_0.png)

Design of a distributed system; N4 is a Byzantine node, L2 is broken or a slow network link

The main challenge in distributed system design is coordination between nodes and fault tolerance. Even if some of the nodes become faulty or network links break, the distributed system should tolerate this and should continue to work flawlessly in order to achieve the desired result. This has been an area of active research for many years and several algorithms and mechanisms have been proposed to overcome these issues.

Distributed systems are so challenging to design that a theorem known as the CAP theorem has been proved and states that a distributed system cannot have all much desired properties simultaneously. In the next section, a basic introduction to the CAP theorem will be provided.

## CAP theorem

This is also known as Brewer's theorem, introduced originally by *Eric Brewer* as a conjecture in 1998; in 2002 it was proved as a theorem by *Seth Gilbert* and *Nancy Lynch*.

The theorem states that any distributed system cannot have Consistency, Availability, and Partition tolerance simultaneously:

- **Consistency** is a property that ensures that all nodes in a distributed system have a single latest copy of data
- **Availability** means that the system is up, accessible for use, and is accepting incoming requests and responding with data without any failures as and when required
- **Partition tolerance** ensures that if a group of nodes fails the distributed system still continues to operate correctly

It has been proven that a distributed system cannot have all the afore mentioned three properties at the same time. This is strange because somehow blockchain manages to achieve all these properties, or does it really? This will be explained later in the chapter where the CAP theorem in the context of blockchain is discussed.

In order to achieve fault tolerance, replication is used. This is a common and widely used method to achieve fault tolerance. Consistency is achieved using consensus algorithms to ensure that all nodes have the same copy of data. This is also called **state machine replication**. Blockchain is basically a method to achieve state machine replication.

In general there are two types of fault that a node can experience: where a faulty node has simply crashed and where the faulty node can exhibit malicious or inconsistent behavior arbitrarily. This is the type which is difficult to deal with since it can cause confusion due to misleading information.
