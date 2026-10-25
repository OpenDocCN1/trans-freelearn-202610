
and utilizes a newly designed trinary hash function called curl, which makes it immune to quantum computing threats to a certain extent, as we cannot use an address more than once after we have sent some Iota from it

## Iota GitHub analysis

If you go to GitHub and search for Iota, you will get to a page that links the repositories of Iota cryptocurrency. The GitHub page of Iota consists of three main repositories, namely wallet, **Iota reference implementation (IRI)**, and Iota JavaScript library. *Figure 11.9* depicts these repositories:

![](img/8c91673d39176d2e910ac52c4bc35d8b_373_0.png)

Figure 11.9: Iota GitHub (https://github.com/iotaledger)

Among these three repositories, IRI is the most interesting repository. So what is IRI? It is a reference implementation of the Iota protocol. Much like bitcoin, Iota is also a protocol operating on a DAG-based database called Tangle and not on a blockchain. Now, IRI represents a blueprint of Iota. So, when Iota developers made the reference implementation in Java, they are actually telling the world, "*this is how Iota functions work and using this reference implementation we can use any other Turing complete programming language to set up Iota clients or Iota nodes.*"

Now if we step back and think about it for a while, we will realize that using this reference implementation, we can even use Solidity, which is a Turing complete programming language, to develop an Iota client, node, or even a smart contract. The possibility is endless and quite exciting. Isn't it?

Next we have the Iota Wallet. It is written in JavaScript and it is an electron app. So what is electron? It is a tool that programmers can use to package a website as a native application. Basically, when we want to develop native applications for macOS, Windows, or Linux, one way to do it would be to code everything from scratch, which is time consuming and confusing as we have to maintain several different code bases. Instead, what we can do is write a website and package it as a Windows, macOS, or Linux application. Now, if we want to make changes or develop the application further, we just develop this website further and package it once again. Electron is gaining traction on many other cryptocurrency projects as well.

Finally, we have a JavaScript library. It provides coding tools and APIs for other developers. Suppose, as a developer, we want to create a website to interact with the Iota protocol. We can use their JavaScript libraries to invoke various Iota functionalities.

In the next section, we will see how to purchase and store Iota cryptocurrency.

### Purchase and storage of Iota

Presently, Iota cannot be purchased directly using fiat currency. It can be bought from crypto-exchanges such as Bitfinex or Binance using ethers or bitcoin.

The following steps need to be followed to buy Iota:

- 1. Create an account on coinbase.
- 2. Buy BTC or ETH in coinbase.
- 3. Open an account on Bitfinex or Binance.
- 4. Transfer some BTC or ETH to Bitfinex or Binance.
- 5. Buy Iota by placing an order bid using BTC or ETH. Iota is sold in millions called MIOTA.

Storage of Iota is pretty straightforward by nature. We can store the Iota in the Iota Wallet as provided in the Iota GitHub. Or we can use paper wallets as provided in the following link: https://arancauchi.github.io/IOTA-Paper-Wallet/.

To create a paper wallet, we need to pass the 81-digit code and generate a QR code, as shown in *Figure 11.10*. We can then take a printout of the wallet and store it in a safe place:

![](img/8c91673d39176d2e910ac52c4bc35d8b_376_0.png)

Figure 11.10: Paper wallet for Iota

From Tangle, beyond blockchain, we could end up in an exciting new phase of technological advancement. We will soon be experiencing machine-to-machine economies, which will be facilitated by Internet-of-Things appliances, and Iota might eventually rule the cryptocurrency ranks.

This brings us to the end of this book. But your journey has just began into this exciting world of smart contracts. Consult the wider community spread over YouTube channels, GitHub links, Reddit threads, and stack exchange forums. You now have the power to change the world using this technology in the midst of all the fear-uncertainty-doubt (FUD); all you need is the ability to create some good source code.

Exit 0

## Summary

We began this chapter with a quick introduction to design patterns and discussed common patterns, such as restricting access, factory patterns, token systems, registries, and voting systems that are used for smart contract development. Then we moved on to understand the concept of decentralized autonomous corporations and society, and how they are different from DAOs. We explored a few crucial proposals related to Ethereum after understanding the proposal life cycle. Select case studies were picked to understand the future implications of consortium blockchains. We wrapped up the chapter by briefly introducing the world of graph-based distributed ledgers with GitHub analysis of the Iota cryptocurrency, and an introduction to Tangle.

## Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

![](img/8c91673d39176d2e910ac52c4bc35d8b_378_0.png)

### Building Blockchain Projects

Narayan Prusty

ISBN: 978-1-78712-214-7

- Walk through the basics of the Blockchain technology
- Implement Blockchain’s technology and its features, and see what can be achieved using them
- Build DApps using Solidity and Web3.js
- Understand the geth command and cryptography
- Create Ethereum wallets
- Explore consortium blockchain

![](img/8c91673d39176d2e910ac52c4bc35d8b_379_0.png)

### Mastering Blockchain

Imran Bashir

ISBN: 978-1-78712-544-5

- Master the theoretical and technical foundations of blockchain technology
- Fully comprehend the concept of decentralization, its impact and relationship with blockchain technology
- Experience how cryptography is used to secure data with practical examples
- Grasp the inner workings of blockchain and relevant mechanisms behind Bitcoin and alternative cryptocurrencies
- Understand theoretical foundations of smart contracts
- Identify and examine applications of blockchain technology outside of currencies
- Investigate alternate blockchain solutions including Hyperledger, Corda, and many more
- Explore research topics and future scope of blockchain technology

## Leave a review - let other readers know what you think

Please share your thoughts on this book with others by leaving a review on the site that you bought it from. If you purchased the book from Amazon, please leave us an honest review on this book's Amazon page. This is vital so that other potential readers can see and use your unbiased opinion to make purchasing decisions; we can understand what our customers think about our products, and our authors can see your feedback on the title that they have worked with Packt to create. It will only take a few minutes of your time, but is valuable to other potential customers, our authors, and Packt. Thank you!
