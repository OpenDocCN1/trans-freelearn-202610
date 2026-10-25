
## Definition and design

The smart contract was formally defined by Nick Szabo in his 1996 paper titled, *Smart Contracts: Building Blocks for Digital Markets*. He described it as follows:

> "A Smart contract is a set of promises, specified in digital form, including protocols within which the parties perform on these promises."

Yup, that's it. That's the definition. Duh! What's so exciting about it? Let us spice it up a little bit. If we closely observe the definition, we will see that it consists of four specific elements:

- **A set of promises:** These are basically business conditions. For example, Judy and Ashley are business partners who jointly own an auction company. A simple business condition could be as follows:
  - `IF` the auction sale results in a profit `THEN` split the profit amount in a 60:40 ratio
  - `ELSE IF` the auction sale results in breaking even `THEN` donate 100 GBP each to welfare fund
  - `ELSE IF` auction sale results in a loss `THEN` recover the loss amount in a 70:30 ratio

Such business conditions can be in the form of a contract signed by either the partners or implemented by a rule-based operation applied to their respective business accounts.

- **Specified in digital form**: A smart contract consists of human-readable lines of code, as well as the software that prescribes its conditions and outcomes converted to machine-level binary code of 1s and 0s. Contractual clauses or rule-based lookups are embedded as code within software.
- **Protocols**: A set of rules for how each party should process data in relation to a smart contract. Generally implemented using computer algorithms, these sets of rules are technology-enabled, rule-based operations that enable actions such as the release of payments.
- **Within which the parties perform**: This is the trickiest element of the smart contract definition. Once a smart contract is deployed on a blockchain, it becomes irrevocable. An encoded smart contract cannot typically be stopped from performing. Automated performance is the soul of a smart contract.

Yet code can be flawed, and if a contract inherently reaches an unmet condition, the contract can stop performing. This is why we need smart contract auditing before deploying it over a public or live blockchain. Before that, we need to know various design models of a smart contract.

From the point-of-view of a blockchain, a smart contract is a piece of code that sits within a "block". Once deployed, it becomes part of the consensus state of the network. The block acts as a software generated container that bundles the messages of a smart contract. Such messages can be inputs, outputs, or pointers to other computer code. Regardless of how a smart contract is designed, they involve code. But this give rises to the common misconception that a smart contract consists of just code.

In fact, there is a whole spectrum of smart contract design models, which is illustrated in Figure 5.2:

![](img/8c91673d39176d2e910ac52c4bc35d8b_171_0.png)

Figure 5.2: Smart contracts lie on a spectrum, Source: Chamber of Digital Commerce, Nov 2016

More variations are likely to emerge as we progress further in developing more smart contract-based DApps. Hence, it becomes really important to study the role of code, which makes up a sizable portion of any smart contract.

## Role of code in a smart contract

All smart contracts involve code and all code has bugs. If not, they acquire them eventually. Today's requirement can become tomorrow's bug. Before we stepped into the 21st century, the year format in many computing and database systems was denoted by YY instead of YYYY. That was a requirement to save on the extra bytes.

While approaching the end of the 1990s, programmers realized how messed up this was going to get because once we are past the year 2000, the year format YY will become ambiguous. Will 19-11-17 be known as the 19th of November, 1917 or the 19th of November, 2017? Some were over-smart, they had the luxury of YYYY format but the algorithm just rolled the date back to the initial 1900. History books recorded this as the bug of the millennium, aka the Y2K bug.

Although organizations worldwide checked, fixed, and upgraded their computer systems to address the anticipated problem, it is not known how many problems went unrecorded. The point is that code does not always perform as the parties which implement it intends. Messages get delayed, passphrases get spooled-out, and data can get corrupted during transmission. The liability implications of such incidents needs to be taken into account before a smart contract is up on the blockchain. Such events need to be part of the overall plan of the contract. Otherwise, it just becomes a risky contract, not a smart one.

So, the role of code just does not end at the auditing, but also takes into account the legal aspect of a contract as we progress in time. Once a smart contract is up in the blockchain, it is treated as law. But any sane person will argue that we can always amend laws later on. Why can't this be done for smart contracts? Of course it can, and it is done using oracles. We

oraclize a smart contract so that it can amend itself by taking in external feeds from reliable sources, as depicted in Figure 5.3, using a simple insurance contract. Such oracle engines feed in an authenticity proof along with the data feed, so that the feed is tamper-proof and independent of the third-party feed API, as well as the oraclize service engines over the blockchain protocol. A related analogy would be buying a certified diamond from a local jewelry store. If a diamond has an external certification of authenticity, such as GIA or AGSL, that specifies its 4Cs (carat, cut, clarity, and color) and that it is conflict-free. Its resale value becomes independent of the store we bought it in:

![](img/8c91673d39176d2e910ac52c4bc35d8b_173_0.png)

Figure 5.3: Oraclized insurance contract, Source: Digital Chamber of Commerce, Nov 2016

We will look more at the oracle aspect of a smart contract in the Optimization and audit section. Before that, we need to know about the basic anatomy of a smart contract; otherwise, we might not be able to get a clear mental image of the oraclize engine and how it fits into the grand scheme of things.

## Basic anatomy of a smart contract design

Blockchain technology has become the jet fuel for propelling the concept of the smart contract in the world of business transactions. Large corporate institutions are rapidly adopting this concept to remain ahead of their competitors. This has led to a call for an industry-wide initiative for formalizing the design framework of a smart contract. One of the big four auditing firms, Deloitte, teamed up with the advisory board of the Digital Chambers of Commerce, has come up with a six-point anatomy to formalize a smart contract design, which has been summarized in Figure 5.4:

![](img/8c91673d39176d2e910ac52c4bc35d8b_174_0.png)

Figure 5.4: Six-point anatomy of smart contract design, Source: Digital Chamber of Commerce, Nov 2016

## Smart contract optimization

In Chapter 2, Grokking Ethereum, we discussed the concept of ether and gas (recall the buffet and car-fuel analogy). Smart contracts that are under-optimized consume more gas than necessary. The creator and user of such smart contracts will always be over-charged while deploying and running them on the blockchain. Xiapu Luo et al, a group of blockchain researchers from the Hong Kong Polytechnic University and UEST China, have identified seven such gas-costly programming patterns that belong to two categories, as illustrated in Figure 5.5. The first category, useless code pattern, introduces additional gas cost due to the increase in bytecode size during the deployment and the runtime removal of bytecode. The second category involves expensive operations using a loop:

![](img/8c91673d39176d2e910ac52c4bc35d8b_175_0.png)

Figure 5.5: Gas-costly programming patterns

Figure 5.6 illustrates pseudo-code representing these entirely under-optimized code patterns. Ideally, this needs to be depicted as bytecode but
