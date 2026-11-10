
![](img/d7c3f2018603810fae83e1a6fd98749e_0_0.png)

Community Experience Distilled

## Getting Started with Eclipse Juno

A fast paced tutorial to get you up and running with Eclipse Juno IDE

Rodrigo Fraxino Araujo
Vinicius H. S. Durelli
Rafael Medeiros Teixeira

[PACKT] PUBLISHING open source* community experience distilled

## Getting Started with
Eclipse Juno

A fast paced tutorial to get you up and running
with Eclipse Juno IDE

Rodrigo Fraxino Araujo
Vinicius H. S. Durelli
Rafael M. Teixeira

![](img/d7c3f2018603810fae83e1a6fd98749e_1_0.png)

BIRMINGHAM - MUMBAI

## Getting Started with Eclipse Juno

Copyright © 2013 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: July 2013

Production Reference: 1100713

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-78216-094-6

www.packtpub.com

Cover Image by Siddhart Ravishankar (sidd.ravishankar@gmail.com)

## Credits

**Authors**
Rodrigo Fraxino Araujo
Vinicius H. S. Durelli
Rafael M. Teixeira

**Copy Editors**
Insiya Morbiwala
Aditya Nair

**Project Coordinator**
Anugya Khurana

**Reviewers**
Francisco Osorio Gálvez
Peter Rice

**Proofreader**
Clyde Jenkins

**Acquisition Editors**
Amarabha Banerjee
Rukhsana Khambatta

**Indexer**
Tejal R. Soni

**Lead Technical Editor**
Ankita Shashi

**Production Coordinator**
Conidon Miranda

**Technical Editors**
Sharvari H. Baet
Athira Lalji
Veena Pagare

**Cover Work**
Conidon Miranda

## About the Authors

**Rodrigo Fraxino Araujo** is a Ph.D. candidate in Computer Science at the University of São Paulo, Brazil. He has spent a year as a visiting scholar at the Institut National de Recherche en Informatique et Automatique, Rocquencourt, France. He is also a software engineer and since 2011, is working at IBM.

> I would specially like to thank Carolina, who gave me the strength to work on this book.

**Vinicius H. S. Durelli** is a Ph.D. candidate in Computer Science at the University of São Paulo, Brazil. As part of his doctoral studies, from 2011 to 2012, he was a visiting scholar at the George Mason University, Virginia, USA. He received his M.S. in Computer Science from the Federal University of São Carlos in 2008. He has been a Sun Certified Java Programmer since 2006. Also, he has been using Eclipse since circa 2004, which makes him feel old. When he is not writing or programming, Vinicius enjoys playing video games (especially Mario and Zelda games) and practicing Brazilian Jiu Jitsu.

> I would like to express my deepest gratitude to my friend Simone Borges for placing food in front of me in order to keep me going while I was trying to hit deadlines related to this book and my ongoing Ph.D. studies.

**Rafael M. Teixeira** currently works as a Software Engineer at IBM Linux Technology Center, developing code for Eclipse open source projects. He's currently taking a MSc program in University of São Paulo, where he also received his Computer Engineering degree. Rafael's favorite hobby is running, but also enjoys some occasional video gaming.

> I would like to thank my family for their caring and support, which made everything possible. I would also like to thank Bianca Teixeira, whose motivating and sweet words have kept me going for the last 7 years and will hopefully do so for many others. I dedicate this work to the memory of Luis Fernando Teixeira.

## About the Reviewers

**Francisco Osorio Gálvez** is a senior developer and technical leader working at Citigroup, and previously at IBM. He works designing, developing, and leading large Java enterprise systems for the banking industry. He enjoys working on different technologies just for fun. You can contact him at `fosoriog@gmail.com`.

> I would like to thank my wife Fabiola for the time I stole from her. Weekends were sacrificed in the review of this book. I'll make it up to you, I promise. ;)

**Peter Rice** is Professor Emeritus of Mathematics from the University of Georgia, USA. He began a career in the IT industry after retirement, which led him to becoming a certified Microsoft trainer, an IBM trainer, and to do extensive work as an independent trainer working from his own company, PM Rice Consulting, Inc. In addition, he has created and worked as principal architect on a number of commercial programs. He also works in a number of languages.

## www.PacktPub.com

## Support files, eBooks, discount offers and more

You might want to visit www.PacktPub.com for support files and downloads related to your book.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/d7c3f2018603810fae83e1a6fd98749e_7_0.png)

http://PacktLib.PacktPub.com

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can access, read and search across Packt's entire library of books.

## Why Subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print and bookmark content
- On demand and accessible via web browser

## Free Access for Packt account holders

If you have an account with Packt at www.PacktPub.com, you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

## Table of Contents

Preface 1
Chapter 1: Introduction 9
  How to use this book 9
  Downloading and installing Eclipse 10
  Eclipse Juno – new and noteworthy 11
  Summary 18
Chapter 2: Java Development 19
  Creating a Java project 20
    Creating a Java class 21
    Creating working sets 21
  Importing a Java project 22
    Importing a project from Version Control Servers 23
  Introducing Java views 23
    The Package Explorer view 23
    The Java Editor view 24
      Compiling errors and warnings annotations 25
      Content assist 25
      Code navigation 26
      Quick fix 27
      Customizing the editor 29
    The Problems view 30
      Customizing the Problems view 30
    The Outline view 31
    The Type Hierarchy view 32
    The Call Hierarchy view 32
    Organizing imports 33
    Save actions 33
    Enforcing Coding Style with Formatter 34

- **Generating code** 34
- Generating getters and setters 34
- Generating constructors 35
- Generating the hashCode() and equals() methods 36
- Generating the toString() method 36
- Generating method comments 36
- Editing code and comment templates 37
- **Refactoring** 38
- Renaming variables 38
- Modifying a method's signature 39
- **Building and running the project** 39
- Creating a launch configuration 40
- **Managing the project build path** 41
- **Summary** 42
- **Chapter 3: Unit Testing with JUnit and Debugging** 43
- **Testing with JUnit – getting started** 44
- Setting up JUnit 46
- Testing with JUnit 49
- Implementing the test methods generated by Eclipse 53
- An overview of the Eclipse JUnit Runner view 57
- Creating test cases 58
- **Debugging when problems arise** 67
- **Summary** 72
- **Chapter 4: Version Control Systems** 73
- **CVS** 74
- Repository 74
- Checking out a project 76
- Synchronizing 76
- Branches 76
- **SVN** 77
- **Git** 80
- **Summary** 86
- **Chapter 5: SWT** 87
- **Getting started** 88
- Setting up 88
- Widgets 90
- Displays 92
- **Controls** 92
- The Label widgets 92
- The Text widgets 94
