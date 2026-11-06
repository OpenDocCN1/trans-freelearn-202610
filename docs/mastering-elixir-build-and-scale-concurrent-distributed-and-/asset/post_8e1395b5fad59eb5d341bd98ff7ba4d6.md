
## Mastering

## Elixir

Build and scale concurrent, distributed, and fault-tolerant applications

![](img/c636f4cfb07f25f1db535995e50706ed_0_0.png)

Packt
www.packt.com

By André Albuquerque and Daniel Caixinha

## Mastering Elixir

Build and scale concurrent, distributed, and fault-tolerant applications

André Albuquerque
Daniel Caixinha

![](img/c636f4cfb07f25f1db535995e50706ed_1_0.png)

BIRMINGHAM - MUMBAI

## Mastering Elixir

Copyright © 2018 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Commissioning Editor:** Richa Tripathi
**Acquisition Editor:** Sandeep Mishra
**Content Development Editor:** Anugraha Arunagiri
**Technical Editor:** Jash Bavishi
**Copy Editor:** Mukti kant Garimella
**Project Coordinator:** Ulhas Kambali
**Proofreader:** Safis Editing
**Indexer:** Pratik Shirodkar
**Graphics:** Tania Dutta
**Production Coordinator:** Shantanu Zagade

First published: July 2018

Production reference: 1280718

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78847-267-8

www.packtpub.com

To Ana and José

–André Albuquerque

To Raquel and Gabriel

–Daniel Caixinha

![](img/c636f4cfb07f25f1db535995e50706ed_4_0.png)

mapt.io

Mapt is an online digital library that gives you full access to over 5,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

## Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Mapt is fully searchable
- Copy and paste, print, and bookmark content

## PacktPub.com

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

## Contributors

## About the authors

**André Albuquerque** is a software engineer at Onfido, after working in the banking industry for seven years. He has a master's degree from Instituto Superior Técnico in distributed systems and software engineering, and, during his banking detour, he obtained a master's degree in economics. He is currently developing Onfido's microservices using Elixir and Ruby, learning every day about how applications can score and scale if we apply the correct tools and sound coding practices from the get-go. In his time off, he loves to build his own keyboards, play basketball, and spend time with his wife and son.

> *Thank you, Caixinha, for being crazy enough to embark on this journey with me, and thank you Paulo, for being the kind of mentor that I could only hope to find one day. To all my Onfido colleagues, it’s a pleasure to grow with you everyday.*

> *This book wouldn’t have been possible without the love and support of my family and friends.*

> *Ana and José, I am blessed to have you in my life.*

**Daniel Caixinha** is a software engineer at Onfido, where he is using Elixir to build resilient systems that can also handle the high growth of the business. After graduating from Instituto Superior Técnico, he joined the startup world, mainly using Ruby, but also got the chance to play around with Elixir. Upon joining Onfido, he got the chance to take Elixir more seriously, which made him fall in love with functional programming in general, and Elixir in particular. Besides building Elixir applications, he is fostering the use of Elixir, being also a member of the Lisbon Elixir meetup.

> *First and foremost, this book is dedicated to my soulmate, Raquel, and my son, Gabriel. Without your love, this book wouldn’t have been possible. I would also like to thank Onfido in general, but two individuals in particular: André, for being in the trenches with me, and Paulo, who fooled me well into believing I could write a book. Last, but definitely not least, I want to thank my parents for always supporting me to be better.*

## About the reviewer

Paulo A. Pereira is the Lisbon Engineering Director at Onfido. Enthusiastic about choosing the right tool for the job and combining different languages and paradigms, he is currently exploring functional programming, mainly via Elixir. Paulo has also authored Elixir Cookbook and was one of the technical reviewers for Learning Elixir.

## Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit `authors.packtpub.com` and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

## Table of Contents

- Preface 1
- Chapter 1: Preparing for the Journey Ahead 7
  - Why functional programming? 8
  - Elixir and Erlang 9
  - Elixir's data types 9
    - Integers 10
    - Floats 11
    - Atoms 12
    - Booleans 12
    - Tuples 14
    - Lists 15
    - Maps 16
    - Binaries 17
    - Strings and charlists 17
    - Other types 19
      - Keyword lists 19
      - Ranges 20
      - MapSets 20
  - Pattern matching 22
    - Pattern matching on tuples 23
    - Pattern matching on lists 24
    - Pattern matching on maps 26
    - Pattern matching on binaries and strings 26
  - Functions and Modules 27
    - Anonymous functions 28
    - Modules and Named Functions 30
    - Module attributes, directives, and uses 36
  - Working with collections 37
    - Looping through recursion 38
    - Eager processing with the Enum module 40
      - Comprehensions 41
    - Lazy processing with the stream module 42
  - Control flow 44
    - if and unless 44
    - cond 44
    - case 45
    - with 45
    - Exceptions 47
  - Typespecs and behaviours 49
  - Typespecs 49
  - Dialyzer 50
  - Behaviours 51
  - Protocols 53
  - Structs 55
  - Bringing structs and protocols together 57
  - Tooling and ecosystems 58
  - IEx 59
  - Mix 61
  - ExUnit 61
  - Erlang interoperability 61
  - Summary 63
  - Chapter 2: Innards of an Elixir Project 65
  - Elixir applications 66
  - Elixir project structure 70
  - Project dependencies 72
  - Umbrella projects 74
  - ElixirDrip – our showcase application 83
  - Using behaviours 85
  - Viewing cross references with xref 88
  - Adopting a consistent coding style 92
  - Summary 98
  - Chapter 3: Processes – The Bedrock of Concurrency and Fault Tolerance 99
  - Inside the BEAM 100
  - Working with processes 102
  - Creating processes 103
  - Message passing between processes 104
  - Building a cache worker 109
  - Detecting errors by linking and monitoring processes 116
  - Using links 118
  - Using monitors 121
  - Recovering from errors with supervisors and supervision trees 122
  - Defining and starting a supervisor 123
  - Streamlined child specs 127
  - Starting children at runtime using dynamic supervisors 129
  - Minimizing error effects with supervision trees 133
  - Summary 137
  - Chapter 4: Powered by Erlang/OTP 139
  - GenServer 140
  - GenServer at your service 141
  - Implementing a CacheWorker with GenServer 144
  - Agent 150
