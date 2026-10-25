
```solidity
pragma solidity ^0.4.0;

contract dummy
{
    uint32 value = 10;
}
```

```text
//Optimizer on
PUSH1 0x60
PUSH1 0x40
MSTORE
PUSH1 0x0
DUP1
SLOAD
PUSH4 0xffffffff
NOT
AND
PUSH1 0xa
OR
SWAP1
SSTORE
CALLVALUE
PUSH1 0x0
JUMPI
```

```text
//Optimizer off
PUSH1 0x60
PUSH1 0x40
MSTORE
PUSH1 0xa
PUSH1 0x0
PUSH1 0x100
EXP
DUP2
SLOAD
DUP2
PUSH4 0xffffffff
MUL
NOT
AND
SWAP1
DUP4
PUSH4 0xffffffff
AND
MUL
OR
SWAP1
SSTORE
POP
```

![](img/8c91673d39176d2e910ac52c4bc35d8b_213_0.png)

Figure 6.9: Opcode generation with and without optimizer

## Parity hack demystified

Parity (https://parity.io/) is a UK-based, venture capitalist-funded, tech start-up company. They provide an Ethereum client, also called Parity, which has an extensive Ethereum wallet and DApp environment for contract deployment. The entire application is written and compiled in the RUST language. The wallets are multi-sig. Multi-sig wallets, in simple words, are wallets that require at least two separate agreements to spend a token from the wallet. These wallets are supposed to be more secure than normal single-sig wallets.

Unfortunately, on July 19, 2017, three multi-sig wallets were exploited from a total of 596 wallets with similar vulnerabilities. Figure 6.10 shows these compromised accounts in Etherscan, the public Ethereum block explorer. When the Parity team got alerted of this hack, they tackled this situation by forming a "White Hat Group" and used the same vulnerability to siphon all the tokens from the remaining 593 wallets into a safe repository so that the hacker could not lay their hands on these wallets. Even then, the damage done by exploiting the three wallets was around 83,017 ETH (~ $30 million on July 2017):

![](img/8c91673d39176d2e910ac52c4bc35d8b_215_0.png)

Figure 6.10: Parity Multi-Sig Hacker account on Etherscan (https://goo.gl/CZCEji)

To become a professional smart contract developer, especially in solidity, it is very important to learn from the previous mistakes of senior developers. So let us walk through the solidity code snippet of the Parity wallet to understand the vulnerability that led to this hack. We are going to learn what actually happened in the source code, step-by-step, and at the end we will see how the Parity team fixed this issue. We will refer to the original source file present in the GitHub archives and take a look at the fixes that were made in the code in order to prevent such hacks in the future. And we will study that by looking at the history of GitHub commits to this project. Sounds like a post-mortem analysis from a Sherlock Holmes' crime scene, doesn't it? Bring on the magnifying glass, my dear Watson!!

First of all, we need to understand how this hack happened because this hack was not a flaw in the Ethereum blockchain or Ethereum virtual machine. This hack did not happen because Ethereum itself had some technical loophole or vulnerability. Neither was the hack due to a flaw in the solidity language. The Parity client did not have any vulnerabilities either. (Now you might be thinking, "Then who let the dogs out!? Who?

Who? Who?!’). My dear Watson, the flaw was actually in the piece of source code the Parity team gave to the users who wanted to create their own multi-sig wallet smart contracts. Being open source software, they are bound to give such source code to anyone under the creative commons license of the open source coding practice.

For example, I want to use the Parity client but I want to use my own multi-sig wallet. Then, the Parity team would give me a snippet or rather a template of source code with some pre-defined function calls and library calls already written inside the code template in which I can code my own logic of multi-sig functionality, yet remain compatible to interact with the Parity client over the Ethereum blockchain to transfer and store ethers. The flaw was in that snippet of source code. A villainous hacker could manipulate it to exploit the main Parity client deployed over the live Ethereum blockchain.

For instance, imagine you have installed CCTV cameras in each of your rooms for extra security. You lock every door and gate, and keep the duplicate keys under your favorite flowerpot in case of an emergency. Somehow, you totally forget to switch on privacy mode for your YouTube account, and all your home-locking activity is getting live fed into your normally "private" YouTube channel, which you think is still secure. Now, if a random thief, living nearby, was watching some random videos on YouTube and accidentally stumbled on your live feed channel, he will know where you keep the extra pair of keys. And it is just a matter of time; the thief will enter your home without even breaking or raising any alarms, and steal your valuables.

At this stage, you might be thinking that this is going to be a complex code debugging operation, with many senior developers such as Gavin Wood himself involved in fixing the code, even though he had not developed it. I also thought the same when the news hit the media channels. But we will see in a short while, how simple the flaw was and how easily such flaws can get overlooked even after many rounds of peer review.

Let us now get inside the code logic. Firstly, we will locate the actual code in GitHub. The path of the code (also depicted in Figure 6.11) is:

- https://github.com/mayukhdifferent/parity/blob/master/js/src/contracts/snippets/enhanced-wallet.sol

![](img/8c91673d39176d2e910ac52c4bc35d8b_217_0.png)

Figure 6.11: enhanced-wallet.sol at Parity GitHub archives

Here, you will notice that the author is Gavin Wood himself. Actually, this is the post-hack enhanced version, where the crucial bug fix was provided by Gavin Wood himself, as shown in Figure 6.11. The funny thing about viral media is that even though it spreads catastrophic news like wild fire, it also suppresses many parts of credible news that also deserves applause. The strategy and fix employed by the Parity team to stop the Parity hack midway actually saved 593 equally vulnerable wallets valued at around $179 million, approximately six times greater than the $30 million loss. Gavin Wood and the entire Parity team deserves applause for this on-the-feet thinking and risk mitigation strategy. It is very easy to act clever and wise once a theft has happened, but it takes real presence of mind to stop a heist from happening mid-way, which the team of developers at Parity has shown.

Let us now study the enhanced-wallet.sol file. This wallet code consists of four contracts, namely WalletEvents, WalletAbi, WalletLibrary, and Wallet. Each contract consists of several functions. For ease of understanding, a simpler

version of the wallet has been shown in Figure 6.12, which concentrates only on the bare minimum code snippet required to understand the vulnerability.

Before we start talking about the workings of this simplified contract, we need to understand what libraries are. When we're deploying our smart contracts on the Ethereum blockchain to build a decentralized application, we'll need to use certain functionalities that many other DApps will have used as well. Instead of coding these common functionalities from scratch, we bundle up such functionalities into a library. Now, this bundled-up library package can not only be used by me but also by several other developers who want to have this common set of functionalities in their decentralized applications. In this way, we are creating reusable coding assets.

When we develop multi-sig wallets for Parity, our code basically uses a helper library. Figure 6.12 has one such helper library named `WalletLibrary`. This has a function called `initWallet`. So, one thing this library contract could do is initialize a wallet. If we are building our own wallet contract code, we can call this library function from our contract to initialize the wallet.

Also, we can check whether an address is the owner of a wallet by calling the `isOwner` function instead of implementing it in our wallet contract. So, the library contract is basically a bundled product of standard helper functions:
