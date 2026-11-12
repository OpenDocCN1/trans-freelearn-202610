
Packt

![](img/e6cf5550804ccae9d1859270da8688f5_0_0.png)

1ST EDITION

## PHP Web Development with Laminas

Build a fully secured and functional e-commerce application with PHP using the Laminas framework

## FLÁVIO GOMES DA SILVA LISBOA

Foreword by Enrico Zimuel, Core Committee member of PHP-FIG

## PHP Web Development with Laminas

Build a fully secured and functional e-commerce application with PHP using the Laminas framework

Flávio Gomes da Silva Lisboa

![](img/e6cf5550804ccae9d1859270da8688f5_1_0.png)

BIRMINGHAM—MUMBAI

## PHP Web Development with Laminas

Copyright © 2022 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

- **Group Product Manager**: Pavan Ramchandani
- **Publishing Product Manager**: Kushal Dave
- **Senior Editor**: Mark D'Souza
- **Senior Content Development Editor**: Rakhi Patel
- **Technical Editor**: Simran Udasi
- **Copy Editor**: Safis Editing
- **Project Coordinator**: Sonam Pandey
- **Proofreader**: Safis Editing
- **Indexer**: Sejal Dsilva
- **Production Designer**: Nilesh Mohite
- **Marketing Coordinators**: Anamika Singh and Marylou De Mello

First published: November 2022

Production reference: 1201022

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80324-536-2

www.packt.com

To my wife, Maria, a warrior, the love of my life, who beat cancer. To my daughter, Koriander, a superheroine, who has empathy as one of her superpowers. To the memory of my mother, Cleonisse, who taught me how to read and let me travel the world in books.

– Flávio Lisboa

## Foreword

_PHP Web Development with Laminas_ by Flávio Gomes da Silva Lisboa is a very good book if you are interested in web development with PHP. The author shows how to implement, using a step-by-step approach, a complete e-commerce application with the Laminas open source project (formerly Zend Framework). I really liked the approach of the author, introducing not only the usage of the framework but also many best practices such as test-driven development, behavior-driven development, principles of Agile development, and more. This is definitely a book to have if you want to build professional web applications in PHP.

> _Enrico Zimuel, principal software engineer at Elastic Core Committee, member of the PHP Framework Interop Group, formerly Zend Framework core developer_

## Contributors

## About the author

**Flávio Gomes da Silva Lisboa** is a development analyst in SERPRO involved in several projects for the Brazilian government, an instructor of development software courses, a teacher of development software disciplines, and a member of the Brazilian PHP community. He also was a programmer of desktop applications for restaurants and industrial kitchens and a system analyst on the international board of the Bank of Brazil. Sometimes, he writes sci-fi romances such as *The One: The Solitude and the Harmony* and *Pandino, the Emperor*. You can read more about him and his adventures at `https://fgsl.eti.br`.

I want to thank the people who have been close to me and supported me, especially my wife Maria and my daughter Koriander. They are the reason for which I want to progress. This text is readable thanks to the uncanny work of Divya Selvaraj and Rakhi Patel. I also need to thank Er Galvão and Matt Fletcher for their accurate technical reviewing. I can't forget Eduardo Bona and Anderson Burnes, who were course coordinators when I taught Zend Framework, now Laminas, to several classes of IT college students. It is necessary to also thank Marcos Melo, Walter Zapalowski, Guilherme Funchal, and Antonio Tiboni, fellows in SERPRO, who helped me to work on an international project that used Zend Framework. Finally, I need to mention the years of partnership with Anderson de Paula in training PHP developers. Of course, this book wouldn't be possible without the wonderful support of the Packt Publishing team, which gave me an incredible experience of writing. Thank you very much to Kushal Dave, Mark D'Souza, Sonam Pandey, and every member of Packt Publishing who was a part of the building of this book.

## About the reviewer

Matt Fletcher is an experienced software engineer who has designed and developed PHP web applications for over 20 years. He is currently the development manager for SilverDoor Apartments, the leading booking agent for serviced apartments worldwide. He leads an international team of developers who use the Laminas framework and API Tools to deliver a range of enterprise tools. His work includes reviewing pull requests, planning system architectures, and delivering high-quality projects.

## Table of Contents

- Preface xiii

## Part 1: Technical Background

## 1 Introducing Laminas for PHP Applications 3

- Why do we need web frameworks? 3
- Continuous integration and continuous delivery 4
- The usefulness of a framework 6
- From Zend Framework to Laminas 6
- My introduction to Zend Framework 7

- Zend Framework – the reference architecture for PHP applications 8
- The differences between Zend Framework and Laminas 10
- The technical and social infrastructure of the Laminas community 10
- Summary 12

## 2 Setting Up the Environment for Our E-Commerce Application 13

- Technical requirements 14
- Installing Laminas 15
- Installing Laminas through an isolated component of Laminas 16
- Installing Laminas through an MVC skeleton application 16

- Configuring the IDE – Eclipse for PHP developers 18
- Integrating Laminas and Eclipse 21
- Managing MySQL 29
- Summary 34

## 3 Using Laminas as a Library with Test-Driven Development

- Technical requirements 37
- Understanding the software requirements 38
- Use case – managing school classes 38
- Implementing a structure for the use case 39
- Creating our first automated test 45
- Developing our use case with TDD 49
- Creating our SchoolClassTest test class 50
- Creating a test method in the SchoolClass class 53
- Creating the SchoolClass class 54
- Configuring autoloading of classes 55
- Configuring the autoloading of test classes 57
- Creating the config.php file and rerunning the SchoolClassTest test case 58
- Creating a new testInserting test method 60
- Creating an edition form edit.php 63
- Creating a SchoolClassTest->testUpdating test method 65
- Altering edit.php 68
- Altering save.php 70
- Creating the delete.php file 70
- Generating code coverage reports 71
- Summary 74

## 4 From Object-Relational Mapping to MVC Containers

- Technical requirements 75
- Using ORM 76
- Use case – managing students 78
- Implementing the use case 78
- Running the SchoolClassTest->testListing test 80
- Adjusting CRUD files 87
- Using Laminas as a container 92
- Creating the school3 project 92
- Creating the School module 93
- Ensuring the module has the right namespace 95
- Implementing the test methods 96
- Running the tests 97
- Summary 108

## Part 2: Creating an E-Commerce Application

## 5 Creating the Virtual Store Project

- Technical requirements 111
- Understanding the project requirements for the sample web application 112
