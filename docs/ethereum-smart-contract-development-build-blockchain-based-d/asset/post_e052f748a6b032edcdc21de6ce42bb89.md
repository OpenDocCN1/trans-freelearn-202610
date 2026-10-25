
Mayukh Mukhopadhyay

# Ethereum
Smart Contract
Development

Build blockchain-based decentralized applications using solidity

Packt

# Ethereum Smart Contract Development

Build blockchain-based decentralized applications
using solidity

Mayukh Mukhopadhyay

Packt

BIRMINGHAM - MUMBAI

# Ethereum Smart Contract Development

Copyright © 2018 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Commissioning Editor: Sunith Shetty
Acquisition Editor: Chandan Kumar
Content Development Editor: Amrita Noronha
Technical Editor: Sayali Thanekar
Copy Editor: Safis Editing, Vikrant Phadke
Project Coordinator: Shweta H Birwatkar
Proofreader: Safis Editing
Indexer: Pratik Shirodkar
Graphics: Jisha Chirayil
Production Coordinator: Aparna Bhagat

First published: February 2018

Production reference: 1210218

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78847-304-0

www.packtpub.com

Dedicated to my wife, **Mrittika**, for loaning me her ACER laptop to
complete this book.

And my daughter, **Abriti**, for keeping her mom super busy while
I was crashing it by mining ethers.

# Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Mapt is fully searchable
- Copy and paste, print, and bookmark content

# PacktPub.com

Did you know that Packt offers eBook versions of every book published, with PDF and Epub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

# Contributors

# About the author

**Mayukh Mukhopadhyay** started his career as a BI developer. After the 2008-09 financial crisis, he was at Tata Consultancy Services for one of their Fortune 500 clients in the telecom sector. Holding a master's in software engineering from Jadavpur University, he is presently working as a data insight developer, where he focuses on applying data science and machine learning to raw telecom equipment logs to generate business insights. He has a varied list of academic interests, ranging from audio signal processing, structural bioinformatics, and bio-inspired algorithms to consciousness engineering. Apart from being an Oracle Certified Specialist, he is a Certified Bitcoin Professional, recognized by C4 (Crypto Currency Certification Consortium). He tries to apply blockchain as a technology to different business domains.

*To my guide @JU Dr Parama Bhaumik—your lectures on distributed systems were my main inspiration while writing this book.*

*To my professional network connections Ivan Liljeqvist, Dug Campbell, Mahesh Murthy, Kaushik Sathupadi, Ravinder Deol, Narayan Prusty, and Thomas Wiesner—thanks for your blogs and vlogs. This book is as much your creation as it is mine.*

# About the reviewer

**Daniel Kraft** studied applied mathematics and theoretical physics in Graz, Austria, where he obtained his PhD from the University of Graz in 2015. After that, he started as a software engineer in Zurich, Switzerland. He has been very interested in Bitcoin and cryptocurrencies since 2011 and involved in Bitcoin development since 2013. Since 2014, he has been the main developer for Namecoin and Huntercoin, and has successfully reimplemented both on top of the modern Bitcoin Core code base. Wherever possible, Daniel also contributed improvements to the upstream Bitcoin Core. He has published multiple research articles in peer-reviewed journals, two of them directly related to cryptocurrency.

# Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit authors.packtpub.com and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

# Preface

If you are reading this line, I want to congratulate you because you have already overcome the most difficult hurdle in the pursuit of the understanding blockchain, and specifically, Ethereum smart contracts. This hurdle is the overwhelming hype surrounding this promising yet premature technology and trying to know what is really going on under the hood.

I am a software developer, and through my book, which you are viewing in your electronic device or physically holding in your hand, we will together embark on a fascinating journey through this enigmatic and revolutionary technology.

The chapters in this book have been arranged in an incremental fashion. We start with a gentle introduction to blockchain using the familiar bitcoin, and quickly dive into the world of Ethereum and the major players in its ecosystem.

Then we proceed to do some hands-on coding of a typical "Hello World" smart contract. We then take on the subject of decentralized autonomous organizations, decentralized applications, and smart contract optimization. We also analyze two famous multi-million-dollar hacks that recently occurred in the Ethereum community, along with the preventive measures employed to avoid them in future.

We then move on to the intricacies of the solidity programming language and web3.js library.

The final chapters mainly deal with the development of standardized tokens, the concept of initial coin offering, potential enterprise use cases of smart contracts, designing a decentralized micro-blogging platform, and surfing the dark web marketplace.

We conclude the book by providing primers on advanced topics with promising future prospects such as graph-based DLTs and quantum secured blockchains.

At the onset, I want to have a clear understanding between us. Even though I am the author of this book and it is my sole responsibility to present the facts as accurately as possible in this book, in no possible way do I consider myself as the sole authority on this subject.

As my reader, I want you to realize that I am just another overenthusiastic fellow learner who will try to assist you with your pursuit of knowledge by introducing the optimal amount of information required to kick-start your journey. And I will make you aware of the hurdles and pitfalls I faced along the way so that you can learn from my mistakes.

Enough said. Let's start our journey to blockchain and get our hands dirty with *Ethereum Smart Contract Development.*

Happy reading!

# What this book covers

Chapter 1, *Blockchain Basics*, will serve as a warm-up session about blockchain before we deep dive into Ethereum and smart contract development. To really appreciate blockchain, we must understand the two founding pillars on which blockchain as a technology is firmly grounded: distributed systems and cryptography. Once we have covered these two core concepts, we try to understand a blockchain from two different perspectives: as a software developer and as a trader of financial instruments. Then we probe into the internal logical architecture of a block in the blockchain, focusing the bitcoin block structure, and get a gentle introduction to the mining and forking process. We conclude the chapter by discussing how blockchain has evolved in recent years and clearly marking out its current position in the technological hype cycle.

Chapter 2, *Grokking Ethereum*, will help us to understand the meaning of decentralization and whether Ethereum is truly decentralized. We will also cover its core technological stack and get familiarized with various jargon, such as Mist, EVM, Swarm, Whisper, Ether, and Gas. We will briefly discuss the notion of a Turing-complete language. We'll then revisit forking, mining, and block architecture from the perspective of Ethereum. We'll end this chapter by getting a notion of Ethereum wallets and client interfaces, which will serve as a firm base for smart contract development.

Chapter 3, *The Hello World of Ethereum Smart Contract*, provides a hands-on guide to developing our first smart contract. As an unspoken tradition of a software developer, we start with a "Hello World" program. Then we try some basic arithmetic increment and decrement operations using a contract. We then learn how to code a loop inside a smart contract and how to raise an issue in GitHub. We end this chapter by creating our own private blockchain right from the genesis block, attach it with Geth, and use the Mist browser to deploy one of the smart contracts we studied in
