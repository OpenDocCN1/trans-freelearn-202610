![](assets/b5f58d72-0138-4193-ab9e-a8684937a516.png)

![](assets/b5f58d72-0138-4193-ab9e-a8684937a516.png)

**Learning Azure Cosmos DB  
  
** 

 

 

 

 

A beginner's guide to creating scalable, globally distributed, and highly responsive applications using Cosmos DB 

 

 

 

 

 

 

 

 

 

 

 

Shahid Shaikh 

 

 

 

 

 

 

 

 

 

![](assets/2844f8a2-9710-4a84-beec-bc949706c999.png)

****BIRMINGHAM - MUMBAI**** 

# Learning Azure Cosmos DB

Copyright © 2018 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Commissioning Editor:** Sunith Shetty  
**Acquisition Editor:** Chandan Kumar  
**Content Development Editor:** Amrita Noronha  
**Technical Editor:** Nilesh Sawakhande  
**Copy Editor:** Safis Editing  
**Project Coordinator:** Shweta H Birwatkar  
**Proofreader:** Safis Editing  
**Indexer:** Aishwarya Gangawane  
**Graphics:** Tania Dutta  
**Production Coordinator: **Aparna Bhagat

First published: May 2018

Production reference: 1290518

Published by Packt Publishing Ltd.  
Livery Place  
35 Livery Street  
Birmingham  
B3 2PB, UK.

ISBN 978-1-78847-617-1

[www.packtpub.com](http://www.packtpub.com)

![](assets/99c429e5-da59-4ca4-8c96-48c4d192cc1f.jpg)[mapt.io](https://mapt.io/) 

Mapt is an online digital library that gives you full access to over 5,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

# Why subscribe?

*   Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
    
*   Improve your learning with Skill Plans built especially for you
    
*   Get a free eBook or video every month
    
*   Mapt is fully searchable
    
*   Copy and paste, print, and bookmark content
    

# PacktPub.com

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at [www.PacktPub.com](http://www.PacktPub.com) and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at `service@packtpub.com` for more details.

At [www.PacktPub.com](http://www.packtpub.com), you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

# Contributors

# About the author

**Shahid Shaikh** is an engineer, published author, and a blogger. He has written tons of tutorials on Node.js and various databases on his blog. He is a believer in open source and almost all his source code is on GitHub. He has also developed and launched a SaaS product called Spark to handle content sharing automation. Currently, he is building a next-generation email marketing tool called Hashmailer.

*I would like to acknowledge my parents for supporting me throughout the book-writing phase. I had to work on weekends and cancel lots of plans to deliver the chapters on time, and my family and friends canceled various things because of me and supported me while I was writing the book, which I am very thankful for.*

# About the reviewer

**Kasam Ahmed Shaikh** is a seasoned professional with a 'could be' attitude with 10 years of determined industry experience with one of the leading IT companies in Mumbai, India. He is a certified Azure architect, recognized as C# Corner MVP. He is also a speaker, and has written a book on Azure cognitive services. He is also the founder of DearAzure.net and Shaikh Academy.

*Very first, I would like to thank Almighty Allah, my family, and in particular, my better half for motivating me throughout the process. I am very thankful to Packt Publishing for believing and considering me for this opportunity.*

 

 

 

# Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit [authors.packtpub.com](http://authors.packtpub.com) and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

# Table of Contents

1.  [Title Page](#3c82ef13-572f-4d7d-8018-1c7870dd639f.xhtml)
2.  [Copyright and Credits](#edcb7632-2865-4ec1-acf7-d1a13e207809.xhtml) 
    1.  [Learning Azure Cosmos DB](#913f4c12-7d42-4d8d-937a-7512237f0d35.xhtml)
3.  [Packt Upsell](#af31b4d9-dd3f-4fd2-9d0a-04b98632b7bc.xhtml) 
    1.  [Why subscribe?](#6a107e52-9d8b-4f60-937e-de668a7379d2.xhtml)
    2.  [PacktPub.com](#90547174-f4b9-462e-951f-4336b8bccbe7.xhtml)
4.  [Contributors](#23bcf460-6981-4e0a-a5cb-99368397cd3f.xhtml) 
    1.  [About the author](#9f275e7f-df46-4545-964c-9d6e63c6f1b8.xhtml)
    2.  [About the reviewer](#4488d331-82c4-4841-9f2b-c1fa6a32f0c5.xhtml)
    3.  [Packt is searching for authors like you](#b94ba860-91ab-44ba-bf00-45bf135544f6.xhtml)
5.  [Preface](#6b5e934b-1812-42be-b262-69fe069c8269.xhtml) 
    1.  [Who this book is for](#9b7864a3-b299-4401-a71a-0168cc7ee313.xhtml)
    2.  [What this book covers](#a22c283c-f6ef-45a9-8e67-11033cca0953.xhtml)
    3.  [To get the most out of this book](#6e40524b-825e-49aa-a6f3-f72dd44399cf.xhtml) 
        1.  [Download the example code files](#b94e21bd-cfdc-452d-a403-4624ef0da98d.xhtml)
        2.  [Download the color images](#6bf7bd2b-0c6a-4e16-a5d1-d789b2e29762.xhtml)
        3.  [Conventions used](#cf50cf66-d64a-4d89-87dd-be3a2ed9a2c0.xhtml)
    4.  [Get in touch](#72f1c448-8d78-4922-a261-75078e747e54.xhtml) 
        1.  [Reviews](#5e87a91f-0672-4cea-a095-27a5ea04a95b.xhtml)
6.  [Introducing Microsoft Azure Cloud and Cosmos DB](#25826e74-4071-4c8f-af67-9f5185cd8e67.xhtml) 
    1.  [Microsoft Azure Cloud features](#43ab6d8c-96b8-4fb2-85f7-43d3ffdb1579.xhtml)
    2.  [Microsoft Azure as an SPI](#7eab4a4b-6cf0-4907-822a-f46ce8a0a555.xhtml) 
        1.  [Azure as PaaS](#b87b31f9-ac41-443a-b881-68d8411de11c.xhtml)
        2.  [Azure as IaaS](#91f2e807-ca8b-4f09-8dce-c4479cdab6e6.xhtml)
    3.  [Azure components](#f0437b2b-f47e-40e5-b8be-363626096dae.xhtml)
    4.  [Getting started with Azure](#b05277bb-f949-4ffd-bc9a-5b4db80cbcdf.xhtml) 
        1.  [Compute modules](#3c5fdeff-255c-404f-ba36-20840896d753.xhtml)
        2.  [Data management modules](#fd6a4a89-ebb3-4ee8-b5cf-6c3fa447a30a.xhtml)
        3.  [Networking module](#4bab3f94-5339-4f86-b4cf-8342fbda6c11.xhtml)
        4.  [Big data modules](#26ae0a8f-2de6-4095-9d97-b876ec842ad2.xhtml)
        5.  [Caching module](#f517d6d6-8843-4748-ae9e-eb3bcfa11bc2.xhtml)
        6.  [Messaging module](#17f39f6f-f6da-437a-8150-48da4314afe3.xhtml)
        7.  [Identity module](#e2ff2e10-cf1d-4eff-a449-db376876040f.xhtml)
        8.  [Mobile service module](#18bc91fa-c11f-427b-bb82-a8b642b4a8bc.xhtml)
        9.  [IoT modules](#af4df387-5f86-41f9-bd99-2eb4ba715c9a.xhtml)
    5.  [Azure Cosmos DB](#bef4c1ef-9233-4c71-bb8f-b7cdb8e208c7.xhtml) 
        1.  [Turnkey global distribution](#fd7c157f-10ca-4191-b659-1e0f41addc63.xhtml)
        2.  [Multilevel data models](#f1addbd2-14a0-4871-b72a-863104aadf7f.xhtml)
        3.  [Scale on demand](#eec1d19a-121f-4ad4-bd54-04341702b0e1.xhtml)
        4.  [High response with less latency](#3f25d432-9f2f-4710-98e2-d4effddc4501.xhtml)
        5.  [Consistency models](#45ffe688-9792-41c7-a432-675b80b98119.xhtml)
        6.  [Money-back guarantee and SLA](#8f83369d-5a7e-4205-a9ca-861b1edfb682.xhtml)
    6.  [Cosmos DB use cases](#8e28f279-90e3-4a37-ad73-29dffb7097e5.xhtml) 
        1.  [IoT](#ba662027-22e4-4df1-8990-2b972a9da3ab.xhtml)
        2.  [Retail and marketing](#da84248f-8e2a-4c60-8a03-7ae11ef00afb.xhtml)
        3.  [Gaming](#3056f899-d612-429e-89dc-2083caad4c71.xhtml)
        4.  [Web and mobile applications](#fee3022d-6a0b-42e3-86be-b03967871f7a.xhtml)
    7.  [Summary](#8eb97968-409e-4f84-9eb9-e72fb6b8ab9c.xhtml)
7.  [Cosmos DB Global Turnkey Distribution](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml) 
    1.  [Setting up an Azure subscription](#aedf4be7-508f-406b-ab7e-90143c8d342e.xhtml)
    2.  [Creating an Azure Cosmos DB database](#1d6c3b15-9cf9-45d4-824f-213c97a129c5.xhtml)
    3.  [Adding data in MongoDB](#80fcf96c-f01d-40ac-8005-2d5d17cfd890.xhtml)
    4.  [Distributing data using global turnkey distribution](#a4c8870a-150c-4a70-9fc7-beabb81a3dea.xhtml)
    5.  [Failover handling](#3685ba33-3ce7-488f-9aeb-7af555fbfa0a.xhtml) 
        1.  [Automatic failover](#678cd3f9-d587-46ae-ab3a-b0c58373616d.xhtml)
        2.  [Manual failover](#6a77574b-5a1c-4d42-a063-afb2c55a88e0.xhtml)
    6.  [Summary](#73dbbda5-ecac-4cb9-b0ef-bf9d573c271d.xhtml)
8.  [Cosmos DB Multi-model Databases](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml) 
    1.  [Cosmos DB data models](#915f4ab9-5e05-44b9-ad1a-5426eb8cf065.xhtml) 
        1.  [ARS](#1c2504fe-e3d4-4fb8-ac03-29666e6cc97e.xhtml)
        2.  [DocumentDB API](#f1a1f6f5-d7a2-4554-953d-5ffd6839a3f9.xhtml)
        3.  [The MongoDB data model](#4973940e-7d87-4dd2-b979-6d8b118a8fb2.xhtml)
        4.  [The tables data model](#bbbeb3ba-84b0-4c2d-95db-571340b80e86.xhtml)
        5.  [The graph (Gremlin) data model](#3f03a3a0-0838-4a22-9770-27391f347ce4.xhtml)
        6.  [Cassandra API](#967e0d63-b9cb-4090-bcb7-f4a56676684b.xhtml)
    2.  [The Cosmos DB consistency levels](#20a1872f-5fcb-4f8f-b11c-52a7ac54909d.xhtml)
    3.  [Summary](#8d4a5051-f311-4f3e-9fd1-82fced80e80f.xhtml)
9.  [Querying Cosmos DB](#f5161897-9696-4441-9e45-f2346e49978e.xhtml) 
    1.  [Performing CRUD operations in DocumentDB](#e62caec9-c2e6-4537-86ad-a2e9b4c6da10.xhtml) 
        1.  [DocumentDB query basics](#a4c28813-c66d-4a63-9107-0927ae6cdb34.xhtml)
        2.  [Creating data in Cosmos DB](#396a7413-039a-4273-a4db-34352d77b7ae.xhtml)
        3.  [Reading data from DocumentDB](#59e7b57b-329d-42b6-beec-1b5572f454bd.xhtml)
        4.  [Updating and deleting documents](#30bfbc62-bbc9-423a-a2f9-49432278a2fc.xhtml)
    2.  [Performing CRUD operations in MongoDB](#6e94fd8c-a3bc-4dc0-a41a-4841f9a69e36.xhtml)
    3.  [Performing CRUD operations in Graph DB](#e72d782d-ebc6-4ba3-aacd-47e6dbc5fc59.xhtml)
    4.  [Performing CRUD operations in tables (key value store)](#9b67ff45-6971-400a-8b7d-a6097498b11c.xhtml)
    5.  [Performing CRUD operations in Cassandra.](#341971a0-ec80-4210-827e-628c95310fa6.xhtml)
    6.  [Summary](#a4f7a913-da92-4a2b-a237-fa028f5b869a.xhtml)
10.  [Integrating Cosmos DB with Node.js](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml) 
     1.  [Node.js setup](#42979d43-7d9e-4da7-ad43-8288adef5b47.xhtml)
     2.  [Cosmos DB MongoDB setup](#4fdfd617-45d7-4e32-b7fd-97e2a1ff8493.xhtml)
     3.  [Connecting Cosmos DB with the Node.js app](#6a6efcd3-f585-44d0-9cea-8dcd60f99d29.xhtml)
     4.  [App data in Cosmos DB](#d270a256-c26f-42f6-a268-8084d52c5df5.xhtml)
     5.  [Integrating the DocumentDB API with Node.js](#3e631187-ab1a-4075-b327-6d850a23f996.xhtml)
     6.  [Integrating the Table API with Node.js](#200a4fa7-035f-4211-90ed-76aa4a2c18bf.xhtml)
     7.  [Integrating the Graph API with Node.js](#672f58cc-39c6-4018-913b-e9d988029956.xhtml)
     8.  [Summary](#de5edf23-eda5-440c-ae76-567fd69359ef.xhtml)
11.  [Building a High-Performance Link Shortening and Tracking System Using Cosmos DB](#5c360edf-dd5e-4d41-991a-11103f5b7705.xhtml) 
     1.  [Why do we need URL shortening?](#074df708-5335-4cae-8bb0-bf1ea576cc7a.xhtml)
     2.  [Requirements of the system](#6c1c540d-36c8-41de-83a1-f24830d59530.xhtml)
     3.  [Capacity estimation](#fda53534-ba00-403e-84e2-b3bcfb3371b2.xhtml)
     4.  [Database design](#cb841799-fbdb-4139-8ff8-73f04acd0ffc.xhtml)
     5.  [Encoding URL algorithms](#5c27d04e-5741-4fd0-839b-1620af6c62bd.xhtml)
     6.  [Node app development](#8ff1423a-941b-4cb4-aad4-4fd1c64f6bf0.xhtml)
     7.  [Security and permission](#9b9f71bf-d457-41be-88b5-d2c38bee83d6.xhtml)
     8.  [Summary](#6d828e4d-f52a-48e5-8b3a-633dc6063320.xhtml)
12.  [Building a Highly Available, Real-Time Email Tracking System using Cosmos](#59413440-6b69-4d90-a2c2-e28c2dd874fe.xhtml) 
     1.  [Is email tracking legal?](#0d7ccf72-cbdb-4352-ba4c-d527b687fe81.xhtml)
     2.  [System design and assumptions](#5a298b9c-71cd-4014-8cf5-fe4044b6b95f.xhtml)
     3.  [Choosing the database API in Cosmos DB](#05b1f557-f71c-4cc4-a5ed-0b475c340d5d.xhtml)
     4.  [Writing our email sender](#deb45ab0-e2ed-4923-8bdf-8a8de0035144.xhtml)
     5.  [Tracking emails and storing tracking data](#586ba5a7-ee66-4e37-8fdc-0085bd559c01.xhtml)
     6.  [Summary](#72447287-b7a2-43a2-a890-ff96c92d9c46.xhtml)
13.  [Cosmos DB Deployment and Management](#4af46b38-5c09-4a5e-a5f6-e37632e5f68e.xhtml) 
     1.  [Turnkey distribution](#26fdc2b0-4a10-4128-8eeb-9cd8d05a37e8.xhtml)
     2.  [Failover handling](#9fcc31ac-5f04-4ce2-8f3f-2821d7d080cd.xhtml) 
         1.  [Automatic failover](#288a6f93-9cd3-4d58-b616-83ce71120896.xhtml)
         2.  [Manual failover](#d950925d-b435-48c6-958e-75b4872100b7.xhtml)
     3.  [Database deployment](#ea6534d2-fd57-4455-82bd-85f8055fd2ec.xhtml)
     4.  [Backup and restore](#77f4a9bb-0693-47fc-88e1-b8287816c443.xhtml)
     5.  [Deploying a custom app over the Azure network](#78d4807b-efa4-4d54-8731-7da6d22b1c88.xhtml)
     6.  [Monitoring traffic and usage](#42716ada-f040-451c-a990-c5fdc8fffb59.xhtml)
     7.  [Database security](#e0d18112-00a0-4cf4-8e0a-dada38d8a3d5.xhtml)
     8.  [Summary](#d36feda2-34b7-43ef-b2ad-44d74f001097.xhtml)
14.  [Other Books You May Enjoy](#5e1a7576-f1ec-46a4-8f55-cd6a12b74dea.xhtml) 
     1.  [Leave a review - let other readers know what you think](#710d2881-c0ca-4d7e-a17a-594033beae3c.xhtml)

# Preface

Azure Cosmos DB is Microsoft's managed database solution for individuals and enterprise with the industry standard service-level agreement. Azure Cosmos DB provides multi-model database support that can scale across more than 30 regions across the world. Azure Cosmos DB provides a turn key distribution that enables the database admins to scale and replicate their database with a few clicks. 

This book covers the major concepts and features of Azure Cosmos DB. This book also covers the development of web applications using Cosmos DB and their integration using the software development kit provided by Azure Cosmos DB.

# Who this book is for

This book is intended for anyone who wants to get well versed with Microsoft's new NoSQL database called Azure Cosmos DB. You should have prior experience of dealing with any SQL or NoSQL databases.

# What this book covers

[Chapter 1](#25826e74-4071-4c8f-af67-9f5185cd8e67.xhtml), *Introducing Microsoft Azure Cloud and Cosmos DB*, aims to provide an introduction to the Microsoft Azure platform. The reader will get to know the various interfaces and modules provided by the Microsoft Azure cloud. Another part of this chapter provides an introduction to Cosmos DB, which is the main topic of the book.

[Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution*, covers one of the key features of Cosmos DB, which is its global turn key distribution system. You can replicate your database more than 30 regions scattered across the globe using a very simple interface.

[Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Databases*, provides the four (five after publishing state of this book) database models that you can pick from. This chapter covers these database models in detail.

[Chapter 4](#f5161897-9696-4441-9e45-f2346e49978e.xhtml), *Querying Cosmos DB*, covers the different types of queries required for multi-model databases. This chapter segregates these queries in the CRUD (Create, Read, Update, and Delete) way to explain it better.

[Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Cosmos DB with Node.js*, covers all integration with one of the popular backend runtime engines, Node.js. We have covered how to connect to the database models of Cosmos DB with Node.js.

[Chapter 6](#5c360edf-dd5e-4d41-991a-11103f5b7705.xhtml), *Building High-Performance Link Shortening and Tracking System Using Cosmos DB*, covers the development of a simple and high-performance link shortening and tracking system that's very similar to bit.ly using Cosmos DB and Node.js. The reader will learn and relate how they can build a complex system like this using a Cosmos DB-managed database solution.

[Chapter 7](#59413440-6b69-4d90-a2c2-e28c2dd874fe.xhtml), *Building Highly Available and Real-Time Email Tracking System Using Cosmos DB*, covers the development of a system that handles the email tracking that is open rates of the email by millions of users and stores them in the database for further analytics.

[Chapter 8](#4af46b38-5c09-4a5e-a5f6-e37632e5f68e.xhtml), *Cosmos DB Deployment and Management*, covers all of the deployment of your custom application and Cosmos DB database to the production environment.

# To get the most out of this book

Get the latest version of Node.js installed and learn MongoDB query basics to get the most out of this book.

# Download the example code files

You can download the example code files for this book from your account at [www.packtpub.com](http://www.packtpub.com). If you purchased this book elsewhere, you can visit [www.packtpub.com/support](http://www.packtpub.com/support) and register to have the files emailed directly to you.

You can download the code files by following these steps:

1.  Log in or register at [www.packtpub.com](http://www.packtpub.com/support).
2.  Select the SUPPORT tab.
3.  Click on Code Downloads & Errata.
4.  Enter the name of the book in the Search box and follow the onscreen instructions.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

*   WinRAR/7-Zip for Windows
*   Zipeg/iZip/UnRarX for Mac
*   7-Zip/PeaZip for Linux

The code bundle for the book is also hosted on GitHub at [https://github.com/PacktPublishing/Learning-Azure-Cosmos-DB](https://github.com/PacktPublishing/Learning-Azure-Cosmos-DB). We also have other code bundles from our rich catalog of books and videos available at **[https://github.com/PacktPublishing/](https://github.com/PacktPublishing/)**. Check them out!

# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: [http://www.packtpub.com/sites/default/files/downloads/LearningAzureCosmosDB\_ColorImages.pdf](http://www.packtpub.com/sites/default/files/downloads/LearningAzureCosmosDB_ColorImages.pdf).

# Conventions used

There are a number of text conventions used throughout this book.

`CodeInText`: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Let's perform an `UPDATE` and `DELETE` operation."

A block of code is set as follows:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
} 
```

Any command-line input or output is written as follows:

```
SELECT *  
FROM Families f  
WHERE f.id = "100" 
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Click on New Database and choose the Gremlin as API."

Warnings or important notes appear like this. Tips and tricks appear like this. 

# Get in touch

Feedback from our readers is always welcome.

**General feedback**: Email `feedback@packtpub.com` and mention the book title in the subject of your message. If you have questions about any aspect of this book, please email us at `questions@packtpub.com`.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit [www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata), selecting your book, clicking on the Errata Submission Form link, and entering the details.

**Piracy**: If you come across any illegal copies of our works in any form on the Internet, we would be grateful if you would provide us with the location address or website name. Please contact us at `copyright@packtpub.com` with a link to the material.

**If you are interested in becoming an author**: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit [authors.packtpub.com](http://authors.packtpub.com/).

# Reviews

Please leave a review. Once you have read and used this book, why not leave a review on the site that you purchased it from? Potential readers can then see and use your unbiased opinion to make purchase decisions, we at Packt can understand what you think about our products, and our authors can see your feedback on their book. Thank you!

For more information about Packt, please visit [packtpub.com](https://www.packtpub.com/).[](https://www.packtpub.com/)

# Introducing Microsoft Azure Cloud and Cosmos DB

The public cloud provides services such as virtual private servers and databases hosted as a service to the general public on a subscription basis. Some platforms, such as Amazon, charge an hourly subscription or offer pay-as-you use (Lambda) services, while others charge a fixed amount for a month, such as DigitalOcean, and so on. The Windows Azure public cloud platform is one of the important pillars of **Microsoft's Cloud OS vision** that is transforming the traditional data center environment, helping businesses unlock insights in data stored anywhere, enabling the development of a wide range of modern business applications, and empowering IT to support users who work anywhere on any device while being able to manage these devices in a secure and consistent way.

In this chapter, we are going to cover the following points related to Microsoft Azure Cloud and Cosmos DB:

*   Azure features
*   Azure as a **service, platform, and infrastructure** (**SPI**)
*   Azure components
*   Getting started with Azure
*   Azure Cosmos DB
*   Azure Cosmos DB features
*   Azure Cosmos DB use cases

Let's get started by explaining the topics one by one.

# Microsoft Azure Cloud features

One of the famous and simplest quotes about Azure goes as follows:

Windows Azure can be anything you want it to be 

Microsoft Azure Cloud provides a wide range of different services. Windows Azure lets you build, deploy, and manage solutions for almost any purpose you can imagine.

Windows Azure is a world of unlimited possibilities. Irrespective of whether you're a large enterprise spanning several continents that has to run server workloads or a small business that needs a website with a global presence, Windows Azure provides a platform for building applications that can leverage the cloud to meet the needs of your business.

Microsoft Azure provides the following key features:

*   **Open**: Azure provides vast collections of **software development kits** (**SDKs**) and tools for various languages in order to build scalable cloud-based applications.
*   **Flexible**: Whether you are running a small business or a huge, high-end processing system, Windows Azure provides  various services and scalability to help you with all the stages of your business.
*   **Microsoft-managed**: Microsoft has data centers across three continents—the United States, Europe, and Asia. Microsoft itself manages these data centers instead of hiring third-party people to run these, which most other providers do.
*   **Compatible**: You can integrate your existing cloud premise with Azure easily using either support teams or tools.
*   **CDN**: Azure provides a **content delivery network** (**CDN**) that consists of a geographically distributed system of servers that enables Windows Azure to deliver high-bandwidth content for performance-sensitive customers by caching data, images, pictures, and video from Windows Azure compute instances.
*   **Distributed database:** Azure cloud provides easy-to-manage distributed databases such as SQL Server and Cosmos DB to help developers scale and build global range applications.

You can refer to this diagram for all of the components Azure Cloud offers:

![](assets/7103ab2a-3634-4473-b4d8-258975cef0d9.png)

Let's dive more into what Azure can do as a **serial peripheral interface** (**SPI**).

# Microsoft Azure as an SPI

There are many cloud computing platforms offered by different organizations such as Google Cloud, Amazon Web Services, and so on. Windows Azure is one of them, and is provided by Microsoft Azure.

Azure is described as the managed data centers that are used to build, deploy, and manage the applications and provide services through a global network. **Platform as a service** (**PaaS**) and **infrastructure as a service** (**IaaS**) are the services provided by Microsoft Azure. It supports majority of the programming languages.

# Azure as PaaS

Azure provides platforms to clients to develop and deploy software without worrying about the server infrastructure and configuration. You can focus on the development of the application and leave the infrastructure worries to Azure.

One of the key perks of using Azure is the overall cost, which is low as the resources are allocated on demand and servers are automatically updated. Azure provides automatic updates to avoid security issues, and it does all that in the background without affecting the application.

# Azure as IaaS

Azure provides a managed compute service that gives complete control of the operating systems and the application platform stack to the application developers. It provides developers with control over the data center and allows them to manage it.

This is the ideal case for projects where complete control is required. The virtual machine can be completely altered to the requirements of the project or business. However, if not managed carefully, it could become troublesome; all the security updates then become the job of the developer who manages the infrastructure.

The maintenance of the legacy application also sometimes becomes difficult due to operating system and application stack updates.

# Azure components

Please refer to the following diagram, consisting of Azure components:

![](assets/8fad0dbb-ad43-4762-83cc-fd95c10c1637.png)

As you can see in the preceding diagram, there are different execution models, in particular the distinctive advancements that can be utilized to execute applications running in the Windows Azure cloud. The execution models mainly consist of the four Windows Azure compute services, websites, cloud services, and mobile services.

At the base are the SDKs for different languages which you can use for building, sending, and overseeing applications that can be kept running on Windows Azure.

The SDKs that are presently upheld incorporate .NET, Java, PHP, Node.js, Ruby, Swift, and Python. There is additionally a general Windows Azure SDK that gives essential help to any programming dialect, for instance C++.

Enough of the theories; let's get started with Azure.

# Getting started with Azure

To start with Azure, first you need to visit Microsoft Azure and create a new free account or use your existing Hotmail account. The free account comes with a credit and you can use that credit for 12 months. You need to add your credit card information and verify it. Microsoft Azure won't charge you without the required action from the user's side:

![](assets/5d776aaa-3eb6-45d9-80bf-60942ae2b58c.png)

When your account is created, log in into the Azure portal. Once your login is successful, you should see a screen similar to the following:

![](assets/7e53220f-841e-40d4-a76d-f353814e511a.png)

Let's understand the components and functionalities of each module in brief.

Azure components are divided into the following major components:

*   Compute modules
*   Data management modules
*   Networking module
*   Big data module
*   Caching and messaging module
*   Identity module
*   Mobile service module
*   Internet of things

Let's look over each of them.

# Compute modules

As the name suggests, the job of this module is to provide a set of tools and services to do computing. Here is the list of services which are provided under compute modules:

![](assets/92016aa3-1e04-47b3-aa27-334a5fd388ec.png)

As seen in the preceding screenshot, these components mainly consist of virtual machines and containers, but as per the requirements you can join them together as well.

For example, a virtual machine with more disks and snapshots along with containers is used for continuous delivery.

Let's look over the next component, the data management modules.

# Data management modules

Data management modules consist of SQL Server, Azure Cosmos DB, Redis, PostgreSQL, and data warehouses. You can deploy these data management modules and use them in your app.

Here is the complete list of data management modules:

![](assets/80c5f631-c550-4d07-9626-beb379ea0b48.png)

We are going to cover Azure Cosmos DB in an upcoming section. Microsoft Azure started to support MySQL and PostgreSQL in the current version (only at the time of writing this book).

Let's look over the networking module.

# Networking module

The Azure traffic manager is in charge of directing the request of a user cleverly to an accessible data center. The procedure includes finding the closest data center to the user who makes the demand for the web application, and if the closest data center is not accessible because of different reasons, the traffic manager deviates the demand to another data center.

Be that as it may, rules are set by the administrator of the application regarding how the traffic manager should act in such cases. The virtual network permits a system between nearby machines at your commencing place and a virtual machine in Azure's data center. IPs to virtual machines can be appointed for identity and accessibility purposes. Virtual network setup requires a virtual private network device.

Azure also provides an application gateway which is very useful in micro services application development.

Following is the list of networking modules Azure supports at the time of writing this book:

![](assets/eba3eff8-7438-45d1-a1b1-160b7b91aea9.png)

Let's look over the big data and business intelligence modules.

# Big data modules

The extensive measure of data can be stored and retrieved using Windows Azure. Azure offers HDInsight, which is a Hadoop-based service to determine and sense out enormous information stored as big data. Companies regularly need to oversee extensive measures of information which are not fundamentally relational in nature. Hadoop is used by many organizations nowadays, and it is used in Azure too.

In this manner, Azure offers Hadoop benefits on their platform for their customers. The term *big compute* alludes to high-performing calculations. This is accomplished by executing code in parallel on many machines in the meantime.

The components offered by big data modules are shown in following screenshot:

![](assets/9e815bdd-caa1-4533-b85e-38700a7ebaa1.png)

As you can see, Azure offers big data services ranging from storage using Hadoop clusters to performing analytics over big data, performing log analytics to determine errors, and generating reports based on your data.

Azure also provides machine learning web services to be used as smart algorithms to train and derive sense out of data.

Let's look over the caching and messaging modules.

# Caching module

The Windows Azure caching service helps you to build a high-end, scalable, and dedicated cache for your website.

Azure cache is offered in three different types:

1.  **Basic**: A cache size less than 1 GB comes with this type.
2.  **Standard**: A cache size ranging from 1 GB to 10 GB comes with this type.
3.  **Premium**: A cache size more than 5 GB and less than 150 GB comes with this type.

Let's look over the messaging module.

# Messaging module

Windows Azure offers two options for handling the interactions between two apps:

*   Message queues
*   Service bus

Generally, service-oriented architecture or SOA-based applications use service buses to perform the communication between various services.

Distributed and independent services, such as micro services, use message queues to perform the communication between them. The developer can choose one according to their needs and requirements.

Let's look over the identity module.

# Identity module

This component is all about the administration of users, validation, and authentication/authorization. The active directory stores the data of clients getting to the application and, furthermore, the association's data within the organization. It can synchronize with the related data on nearby machines dwelling on-premises for effective sessions.

Here are the security and identity modules provided by Azure:

![](assets/fbe03a7a-013b-44fd-88ba-73670b42756c.png)

Microsoft Active Directory is one of the most mature and popular authentication and authorization modules on the market.

Let's look over the mobile service module.

# Mobile service module

Windows Azure offers a simple platform to create portable mobile applications. You can begin the development of your mobile app using the Azure mobile SDK. There are so many libraries already available in Azure SDK such as notification, authentication, and so on. You can focus on your business logic and Azure SDK will help you in the common routines to ship your app faster.

Windows Azure provides a very easy platform to develop mobile applications. You can simply start using mobile development tools after logging in to your account. You don't have to write big custom code for the mobile application if you use this service. The push notifications can be sent, data can be stored, and users can be authenticated in less time.

Please refer to the following screenshot for the full list of mobile service modules:

![](assets/47b38414-6aaf-48b1-b002-70c16221ed2d.png)

As you can see, the list of modules is enough for mobile app developers to bootstrap their application quickly and easily.

Let's look over the last, and one of my favorite, modules: the **internet of things** (**IoT**)!

# IoT modules

IoT is a hot subject right now and Azure is one of the first providers in the cloud market to facilitate such services. IoT modules allow developers to deploy services such as IoT device communication, networking, and device allocation to the hub.

Refer to the following list of modules provided by Azure for IoT purposes:

![](assets/69690f62-c5eb-45f3-bc27-d5e67c165ed9.png)

There are other modules, along with add-ons such as developer tools, monitoring tools, Bing Maps **application programming interfaces** (**APIs**), and so on.

I hope this provides you with brief but useful insight into Azure. Let's dive into Azure Cosmos DB now.

# Azure Cosmos DB

Azure Cosmos DB is Microsoft's globally distributed, multi-model database with scaling at the click of a button. You can scale Azure Cosmos DB across 30+ Azure regions spread across the globe.

It's multi-model, hence it supports document storage, table storage, and graph values all together in a single database. Azure provides excellent throughput, latency, availability, and consistency guarantees with comprehensive **service level agreements** (**SLAs**) which, as of now, no other database provider is offering.

SLAs represent the promise on paper. That means Microsoft Azure offers you upper bound time for various features such as latency, failover, and so on. For more information, visit [https://azure.microsoft.com/en-us/support/legal/sla/cosmos-db/v1\_0/](https://azure.microsoft.com/en-us/support/legal/sla/cosmos-db/v1_0/). 

Here is the featured and introductory representation of Azure Cosmos DB:

![](assets/f67a46e8-c26c-484a-82b9-18df307d2510.png)

Here are the key features and capabilities Microsoft Azure Cosmos DB offers:

*   Turnkey global distribution
*   Multilevel data models
*   Scale on demand
*   High response with less latency
*   High availability
*   Consistency models
*   Money-back guarantee with SLA

Let's look over each of them in brief.

# Turnkey global distribution

Azure Cosmos DB is baked with scaling and distribution. Mobility and cloud are the current major sources of computing use and that creates the challenge of high availability with less latency.

Azure Cosmos DB provides an easy way to build and distribute your application across an Azure data center without doing manual work and configuration. Azure automatically replicates your data across a number of data centers available in the Azure network.

We will be covering turnkey distribution in the next chapter in detail.

Let's look over multilevel data models.

# Multilevel data models

Azure Cosmos DB supports multiple data models, including document stores, key values, graphs, and tables. Azure offers consistency and high availability across all data models, regardless of their nature, to store the data.

At the time of writing this book, Azure supports APIs for the following models:

*   Document DB API
*   MongoDB API
*   Table API
*   Graph API

The Microsoft team is working to support other data models, if required, and to enhance the existing ones.

We will study the data models in detail in [Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Databases*.

Let's look over the scaling feature of Cosmos DB.

# Scale on demand

Azure Cosmos DB introduces a global turnkey feature; using this, you can deploy and scale your application with literally the click of a button. Azure handles every key technical detail such as infrastructure management, replication, traffic diversion, and so on.

Almost every database as a service offers this scaling, so what's new? Azure offers a software level agreement for latency and deployment time, and if it takes more time than promised, you will get your money back!

Let's look over the latency responsiveness of Cosmos DB.

# High response with less latency

According to the Azure documentation:

*"Azure Cosmos DB guarantees end-to-end low latency at the 99th percentile to its customers."* 

This is a huge promise. Cosmos DB offers end-to-end latency reading under 10 ms for 1 KB of payload, and write under 15 ms in the same region of Azure. This is very impressive and helpful.

With this high response rate and low latency, let's see how Cosmos DB manages consistency.

# Consistency models

Most of the distributed databases offer consistency models, mainly **strong** and **eventual consistency**. Azure offers three more consistency levels. They are:

*   **Bounded staleness consistency:** This guarantees that the read may lag behind the write. This consistency model increases the cost of a read option and is higher than session and eventual consistency.
*   **Session consistency**: This is ideal for all scenarios in which a user is involved since it needs real-time reads and writes.
*   **Consistency prefix**: This guarantees that the data written in order will be seen and read in the order that it was performed.

Let's look over the main promise of Azure Cosmos DB, which is its money-back guarantee.

# Money-back guarantee and SLA

Azure Cosmos DB promises that your data gets there fast, or they send your money back. Azure Cosmos DB provides service level agreements for availability, latency, throughput, and consistency.

An SLA, as of now, is offered only by Microsoft. You can refer to the complete documentation here: [https://azure.microsoft.com/en-us/support/legal/sla/cosmos-db/v1\_0/](https://azure.microsoft.com/en-us/support/legal/sla/cosmos-db/v1_0/).

Let's look over the few use cases of Azure Cosmos DB.

# Cosmos DB use cases

You can use Cosmos DB in scenarios which are highly distributed, mission critical, highly available, and easy to deploy. The following are the use cases offered by the Microsoft team:

*   IoT
*   Retail and marketing
*   Gaming
*   Web and mobile

Let's look over the use cases briefly.

# IoT

IoT consists of various devices that emit data every fraction of a second. This places a need for high ingestion from multiple sources. Azure IoT hub can be used to receive the data from various IoT devices.

Once the data is ingested, you can use Azure HDInsight to process and derive real-time insight. You can then store this data in multiple sources, such as tables, to view in SaaS-based applications, DocumentDB to process the data for graphs and processing, and so on.

Let's look over the next use case.

# Retail and marketing

Microsoft uses Azure Cosmos DB in the Microsoft e-commerce platforms Windows Store and Xbox Live. Azure Cosmos DB can be used to store product data, search the products, and perform the session, which is very important for e-commerce applications.

Azure Cosmos DB can also be used for event sourcing using its own change feed functionality. Change feed provides the amazing event generation mechanism to handle micro service communication.

You can refer to the following diagram about micro service communication:

![](assets/bc6a7eb3-526e-4032-adfe-bb6b5b6d306f.png)

Change feed is a concept introduced by RethinkDB; Packt authored a book on this called *Mastering RethinkDB*.

Let's look at the next use case, which is my favorite one, gaming.

# Gaming

Games are highly mission critical and should be very much available at all times. That requires fast and highly available databases. Azure has already proved its worth in games such as *Halo 5*, *The Walking Dead*, and so on. Azure Cosmos DB performance can be used to handle millions of requests and to show real-time data feeds and social engagement.

Azure Cosmos DB provides an API that can be directly used in games, and with the help of database functions such as Hadoop, could be used for real-time analysis and feeds.

Let's look over the web and mobile use case.

# Web and mobile applications

Azure provides various sets of tools such as key value stores, traffic manager, gateways, and of course Cosmos DB. These can be used in building social media applications, news feeds, and mobile applications using various frameworks.

Azure machine learning and Hadoop can be used as well to generate personalization reports, feeds, and recommendations, which is an amazing part of any modern application. Since most of the data is stored in JSON, it can be communicated and parsed across various scenarios such as the frontend, mobile, backend, and database.

# Summary

In this chapter, we learned about the Azure platform and its features. We looked at the services and infrastructure offered by Azure cloud to develop mission critical applications. We also looked over Azure Cosmos DB and its features and use cases.

In the next chapter, we are going to learn more about the Azure turnkey distribution feature which is one of the key selling points of Azure Cosmos DB.

# Cosmos DB Global Turnkey Distribution

Cosmos DB provides multi-model databases, various consistency levels, security, and infrastructure, but one of the key selling points of Cosmos DB is its turnkey global distribution system.

The Cosmos DB turnkey distribution system provides a feature the user can use to replicate their database across any Azure data center at literally the click of a button.

In this chapter, we are going to learn about turnkey distribution along with the following topics:

*   Setting up an Azure subscription
*   Creating an Azure database
*   Adding data in MongoDB
*   Distributing data using global turnkey distribution
*   Failover handling

Let's begin.

# Setting up an Azure subscription

I am sure you have already created your account on Azure. You need to upgrade your account in order to use the resources, such as Cosmos DB, virtual machines, and so on.

At the time of writing the book, Azure provided us with free credit of around 13,000 Indian rupees (approximately $200) for a month to try out their services at first upgrade!

Open Azure portal ([https://portal.azure.com](https://portal.azure.com/)) and log in using your Microsoft account. Once you have logged in, click on any services, for instance, click on Cosmos DB's icon from the left menu.

You should be viewing the subscription popup; click on that and fill in your credit card information. Microsoft will charge around 2 Indian rupees to verify your identity. They will not charge you further.

Once your account is verified, you can see your subscription in the account section. Refer to the following screenshot for reference:

![](assets/3e792ee6-27c4-410b-833d-44610975c18a.png)

Let's go ahead and create an Azure Cosmos DB.

# Creating an Azure Cosmos DB database

Let's create our first Cosmos DB:

1.  Open Azure portal ([https://portal.azure.com](https://portal.azure.com/)) and click on the Azure Cosmos DB link placed in the left navigation bar:

![](assets/f136f680-d7af-44d6-ad86-3f9dddeabc8d.png)

2.  Click on the Create Azure Cosmos DB button. Fill in the details. Give it a proper name; choose MongoDB in the API dropdown.
3.  Create a new resource group by giving the proper names to identify the data and services easily, which also helps with backups and restoring. A resource group is a container that holds related resources of particular Azure solutions. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You can deploy, delete, and manage all the resources required for your app within the group.

Choose any location; it's totally your choice. Generally, the decision to choose a location is made based on the number of requests coming from particular regions and we should have the location of our Server located nearby the demanding users for fast processing and low latency.

4.  Check on the Enable geo-redundancy button if you want the automatic database replication on different geolocations for higher availability.

Here is what I have selected:

![](assets/2d8669cf-133f-4ab0-b119-118cb08ac200.png)

5.  Click on the Create button to create the database. It will take a few seconds to set up and create the database.
6.  Now, click on the Refresh button (yeah, it's not Ajax) to see the database, just like the one shown in the following screenshot:

![](assets/fd074afc-ecdb-4652-a739-5cd9d72da261.png)

7.  Click on the database; it will take you to the database administration page. Now, first we need to store some data in our database in order to see the method of global turnkey distribution.

As you may be aware, MongoDB stores data in collections. Collections, in big data terms, is a place where all objects are stored.

8.  First we will create a collection. In the navigation bar at the top, find and click on the Add Collection button.
9.  Fill up those values, such as name and database name. Kindly refer to the following screenshot:

![](assets/ff7910ce-db04-4c84-969c-b9b6c5c98655.png)

If you are confused about why you have to create a new database here, too, then let me explain.

Cosmos DB is a multi-model database; that means other databases, such as MongoDB for instance, run under Cosmos DB. MongoDB does have databases, collections, and so on, which stay as they are. On top of the MongoDB functions, Cosmos DB will provide you with the REST APIs to manage those databases and scale them efficiently.

Keep the throughput capacity as `1000` now; it means 1,000 requests per second of a 1 KB document can be made to Cosmos DB, which is enough for testing purposes.

10.  Click on the OK button, and this should create a new collection.

Once the collection is created, we can dump some data in it.

# Adding data in MongoDB

Let's add few data in the MongoDB:

1.  Once the collection is created, you can add new documents to it. Click on Data Explorer from the left menu and select the database.
2.  Refer to the following screenshot if you are having difficulty finding the database:

![](assets/70bf32b7-820c-4f4c-85b6-71bea8011f6e.png)

3.  Click on New Document and it will open up the query editor. Copy the following document and paste in the editor, or you can create your own:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "India" 
} 
```

4.  You can add more fields of your choice. Click on the Save button to add the record in the MongoDB collection. Repeat the steps to add multiple documents.
5.  You can also query the data you have added in the collection from the Data Explorer screen. Click on the New Mongo Query button on the top menu and it will open up an editor. Add any Mongo query you like, or just add `{}` to fetch all documents from the MongoDB collection.
6.  Refer to the following screenshot to see how it works:

![](assets/fe5aab4f-8b70-4f5f-8f03-eb6500eb6731.png)

Results may vary on your account.

So far we have created the Cosmos DB instance, created a new MongoDB database and collection, and added some dummy data to it. I think it's time to see how turnkey distribution works; is it really what Microsoft has promised? Feeling pumped? Me too; let's dive in.

# Distributing data using global turnkey distribution

Let's do it. We have chosen one data center location while creating the Cosmos DB instance. Let's replicate this using the *click of a button* as promised by Microsoft:

1.  In the left menu on the Cosmos DB management screen, go to Settings | Replicate Data Globally and click on it.
2.  It should open up a screen similar to the following screenshot:

![](assets/3ea8e6c6-98b2-41aa-9d8f-9ee5fc45fcc3.png)

See that beautiful map of our planet? Well, all the pentagon icons are the locations of Azure data centers, and the one ticked is ours.

Right now, we have stored our data in one location; let's distribute it to the **West India** location. You can choose the location of your preference.

3.  So, how do we do it? Well, just click on the pentagon icon and click on the Save button; you should be good to go! Isn't it amazing? Distributing data with at click of a button!
4.  It will take few seconds or minutes depending on the data. Once it is done, you will see two ticks on the map, and one more read region, as shown in the following screenshot:

![](assets/c7f84ddf-1ec1-43dc-830b-944890a4aa86.png)

So now traffic from Southeast Asia can read data from its nearest data center. You can replicate this in various locations depending on your traffic.

Each data center location has its own price range and you will be charged on your account accordingly.   
Each replicated node will have read only ability. When there is any change in a write region, Cosmos DB will replicate it across every node distributed across the globe. 

This concludes the data distribution using global turnkey feature of Cosmos DB.

Let's look at failover handling briefly.

# Failover handling

In the event of an Azure data center outage, Cosmos DB automatically triggers failovers of all Cosmos DB databases with a presence in the affected region.

There are two types of failover handling supported:

1.  Automatic failover handling
2.  Manual failover handling

Let's look at each of them briefly.

# Automatic failover

If any read region fails, then Cosmos DB automatically disconnects that read region from a write region and diverts all its traffic to the preferred region list provided by the user.

If the user has not provided any region list then the traffic will be diverted to the main write region for further processing.

Once the read region is online, traffic will be automatically diverted to that region. All of this happens without a single line of code change in your application.

If the write region fails, then using a consensus protocol, another read region would be promoted as the write region. You can also specify which read region should become the write region in case of failure.

Let's look at manual failover.

# Manual failover

If you want to change your current write region to another read region, then you can do that using the manual failover feature. You can upgrade any read region of your choice to a write region and transfer the write traffic. This ensures no loss of data whatsoever as well.

# Summary

In this chapter, we have learned about the core features of Cosmos DB and created a database using MongoDB as a data model. We replicated these data sources globally using the global turnkey feature.

In the next chapter, we are going to learn more about the multi-model databases of Cosmos DB in detail. We will cover each database, and how to create them, query them, and connect them with your application.

# Cosmos DB Multi-model Databases

Cosmos DB is Microsoft's globally distributed (using a turnkey distribution), horizontally partitioned, multi-model database as a service. Cosmos DB allows its users to scale their database throughput and storage across 30 plus regions spread across the globe.

Cosmos DB is the first database as a service that offers service-level agreements for most throughput and low latency. Cosmos DB is designed to power multi-tenancy and scale.

In this chapter, we will cover the following topics:

*   Cosmos DB data models
*   Atom record sequence data model
*   DocumentDB API
*   MongoDB API
*   Table API
*   Graph API
*   Consistency models

Let's begin!

 

# Cosmos DB data models

A Cosmos DB data model is based on an **Atom record sequence** (**ARS**) data model. ARS is built to support multiple data models, such as column-based, key value, graph-based, and document-based. You will learn more about ARS in the next section.

At the time of writing this book, Cosmos DB supports the following data models:

*   DocumentDB
*   MongoDB
*   Tables
*   Graph
*   Cassandra

The Microsoft Azure team is working on other data models too such as column based; they will release them soon!

DocumentDB is Microsoft's own document-based database also known as the NoSQL database. DocumentDB provides SQL query capabilities over schema-less document-based records. Cosmos DB provides an API and SDKs in various languages to integrate DocumentDB with your app! You will learn about it in the coming sections.

MongoDB is a very popular document store that has both an open source and enterprise version. MongoDB provides all complex operations such as aggregation, filter, and map-reduce over document store data. This is one of my favorite databases to start and ship any product faster. You will learn more about MongoDB in the coming sections.

Tables or table storage is Microsoft's version of Redis, a NoSQL-based key value store deployed as a service to hold petabytes of data and scale on the fly! You will learn more about it in the coming sections.

The graph is a database that consists of vertices and edges rather than key and value or column and rows. Graph databases are mainly used as a recommendation or suggestion feature. I am sure that you have used the *People you may know* feature of Facebook, that's graph! You will learn more about in the coming sections.

Cassandra is an open source, Apache licensed, distributed NoSQL database to mission-critical data with amazing durability and availability. Cassandra is proven and been used in some of the popular sites on the internet such as GitHub, Instagram, Reddit, eBay and CERN (not an internet site though). We will learn more about it in the coming section.

We will go deeper into each data model, but let's first look at what ARS is all about!

# ARS

In order to achieve multi-model database functionality, Cosmos DB supports the ARS model. The ARS model provides a very low-level data format, which can be used by any other databases or programming languages.

Atoms consist of a set of primitive data types such as number, string, and Boolean. Records consist of structure (set of primitive data types) and sequences that are arrays that hold atoms and records.

The data stored in this way is then used by your top-level databases such as Document DB, MongoDB, and tables. So, in layman's terms, whatever data you create using any of the Cosmos DB APIs is first converted into an ARS model and then stored in various regions.

This standard means of data storage opens up many opportunities. First, you can migrate or change the data model on the fly. Second, you scale and distribute it anywhere in the supported regions because the data model is technically the same for each database.

If you want more in-depth information about ARS, then refer to this official technical documentation page [https://azure.microsoft.com/en-in/blog/a-technical-overview-of-azure-cosmos-db/](https://azure.microsoft.com/en-in/blog/a-technical-overview-of-azure-cosmos-db/).

Let's go ahead and dive deeper into each database model. First, you will learn DocumentDB API.

# DocumentDB API

As we mentioned earlier, DocumentDB is Microsoft's own document-based NoSQL database. DocumentDB provides SQL query capabilities over schema-less document-based records.

Cosmos DB provides an API and SDKs in various languages to integrate DocumentDB with your application. Let's see a few advantages and features that DocumentDB API with Cosmos DB provides:

*   **Elastic scale**: Effectively scale up or downsize your database to meet your application needs. Your information is stored on **solid state drives** (**SSD**) for low latency. Cosmos DB stores the data in various collections, which can be scaled and used for a large set of records.
*   **Replication**: As you have learned in the last chapter, Cosmos DB databases, such as DocumentDB can be replicated across any number of regions supported in the Azure data center. This provides us the ability to develop globally-available applications with as low latency as possible. Cosmos DB also provides failover handling whether it's automatic or manual as discussed in [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution*.
*   **SQL-like query**: DocumentDB is a NoSQL database, but with Cosmos DB, you can query those JSON documents using SQL-like queries. You can query your data at a very high speed without the need for specifying the indexes, views, and so on as we do in SQL databases.
*   **Multiple consistency levels**: You can select the consistency among the top five well-defined consistency levels. The consistency levels are strong, bounded staleness, session, consistent prefix, and eventual. This allows you the flexibility to choose and trade-off the performance and data consistency. In some applications, there is no need for a strict write but data needs to be shown in real time. In other applications, there is a need for strict write in the database. You can trade-off and choose the consistency levels. We will discuss more about the consistency levels in the coming sections.
*   **Complete document indexing**: Cosmos DB, by default, for performance sake, indexes all the documents in the database. You can, however, specify to provide an index on which document or key if you want, but this is the default behavior.
*   **Real-time change feed**: You can listen to every change in the database and get the notification. You can use this change feed to initiate the API call, do replication, or build an audit log.

Let's go ahead and create a DocumentDB database:

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the Option bar.
2.  Click on the + button and give your new database a proper ID and choose the SQL option from the API dropdown.

You can create a new resource or use an existing one. Select the location where the most writing will happen and click on the Create button. You can refer to the following screenshot for further information:

![](assets/a882fdeb-99ca-437c-acf9-7e835e18ef2f.png)

Azure will create the new database with DocumentDB API and notify you. You will get a notification similar to this:

![](assets/21691fbd-b3a2-49f4-ba32-4a3aa08afcc3.png)

Let's create a new collection and store some data in it:

1.  Go to Data Explorer on the left menu and click on the New Collection button.
2.  Give your database and collection a name and click on the OK button. Refer to the following screenshot for further information:

![](assets/fd583861-5d18-43c7-8179-a1312f43a501.png)

3.  Once the collection is created, we can create new documents in it. Click on the New document button and add the following JSON object:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
} 
```

4.  Click on the Save button and the new document will be created.

5.  Once the document is created, we can query it using standard SQL-like queries. Click on the New SQL Query button and paste this query:

```
SELECT * FROM users 
```

6.  You can replace `users` with the collection name you provided. Do not add a semicolon as it shall throw an error.
7.  Run this query by clicking on the Execute Query button and you shall see the records as shown in the following screenshot. It may differ from your result:

![](assets/cf0a1458-658c-4e4a-a49e-3cb5c19e4587.png)

8.  You can create new triggers and stored procedure from Data Explorer. For a demo purpose, let's create a simple stored procedure called `helloworld`, which upon execution returns the message `Hello, World`.
9.  In Data Explorer, click on stored procedure and create a new stored procedure. Give it a name, that is, `helloworld` and paste the following code and save it:

```
function storedProcedure(){ 
    var context = getContext(); 
    var response = context.getResponse(); 
    response.setBody("Hello, World"); 
} 
```

10.  On the left menu, click on Script Explorer and it will list the scripts we have created along with the stored procedure.
11.  Click on the `helloworld` procedure and click on the Execute button. This will run the stored procedure and show you the output as follows:

![](assets/35afc3fb-baec-4ce0-a9a4-1c682c9f761d.png)

Similarly, you can create triggers as well and test it from Script Explorer.

12.  You can change the consistency levels from the default consistency settings. Choose any among the five consistency levels as shown in the following screenshot:

![](assets/a054129a-18af-48eb-b4ba-35922dd645f9.png)

You will learn about the change feed feature when we try programming the SDKs of Cosmos DB in [Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Cosmos with* *Node.js.*

Let's move ahead to the next database model that is, MongoDB.

# The MongoDB data model

MongoDB is one of the most popular open source NoSQL databases in the world. MongoDB is one of the key competitors to Microsoft's DocumentDB. MongoDB provides community license and enterprise version for organizations.

You can use MongoDB as a data model in Cosmos DB. If your existing application already uses MongoDB, then all you need to change is the connection parameter and your application will work! It is as smooth as that.

Cosmos DB provides an API and SDKs in various languages to integrate MongoDB with your application. Let's see a few advantages and features that MongoDB API with Cosmos DB provides:

*   **Elastic scale**: Effectively scale up or downsize your database to meet your application needs. Your information is stored on an SSD for low latency. Cosmos DB supports the MongoDB format, that is, collections that can be scaled and used for a large set of records.
*   **Replication**: As you have learned in the last chapter, Cosmos DB data models, such as MongoDB can be replicated across any number of regions supported by the Azure data center. This provides us with the ability to develop a globally available application with as low latency as possible. Cosmos DB also provides failover handling, whether it's automatic or manual as discussed in [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution*.
*   **Compatibility**: Cosmos DB keeps the MongoDB API and SDKs as independent as possible. You can use your existing MongoDB expertise, code, and APIs to work with MongoDB and scale it. Cosmos DB provides fully managed MongoDB along with the  other features to help you scale and manage it properly.
*   **Fully managed**: You don't need to install or run the MongoDB daemon or server anymore. It is fully managed by Microsoft. Fear of restarts, crashes, and being out of memory are no more!
*   **Automatic indexing**: Cosmos DB indexes all documents of MongoDB for faster response time and low latency.

Let's go ahead and create a MongoDB data model in Cosmos DB:

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the option bar.
2.  Click on the + button and give your new database a proper ID; choose the MongoDB option from the API drop down.
3.  You can create a new resource or use the existing one. Select the location where the most writing will happen and click on the Create button. You can refer to the following screenshot for further information:

![](assets/e051258b-4a79-46f6-afe1-55b4c547c694.png)

Azure will create the new database with MongoDB API and notify you of the same.

4.  Once the MongoDB data model is created, we can create new collections in it and store the data.
5.  Go to Data Explorer on the left menu and click on the New Collection button. Give your database and collection a name and click on the OK button. Refer to the following screenshot for further information:

![](assets/fccdf267-5b5e-49d8-9425-de3ed6d8e951.png)

6.  Once the collection is created, we can create new documents in it. Click on the New document button and add the following JSON object:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
}
```

7.  Click on the Save button and the new document will be created.

Once the document is created, we can query it using standard MongoDB queries.

8.  Click on the New Mongo Query button and paste the preceding mentioned query.
9.  Run this query by clicking on the Execute Query button and you shall see the following records as shown in the next screenshot. It may differ from your result:

![](assets/9794d330-f8af-4f69-8ea3-298b17fb96e8.png)

Similarly, we can delete these documents or update them using Data Explorer. You can also create stored procedures and triggers as we have discussed in the previous section *DocumentDB API*.

MongoDB is one of the key database models in Cosmos DB. One of the core reasons is due to its popularity. The majority of developers and enterprises are already using it, and it would be a great customer base if they migrate to Cosmos. This is the key reason why they describe it as an easy-to-migrate model; otherwise, why would Microsoft offer a competitor as an option!

You will learn about the SDKs and how to integrate them with our custom application in [Chapter 6](#5c360edf-dd5e-4d41-991a-11103f5b7705.xhtml), *Building High-Performance Link Shortening and Tracking System using Cosmos DB*.

As for the consistency and replication, you can use any consistency levels among the five provided by Cosmos with MongoDB.

You can also distribute it across any number of regions provided by Azure as discussed in [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution*.

Let's go ahead and check out the third data model that is Tables!

# The tables data model

Cosmos DB provides the tables data model as an API, which you can use to build a distributed key value store using the standard table query model.

Table API is one of the first in the market with a managed distributed key value store. Let's look at the features provided by Table API:

*   **Low latency**: Table API with Cosmos DB provides single digit latency for reads and write with SLA
*   **Variable throughput**: Table API has a variable throughput model. You can change the scalability limit as you grow
*   **Elastic scale**: You can scale the database across any number of Azure regions supported
*   **Automatic indexing**: Cosmos DB with Table API creates indexes for all properties for faster read and write operation
*   **Advance query support**: Cosmos DB provides advance query such as aggregation, geospatial query, and sorting on top of Table API
*   **Multiple consistency**: You can choose consistency level among the five provided by the Cosmos DB as per your need

Let's go ahead and create a table data model in Cosmos DB:

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the option bar.
2.  Click on the + button and give your new database a proper ID, choose the Table (key value) option from the API drop down.

3.  You can create a new resource or use the existing one. Select the location where the most writing will happen and click on the Create button. You can refer to the following screenshot for further information:

![](assets/15bd363f-cff0-4eb7-a121-9b2e78ff9f1f.png)

4.  Azure will create the new database with Table API and notify you of the same. Once the table data model is created, we can create new collections in it and store the data. Go to Data Explorer on the left menu and click on the New Table button. Give your table a name and click on the OK button. Refer to the following screenshot for further information:

![](assets/69874c78-4d55-4b8b-8736-163949e03497.png)

5.  Once the table is created, we can create a key value pair in it. Click on the New Entity button and add the following values:

![](assets/514c51db-04f6-4929-b0b3-4362b64441cb.png)

You can create as many entities as you want with the Query Explorer. You can always create a stored procedure and triggers for your application if you need.

Let's go ahead and learn about the last but not the least data model in Cosmos DB that is, graph data model.

# The graph (Gremlin) data model

In SQL database, we have rows and columns; in NoSQL, we have a key and value-based document, and in big data, we have column and data.

However, none of these models are directly mapped to real-world objects. Graph databases are built to bridge this gap.

A graph data model is a structure that's comprised of vertices and edges. Both vertices and edges can have a set number of properties. Vertices represent discrete objects, such as a person, a place, or an event. Edges represent relationships between vertices. For example, a person might know another person, be involved in an event, and may have recently been at a location. Properties denote details about the vertices and edges. Azure Cosmos DB supports the property graph model.

For example, consider the following graph:

![](assets/ffd1bad6-6350-456b-b572-61f61ec3ce74.png)

One of the important use cases of the graph is to find relations between entities. For example, if I am friends with someone, then using the vertices, we can recommend their friends to us, similar to the *Friends you may know* feature of Facebook.

Graphs are also useful in major data science exploration. Complex algorithms are built on top of the graph database to obtain a sense of huge data.

Let's look at some of the features that graph databases in Cosmos DB provide:

*   **Elastic scale**: Effectively scale up or downsize your graph database to meet your application needs. Your information is stored on an SSD for low latency. Cosmos DB supports the Graphlin API format.
*   **Replication**: As you have learned in the last chapter, Cosmos DB data models, such as graphs, can be replicated across any number of regions supported in the Azure data center. This provides us the ability to develop globally available applications with as low latency as possible. Cosmos DB also provides failover handling whether it's automatic or manual as discussed in [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution*.
*   **Fast query traversal**: You can use the Graphlin syntax to create and store graphs and query them with single digit latency with Cosmos DB.
*   **Fully managed**: You don't need to install or run the graph daemon or server anymore. It is fully managed by Microsoft. Fear of restarts, crashes, and being out of memory are no more!
*   **Automatic indexing**: Cosmos DB indexes all properties within the nodes and edges of a graph for a faster response time and low latency.

Let's go ahead and create a new graph database in Cosmos DB:

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the option bar.
2.  Click on the + button and give your new database a proper ID, choose the Gremlin (graph) option from the API drop down.

3.  You can create a new resource or use the existing one. Select the location where the most writing will happen and click on the Create button. You can refer to the following screenshot for further information:

![](assets/e8c917c4-4e59-4d84-b69f-413d6d8f4eb7.png)

4.  Once the graph data model is created, we can create new databases and graphs in it and store the data.
5.  Go to Data Explorer on the left menu and click on the New Graph button.

6.  Give your database and graph ID a name and click on the OK button. Refer to the following screenshot for further information:

![](assets/2e37f583-8da7-4405-aca4-c42435dd554f.png)

7.  Once the database is created, you can create a new vertex. Click on the New Vertex button and give it a label, say, a laptop.
8.  Click on New Vertex again and create a new vertex, say, John. In relation, we will say, John uses Laptop. You can open the properties by clicking on the second vertex from the graph.

9.  Go to Targets, and choose the laptop vertex as a target. Refer to the following screenshot for further information:

![](assets/38ec9c15-a0a2-4e69-9298-16915567cbee.png)

Once it is created, you shall see the graph connected as shown in the following image:

![](assets/f6522893-5a54-40b1-8c2b-625e6d01cc72.png)

This is awesome! We can indeed create more vertexes and connect them together and query it to get some useful information.

We will do more graph queries in the coming chapter that is, [Chapter 4](#f5161897-9696-4441-9e45-f2346e49978e.xhtml), *Querying Cosmos DB*.

Let's look at the Cassandra API supported by the Cosmos DB.

# Cassandra API

Cassandra is open source highly distributed NoSQL database with the industry proven performance and durability. Here are some of the features of Cassandra.

*   **Highly scalable: **Cassandra allows the elastic scale, it allows to manage the hardware requirements of the system on the go. You can add/remove hardware depending on the traffic and requirements.
*   **No single point of failure: **Cassandra is proven to be a highly available database and has no single point of failure built right in the core of the architecture.
*   **Linear scalability: **Cassandra improves the throughput of the system as and when new Cassandra node is added hence it always improves the performance of the system and replicates the effect in hardware quickly.
*   **Data variety support: **Cassandra supports the various data formats such as SQL, NoSQL, Unstructured data and it dynamically accommodate changes to your data structure depending on the need for the system.
*   **Transaction support: **Cassandra supports the transaction for the atomic operation.

By using Cassandra API in CosmosDB, you avail all these features mentioned above with the following benefits as well provided by Cosmos DB.

*   **No management:** CosmosDB provides you the API to manage your data and rest of the infrastructure is managed by CosmosDB.
*   **Automatic Indexing: **CosmosDB by defaults creates the indexes for all the columns in the database, hence it improves the read time drastically.
*   **Global Distribution:** CosmosDB allows you to distribute the database across the globe in a 30+ region using a single click of a button.
*   **Multiple consistency:  **You can choose the consistency level from the strong, bounded-staleness, session, consistent prefix, and eventual. 

This covers all five data models supported by Cosmos DB. Let's go ahead and learn in brief about the consistency levels of Cosmos DB.

# The Cosmos DB consistency levels

In distributed databases, consistency is a very important factor. You need to decide the level of urgency and the importance of the data in your application to figure out the consistency.

Cosmos DB provides five consistency levels:

1.  **Strong**: This consistency level guarantees that a write operation is only visible to the application or consumer of data when it is committed by the majority of the replicas. This increases the cost of the read operation.
2.  **Bounded staleness**: In this consistency level, the read operation may lag behind the write operation. In simpler words, this is the best-suited consistency level for distributed databases because of the low-cost read operation and low latency.
3.  **Session**: As the name suggests, session consistency is bound to the client session. Each session or user session, to be specific, guarantees the monolithic read and write.
4.  **Consistent prefix**: In this level, the read operation will return the data in the sequence in which it was written. If it was written as A, B, and C, then read will return either A, AB, ABC but not BA or BC.
5.  **Eventual**: This is the weakest consistency level. In this, the client may see the data which is an older version and without an order. This is very cost effective but not suitable for mission-critical applications.

You can always choose consistency levels for each Cosmos DB from the data explorer menu.

# Summary

In this chapter, you have learned all four data models supported by Cosmos DB. You also learned how to create databases with each model and put some data in them. You learned consistency levels as well.

In the next chapter, we will dive deeper and perform some good queries in each data model and try to learn more about Cosmos DB data models.

# Querying Cosmos DB

In the last chapter, we learned about various data models and created databases and tables or collections in those data models. It's time to ingest some data in those data models and do the query to see how effective it is.

In this chapter, we are going to cover the following topics:

*   Performing CRUD operations in DocumentDB.
*   Performing CRUD operations in MongoDB.
*   Performing CRUD operations in Graph DB.
*   Performing CRUD operations in tables (key value store).
*   Performing CRUD operations in Cassandra.

Let's begin with Document DB queries.

# Performing CRUD operations in DocumentDB

DocumentDB is Microsoft's own NoSQL database with an SQL-like query feature. When I say SQL-like, I mean it allows us to query the JSON documents using our all-time favorite SQL queries.

Before beginning with the queries, let's learn the basics of DocumentDB query.

# DocumentDB query basics

Every DocumentDB query consists of a `SELECT` statement and optional `FROM` and `WHERE` clauses as per ANSI-SQL standards.

For each query, the source in the `FROM` clause is searched and traversed. Then the filter part in the `WHERE` clause is applied on the source to retrieve a subset of JSON documents.

At the end, the `SELECT` clause is used to populate the requested JSON values in the select list.

The order of the statements can be like the following:

```
SELECT <select_list> 
[FROM <from_specification>] 
[WHERE <filter_condition>]
[ORDER BY <sort_specification]  
```

Let's begin with the query.

# Creating data in Cosmos DB

To perform query operations, we need to add in some data. You can add as many documents as you want but, for the sake of example, I am going to add two nested JSON documents.

The first document is shown in the following code:

```
{ 
  "id": "100", 
  "lastName": "Doe", 
  "parents": [ 
     { "firstName": "John" }, 
     { "firstName": "Mary"} 
  ], 
  "children": [ 
     { 
         "firstName": "Emily",  
         "gender": "female",  
         "grade": 5, 
         "pets": [{ "givenName": "Fluffy" }] 
     } 
  ], 
  "address": { "state": "WA", "county": "King", "city": "seattle" }, 
  "creationDate": 1431620472, 
  "isRegistered": true 
} 
```

Here is the second document, with a few more changes:

```
{
   "id": "200",
   "parents": [
       { "familyName": "Wakefield", "givenName": "Robin" },
       { "familyName": "Miller", "givenName": "Ben" }
   ],
   "children": [
       {
         "familyName": "Merriam", 
         "givenName": "Jesse", 
         "gender": "female", "grade": 1,
         "pets": [
             { "givenName": "Goofy" },
             { "givenName": "Shadow" }
         ]
       },
       { 
         "familyName": "Miller", 
          "givenName": "Lisa", 
          "gender": "female", 
          "grade": 8 }
   ],
   "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
   "creationDate": 1431620462,
   "isRegistered": false
 }
```

To create these documents using Azure portal, perform these steps.

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the option bar.
2.  Go to Data Explorer from the left menu and select the collection we created in the last chapter.
3.  Click on the New Document button and add the JSON mentioned above.

Once the documents are created, we can query them.

# Reading data from DocumentDB

Let's query our data using SQL query structure:

1.  Click on the New SQL Query button and paste this query:

```
SELECT *  
FROM Families f  
WHERE f.id = "100" 
```

2.  This will yield the following result:

```
[{ 
  "id": "100", 
  "lastName": "Doe", 
  "parents": [ 
     { "firstName": "John" }, 
     { "firstName": "Mary"} 
  ], 
  "children": [ 
     { 
         "firstName": "Emily",  
         "gender": "female",  
         "grade": 5, 
         "pets": [{ "givenName": "Fluffy" }] 
     } 
  ], 
  "address": { "state": "WA", "county": "King", "city": "seattle" }, 
  "creationDate": 1431620472, 
  "isRegistered": true 
}] 
```

3.  You can also execute a traditional order by queries, similar to the following one:

```
SELECT c.givenName  
FROM Families f  
JOIN c IN f.children  
WHERE f.id = '200' 
ORDER BY f.address.city ASC
```

4.  This will yield the following result:

```
[ 
  { "givenName": "Jesse" },  
  { "givenName": "Lisa"} 
]
```

5.  You can also run the sub-queries, that is, get the subdocuments from the main document. You can refer to the following query:

```
SELECT *  
FROM Families.children 
```

6.  This will yield the following result:

```
[ 
  [ 
    { 
        "firstName": "Emily", 
        "gender": "female", 
        "grade": 5, 
        "pets": [ 
          { 
              "givenName": "Fluffy" 
          } 
        ] 
    } 
  ], 
  [ 
    { 
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", 
        "grade": 1 
    }, 
    { 
        "familyName": "Miller", 
        "givenName": "Lisa", 
        "gender": "female", 
        "grade": 8 
    } 
  ] 
] 
```

So, we now know how to create and query documents. Let's perform an `UPDATE` and `DELETE` operation.

# Updating and deleting documents

Azure portal does not allow the running of these queries, that is, updating and deleting the document using queries, but you can do it using SDKs of your choice or from the Azure portal UI.

To edit, you can just run the `SELECT` query and edit the document live and click on the Save button to make the changes take effect, or you can click on the DELETE button to remove the document.

We will look over updating and deleting using Azure SDK in [Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Node.js with Cosmos DB*.

Let's look over the MongoDB query operation.

# Performing CRUD operations in MongoDB

MongoDB is one of the popular NoSQL databases. You need to create a MongoDB database and collection and you can do basic CRUD using the Azure Terminal in the browser, or get a connection string and connect it to your node app.

1.  Open your Azure portal and create a new MongoDB instance; refer to the following screenshot for reference:

![](assets/ce361e5c-2669-4dd5-94dd-a34b4e79d0fd.png)

2.  Once it is created, you can add a new collection to it. Refer to the following screenshot for reference:

![](assets/db5a72b5-aa15-48eb-b86b-75e9a7332d6a.png)

3.  Now you need to retrieve a connection string. Go to the Settings | Connection String option and copy the connection string.

Keep it somewhere secure and do not share it. 

Now you can connect your Node.js application to this MongoDB instance using the connection string.

We are going to cover Node.js in [Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Node.js with Cosmos DB*.

You can, however, do the CRUD on the Azure browser terminal itself.

4.  Click on the New document button on top of the bar and add the following JSON to create a document:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
} 
```

5.  Once a document is created, you can view it using the query, or by clicking on the left bar which contains lists of documents. You can delete it by clicking on that small cross button.

We will cover doing the CRUD using code in the next chapter.

Let's see how to do crud operations in Graph DB.

# Performing CRUD operations in Graph DB

As discussed in [Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Databases*, a graph data model is a structure that's comprised of vertices and edges. Both vertices and edges can have a set number of properties:

*   Vertices represent discrete objects, such as a person, a place, or an event.
*   Edges represent relationships between vertices. For example, a person might know another person, be involved in an event, and recently have been at a location.
*   Properties denote details about the vertices and edges. Azure Cosmos DB supports the property graph model.

Let's create a new database first:

1.  Click on New Database and choose the Gremlin as API. Refer to the following screenshot for reference:

![](assets/d4cb2664-8860-437c-a05c-30ef985abfe5.png)

2.  Now, create a new graph database and Graph Id. Refer to the following screenshot for reference:

![](assets/1eb0b04b-e298-405c-81c2-b11c4fcd4960.png)

3.  Now, you need to generate the keys to connect it via any programming interface. Go to Settings | Keys and click on the primary and secondary keys to generate it. Refer to the following screenshot for reference:

![](assets/e6a19f2b-3885-4bea-b185-abe9c7ac11a5.png)

We can use these keys to connect to your Graph database using the code.

Now, coming to the CRUD part, we can create new vertices and edges using the Azure portal as well as code too. We have covered the Azure portal in [Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Database,* as well.

Once you have created some vertexes and edges and added some properties to them, you can perform the query on the graph.

One of the simplest ways is to do so is via the Azure terminal.

4.  Run this query to return the vertices of the graph:

```
g.V().count() 
```

You can, in fact, delete the vertices from the graph and edit them dynamically using the portal UI.

Graph is a very vast topic and you can do lots more in it.

The following is a reference link where you can read further details about graph:  
[https://docs.microsoft.com/en-us/azure/cosmos-db/gremlin-support](https://docs.microsoft.com/en-us/azure/cosmos-db/gremlin-support) 

Now, let's see how to perform the CRUD operation in the Table database API.

# Performing CRUD operations in tables (key value store)

Table storage API provides us with the ability to store key value pair data efficiently and easily. You need to create a new table database inside Cosmos DB to perform the CRUD operation.

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the option bar.
2.  Click on the + button and give your new database a proper ID, then choose the Table (key-value) option from the API dropdown.

3.  You can create a new resource or use the existing one. Select the location where most writing will happen and click on the Create button. You can refer to the screenshot for reference:

![](assets/8bbe31dc-7d64-4f27-80fc-dee0d715466d.png)

4.  Now you can create a table in it to store our data. Refer to the following screenshot for reference:

![](assets/66edfaa7-7a9c-4f22-8426-045f0ab96230.png)

We can perform the CRUD operation on the same data. We need to create the Table entity to store data in that format. Click on the Add new entity and add the row key and partition key. Refer to [Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Databases,* for the same.

Once you have created the entity, we can add the data in it. Also, you can copy the connection string from the Azure portal and paste it to use it further with your custom application.

You can add as many entities as you want and, once done, you can delete, edit, or query it from the data explorer.

Now, let's see our last database API, that is, the Cassandra API.

# Performing CRUD operations in Cassandra.

Cassandra API provides us the ability to store NoSQL data efficiently and easily in Cassandra. You need to create a new Cassandra database inside Cosmos DB to perform the CRUD operation.

1.  Log in to your Azure portal and click on the Azure Cosmos DB option on the left side of the options bar.
2.  Click on the + button and give your new database a proper ID, then choose the Cassandra option from the API dropdown.
3.  You can create a new resource or use the existing one. Select the location where most writing will happen and click on the Create button. You can refer to the screenshot for reference:

![](assets/fc900918-0e66-49a0-9a7e-3b65e8e095d0.png)

4.  Now, click on Data explorer |  New Table button and create a new table to store the data. Refer the screenshot below for the reference.

![](assets/32d22c55-5bd3-4176-ac67-36e721b63842.png)

5.  Now, we can insert data using the standard SQL commands as shown below.

```
INSERT INTO uprofile.user
(first_name, last_name)
VALUES ('Shahid', 'Shaikh');
```

6.  You can read data from the `Cassandra` table using the `SELECT` query.

```
SELECT * from uprofile.user;
```

Similarly, `UPDATE` and `DELETE` commands can be used to update and delete the records from Cassandra table respectively.

This covers performing the CRUD operations using the various data models.

# Summary

In this chapter, we have learned how to perform CRUD operations on each of the database APIs supported by the Cosmos DB. We also learned how to use the connection strings and keys to connect to the database from your code.

In the next chapter, we are going to learn Node.js integration with various Cosmos DB databases.

# Integrating Cosmos DB with Node.js

Cosmos DB supports various programming languages, such as Java, .NET, Python, and Node.js. Node.js is not actually a programming language but a runtime framework for JavaScript.

I am a Node.js enthusiast and more comfortable with the development of a backend system using Node.js, so we are going to do the Cosmos DB integration with Node.js in this chapter, as well as in the next chapters.

Here are the topics that we are going to cover in this chapter:

*   Node.js setup
*   Cosmos DB MongoDB setup
*   Connecting Cosmos DB with the Node.js app
*   Exploring app data in Cosmos DB
*   Integrating the DocumentDB API with Node.js
*   Integrating the Table API with Node.js
*   Integrating the Graph API with Node.js

So, let's begin.

 

# Node.js setup

You need to install and set up Node.js on your machine. Go to the official Node.js website ([https://nodejs.org/en/download/](https://nodejs.org/en/download/)) and download the latest version, depending upon your operating system.

I am covering the installation for the Ubuntu here. Let's go through the steps:

1.  Open your Terminal and run this command:

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -  
```

This will add the Ubuntu repository entry for Node.js version 8+.

2.  Now, run this command to install Node.js:

```
sudo apt-get install -y nodejs  
```

After a while Node.js, along with NPM, will be set up.

3.  To check the version of Node.js and NPM, run the following command:

```
node -v
npm -v 
```

The output is as follows:

![](assets/7f49cf95-4cf2-4cd3-8338-ddd47a48ae03.png)

You can run a similar command in Mac as well as in Windows (using PowerShell or Windows Command Prompt).

This should ensure that Node.js is set up properly and we can proceed with the Cosmos DB MongoDB creation.

# Cosmos DB MongoDB setup

MongoDB is one of the most popular NoSQL databases. You need to create a MongoDB database and collection and generate the access keys.

Open your Azure portal and create a new MongoDB instance. Refer to the following screenshot for reference:

![](assets/f18ae269-218e-46ab-869d-d1bd238324d8.png)

Click on the Create button and a new MongoDB API will be created. Next, we need to connect our Node.js app with MongoDB API of Cosmos DB.

# Connecting Cosmos DB with the Node.js app

The design of Cosmos DB is done in such a way that you can easily integrate it with your app without doing much extra work. I mean, you don't need to do the configuration and management part, which is a huge pain with these NoSQL databases; you can easily connect to the Cosmos DB MongoDB using the existing Node.js modules you are aware of.

We are going to use the same set of node modules that we use for the normal development with MongoDB, no extra stuff!

Let's prove this point by configuring the existing project written in Node.js and MongoDB with our Cosmos DB Mongo API.

For a demonstration, we are going to use a sample code hosted as an open source:

[https://github.com/prashanthmadi/mean](https://github.com/prashanthmadi/mean)

Let's do it:

1.  First things first, download the code. You can either use `git` or just visit the GitHub repository and download it as a ZIP. I am going to use `git` now.
2.  Run this command in the Terminal to download the project:

```
git clone https://github.com/prashanthmadi/mean  
```

3.  Once the project is downloaded, run this command to install the required dependencies:

```
cd mean && npm install
```

4.  It will install the dependencies required for the project to run. Refer to the following screenshot for reference:

![](assets/6f3ad7f1-f55f-42ce-b43f-ad29fd87ec87.png)

Once the code is downloaded, we need to do few changes in order to run it.

5.  First, copy the MongoDB URI. Go to the Connection String screen and copy the Connection String. Refer to the following screenshot for reference:

![](assets/a1f962d3-c995-41d1-b616-9913b8e59fc1.png)

6.  Now, go to the project folder and edit the configuration file. Open up `config/env/local-development.js` and change the connection URI:

```
module.exports = { 
  db: { 
    uri: '===== paste your URL here =====' 
  }, 
  sessionSecret: process.env.SESSION_SECRET || 'youshouldchangethistosomethingsecret', 
  facebook: { 
    clientID: process.env.FACEBOOK_ID || 'APP_ID', 
    clientSecret: process.env.FACEBOOK_SECRET || 'APP_SECRET', 
    callbackURL: '/api/auth/facebook/callback' 
  } 
};
```

7.  Save the changes in the configuration file and run the code base using the following command:

```
npm start  
```

8.  This will run the build and, once the project is started, you can view the project at `localhost:3000`. Refer to the following screenshot for reference:

![](assets/30ba27fe-3566-46fb-9f90-0669d1a7327c.png)

9.  Here is how it's going to look in the browser:

![](assets/b7108fe6-cc7f-454a-8d68-5b0533b9caf5.png)

Congratulations! Our app is up. But we need to do some operation in order to generate data for the MongoDB. Let's create a new account and see how it works!

10.  Click on Sign Up and fill in those details. Once your account is created, you can log in to the system, view and edit your profile, and stuff like that. You should see something like this:

![](assets/f18cad84-2c92-4e39-b97b-1835205bfed3.png)

We have now connected our Node app to MongoDB, so it's time to see the data inside Cosmos DB data explorer.

# App data in Cosmos DB

We have our application connected to the Cosmos DB, and we have created a user as well. Let's view the data in the Cosmos DB MongoDB collections:

1.  Log in to the Azure portal and go to the overview screen of the database we have created. You should see the collections listed over there. Your screen should look like this:

![](assets/a824ea22-0006-437a-8c62-4ec20059bce4.png)

2.  Click on the users collections to view the data inside that collection.
3.  Double click on any **ID** of the collection data to view the complete data. Refer to the following screenshot for reference:

\>![](assets/79a3a906-9c00-467b-a3be-fbddd5896218.png)

So, this confirmed our app connectivity with the database using Node.js. Next, let's look at how to use Node.js with DocumentDB and other database APIs supported by the Azure Cosmos DB.

# Integrating the DocumentDB API with Node.js

DocumentDB is another important database API supported in Azure Cosmos DB. In order to integrate it with Node.js, you need to install the `documentdb` node module. Here are the following steps:

1.  Install the module using the following command:

```
npm install documentdb@latest
```

2.  Once the module is installed, you can use the functions to connect to DocumentDB and perform crud operations and queries.
3.  First you need to create a new document DB API and copy the connection URL.

Here is an example:

4.  First, create a configuration file named `config.js` and add the following code:

```
var config = {}
    
config.endpoint = "paste the document db URI";

config.primaryKey = "paste the primary key";
   
config.database = {
"id": "packt-users-2"
};
    
config.collection = {
"id": "users"
};
    
module.exports = config;  
```

5.  Now, we will be doing the connection with the `documentdb`. Here is the sample code. You can store it in a file named `app.js`:

```
var documentClient = require("documentdb").DocumentClient;
var config = require("./config");
var url = require('url');
    
var client = new documentClient(config.endpoint, { "masterKey":     
config.primaryKey });
    
var HttpStatusCodes = { NOTFOUND: 404 };
var databaseUrl = `dbs/${config.database.id}`;
var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;
    
/**
 * Get the database by ID, or create if it doesn't exist.
 * @param {string} database - The database to get or create
 */
function getDatabase() {
    console.log(`Getting database:n${config.database.id}n`);
    
    return new Promise((resolve, reject) => {
        client.readDatabase(databaseUrl, (err, result) => {
            if (err) {
                if (err.code == HttpStatusCodes.NOTFOUND) {
                    client.createDatabase(config.database, (err, created) => {
                        if (err) reject(err)
                        else resolve(created);
                    });
                } else {
                    reject(err);
                }
            } else {
                resolve(result);
            }
        });
    });
}
    
/**
 * Get the collection by ID, or create if it doesn't exist.
 */
function getCollection() {
    console.log(`Getting collection:n${config.collection.id}n`);

    return new Promise((resolve, reject) => {
        client.readCollection(collectionUrl, (err, result) => {
            if (err) {
                if (err.code == HttpStatusCodes.NOTFOUND) {
                    client.createCollection(databaseUrl, config.collection, {    
offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                } else {
                    reject(err);
                }
            } else {
                resolve(result);
            }
        });
    });
}
    
    
/**
 * Cleanup the database and collection on completion
 */
function cleanup() {
    console.log(`Cleaning up by deleting database ${config.database.id}`);
    
    return new Promise((resolve, reject) => {
        client.deleteDatabase(databaseUrl, (err) => {
            if (err) reject(err)
            else resolve(null);
        });
    });
}
    
/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}
    
getDatabase()
    .then(() => getCollection())
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });  
```

6.  You can run the code using the following command:

```
node app.js 
```

This code will connect to the database, get the collection details, and exit with error if any.

It covers the integration with DocumentDB using Node.js modules. Let's head over and see how we can integrate Node.js with the Table API.

# Integrating the Table API with Node.js

Azure table storage is a key-value-based NoSQL database with high performance. In order to integrate it with Node.js, we need to install the required module. Microsoft has provided official node modules to use the Table API ([https://www.npmjs.com/package/azure-storage](https://www.npmjs.com/package/azure-storage)):

1.  First, create the Table API in Azure portal and copy the connection string. Export it as the environment variable. In a Linux-based system, you can run this command to export a connection string as an environment variable:

```
export.AZURE_STORAGE_CONNECTION_STRING = <Your connection string>  
```

2.  Next, install the required dependency. Here is the command:

```
npm install azure-storage
```

3.  Once it is installed, you can include the module in your code base and start using the Table storage, for example:

```
var azure = require('azure-storage'); 
var tableObject = azure.createTableService(); 
tableObject.createTableIfNotExists('demoTable', function(error, result, response){ 
  if(!error){ 
    // Table exists or created 
  } 
  console.log(result); 
});  
```

This will connect to your Azure table storage using the connection string. Let's go ahead and integrate the Graph API with Node.js.

# Integrating the Graph API with Node.js

Cosmos DB supports graph using `gremlin` support, and `gremlin` is supported in Node.js for a long time.

We can install the `gremlin` node modules using the following command:

```
npm install gremlin@latest
```

Always lock the dependencies in production. 

You can create a new Graph API in Azure portal and copy the connection details. I am going to show the sample code to connect to that graph database.

We have installed the node module. Let's place our configuration details in one file:

```
var config = {}
    
config.endpoint = "graph api end point";
config.primaryKey = "primary key";
config.database = "database name"
config.collection = "collection name"
    
module.exports = config;
```

Now, we can connect to the Graph DB using the following code:

```
var Gremlin = require('gremlin');
var config = require("./config");
    
var client = Gremlin.createClient(
    443, 
    config.endpoint, 
    { 
        "session": false, 
        "ssl": true, 
        "user": `/dbs/${config.database}/colls/${config.collection}`,
        "password": config.primaryKey
    }
);
```

You can run the code using `node <filename.js>` and if the connection string is bad, it will throw an error and exit.

You can use `execute(query)` function to perform the queries in your graph database, for example:

This code piece will return the number of vertices present in the graph:

```
client.execute("g.V().count()", { }, (err, results) => {
  if (err) return console.error(err);
  console.log("Result: %sn", JSON.stringify(results));
});
```

Similarly, you can execute other queries as well.

This covers the Node.js integration with Graph DB as well.

# Summary

We have covered a lot in this chapter. We did the integration with the famous MongoDB using the existing code base to prove the point about migration or moving from traditional MongoDB to managed MongoDB without a single code change (except configuration URI of course).

We also learned about integrating Node.js with DocumentDB, Table, and Graph DB.

In the next chapter, we are going to build a high-performance link encoding and redirection service using Node.js and Cosmos DB. This is a similar to the `bit.ly` service.

# Building a High-Performance Link Shortening and Tracking System Using Cosmos DB

Cosmos DB is one of its kind to provide SLA on their services. Such a high performance system is required for applications which function in real time, such as group chat, live video calls, or link shortening services.

In this chapter, we are going to build a link shortening service using Cosmos DB. We will discuss which API to choose in the coming section. We will use Node.js for the application development.

In this chapter, we are going to learn the following:

*   Why do we need URL shortening?
*   Requirements of the system
*   Capacity estimation
*   Database design
*   Encoding URL algorithm
*   Node app development
*   Security and permission

 

# Why do we need URL shortening?

URL shortening is used to create shorter aliases for long URLs. Users are redirected to the original URL when they hit these short URLs. A shorter version of any URL would save a lot of space whenever we use it, for example, when printing or tweeting as tweets have a character limit.

For example, if we were to shorten this page through TinyURL:  
[https://packtpub.com](https://packtpub.com)

In this case, we would get [http://tinyurl.com/jlg8zpc](http://tinyurl.com/jlg8zpc).

The shortened URL is nearly 1/3 of the size of the actual URL.

URL shortening is used for optimizing links across devices, tracking individual links to analyze audience and campaign performance, and hiding affiliated original URLs, and so on.

If you haven't used [tinyurl.com](http://tinyurl.com/) before, please try creating a new shortened URL and spend some time going through different options their service offers. This will help you a lot in understanding this chapter better.

You can also use [bit.ly](https://bitly.com/) to understand the concept better.

Let's look over the requirements imposed by the system.

# Requirements of the system

Our URL shortener system should meet the specific requirements.

The following are functional requirements:

*   Given an URL, our service should generate a shorter and unique alias of it
*   When users access a shorter URL, our service should redirect them to the original link

The following are non-functional requirements:

*   The system should be highly available. This is required because, if our service is down, all the URL redirections will start failing.
*   URL redirection should happen in real-time with minimum latency.
*   Shortened links should not be guessable (or predictable).

The following are extended requirements:

*   Analytics, for example, how many times a redirection happened?
*   Our service should also be accessible through REST APIs by other services

Let's calculate the size of the database and bandwidth required by the system to go live.

# Capacity estimation

Our system would be read-heavy; there would be lots of redirection requests compared to new URL shortenings. Let's assume a 100:1 ratio between read and write.

*   **Traffic estimates**: If we assume that we would have *500 Million* new URL shortenings per month, we can expect (*100 \* 500 Million => 50 Billion*) redirections during the same time. What would be **Queries Per Second** (**QPS**) for our system?

New URL shortenings per second:

*500 million / (30 days \* 24 hours \* 3600 seconds) ~= 200 URLs/s* 

URL redirections per second:

*50 billion / (30 days \* 24 hours \* 3600 sec) ~= 19K/s* 

*   **Storage estimates**: Since we expect to have 500M new URLs every month, if we were to keep these objects for five years, the total number of objects we would be storing would be 30 billion:

*500 million \* 5 years \* 12 months = 30 billion* 

Let's assume that each object we are storing can be of 500 bytes (just a ballpark figure, we will dig into it later); we would need 15 TB of total storage:

*30 billion \* 500 bytes = 15 TB* 

*   **Bandwidth estimates**: For write requests, since every second we expect 200 new URLs, the total incoming data for our service would be 100 KB per second:

*200 \* 500 bytes = 100 KB/s* 

For read requests, since every second we expect ~19K URLs redirections, the total outgoing data for our service would be 9 MB per second:

*19K \* 500 bytes ~= 9 MB/s* 

*   **Memory estimates**: If we want to cache some of the hot URLs that are frequently accessed, how much memory would we need to store them? If we follow the 80-20 rule, meaning 20% of URLs generating 80% of traffic, we would like to cache this 20% of hot URLs.

Since we have 19K requests per second, we would be getting 1.7 billion requests per day:

*19K \* 3600 seconds \* 24 hours ~= 1.7 billion* 

To cache 20% of these requests, we would need 170 GB of memory:

*0.2 \* 1.7 billion \* 500 bytes ~= 170 GB* 

# Database design

A few observations about the nature of the data we are going to store:

*   We need to store billions of records
*   Each object we are going to store is small (less than 1K)
*   There are no relationships between records, except if we want to store which user created what URL
*   Our service is read-heavy

We would need two tables, one to store the URL and one to store the count of the link.

The following represents the `URL` table:

 
| `\_id` |
| --- |
| `long\_url` |
| `clicked` |
| `created\_date` |

 

The following represents the `Counter` table:

 
| `\_id` |
| --- |
| `Seq` |

# Encoding URL algorithms

This is the interesting part where the actual URL encoding and decoding happens.

Before we jump into the code development, let's explore the basic theory behind URL shortening. Our requirement is as follows:

Given a long URL such as [http://stackoverflow.com/questions/tagged/node.js](http://stackoverflow.com/questions/tagged/node.js), generate a unique URL that is shorter, such as `localhost:3000/3Ys`.

First, let's look at actually generating a unique key from our original URL, in this case 3 years, which is at the end of our localhost URL.

To satisfy the uniqueness requirement, we can use a special class of functions called bijective ([https://en.wikipedia.org/wiki/Bijection](https://en.wikipedia.org/wiki/Bijection)) functions, which guarantee a 1-to-1 mapping. For explanation purposes, a bijective function basically says:

"A decoded URL is mapped to exactly one key, and a key is mapped to exactly one decoded URL" 

We are going to use a base58 algorithm to fulfil our URL shortening and decoding task.

We are going to convert a unique integer ID (which is in base10) to its equivalent in base58. The base58 alphabet we will be using is as follows:

```
123456789abcdefghijkmnopqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ 
```

It is just the numbers 1-9, a-z, and A-Z, giving us a total of 58 characters, hence the 58 in base58. We are not including 0, l, or O to avoid confusion when sharing the URL over the phone or copying it manually.

Now, to put it all together to generate a shorter URL, our solution will be as follows:

1.  Create a global auto incremented integer
2.  Every time a new URL is shortened and added to our database, we'll increment that global number (base 10) and use it as our unique ID for that entry in the DB
3.  Then, we will base58 encode that unique ID to generate a unique, shorter URL

For example, an entry with the unique ID 100022 (base 10) will result in a base58 encoding of 3Ys. So if you store a lot of URLs, say 100,000,000, they would generate a shortened hash:

```
9QwvW
```

Let's develop the code!

# Node app development

So, we have everything set out; let's build the app.

Let's create our `package.json` file.

Always use the `npm init --y` command to generate boilerplate `package.json` file. 

Here are the packages we require:

*   `body-parser`
*   `express`
*   `mongoose`

Let's install them. Run this command under your project directory to install the dependencies:

```
npm install --save body-parser express mongoose 
```

This is what my `package.json` looks like:

```
{ 
  "name": "url-shortener", 
  "version": "1.0.0", 
  "description": "A NodeJS + Express + MongoDB based URL shortener", 
  "main": "app.js", 
  "scripts": { 
    "test": "echo "Error: no test specified" && exit 1" 
  }, 
  "author": "shahid", 
  "license": "MIT", 
  "dependencies": { 
    "body-parser": "^1.14.1", 
    "express": "^4.13.3", 
    "mongoose": "4.2.9" 
  } 
} 
```

Let's create the configuration file. Name it `config.js` and place the code, shown as follows:

```
var config = {}; 
config.db = {}; 
config.webhost = 'http://localhost:3000/'; 
config.db.uri = 'copy the uri from Azure portal'; 
config.db.host = 'packt-mongodb-2.documents.azure.com'; 
config.db.name = 'url_shortener'; 
module.exports = config; 
```

Copy the URI from the Azure portal screen. Refer to the following screenshot for reference:

![](assets/93a8f365-1b81-4999-ab51-4c3d35829793.png)

We have done the configuration. Let's write the code to connect to our MongoDB database of Cosmos.

Create a file called `connection.js` and place the following code in it:

```
const mongoose = require('mongoose'); 
const config = require('../config.js'); 
mongoose.connect(config.db.uri); 
 
module.exports = mongoose.connection; 
```

This code will return the connection object to the caller.

Next, we will create database schema codebase which is to ensure the MongoDB schema and data indexing.

Here is the codebase for the same. Place this in the file named `url.js`:

```
var mongoose = require('mongoose'); 
var connection = require('./connection'); 
var Schema = mongoose.Schema; 
 
connection.once('open',function() { 
  console.log("Connected...."); 
}); 
 
var CounterSchema = Schema({ 
    _id: {type: String, required: true}, 
    seq: { type: Number, default: 0 } 
}); 
 
var counter = mongoose.model('counter', CounterSchema); 
 
// create a schema for our links 
var urlSchema = new Schema({ 
  _id: {type: Number, index: true}, 
  long_url: String, 
  clicked: {type: Number, default:0 ,index: true}, 
  created_at: Date 
}); 
 
urlSchema.pre('save', function(next){ 
  var doc = this; 
  counter.findByIdAndUpdate({_id: 'url_count'}, {$inc: {seq: 1}},    function(error, counter) { 
      if (error) 
          return next(error); 
          console.log(counter); 
      doc.created_at = new Date(); 
      doc._id = counter.seq; 
      next(); 
  }); 
}); 
 
var Url = mongoose.model('Url', urlSchema); 
 
module.exports = Url; 
```

As you can see in the preceding code, we are using our connection object and printing the status of the connection on the console.

As we discussed in the database section, we have created two schemas. One is to maintain the global counter and the other is to maintain the actual link.

We are using the pre-function event of Mongoose to basically get the latest count of the URL value from the counter collection. This is to make sure that we never generate duplicate short URL.

At the end, we are exporting the database object.

Next, we will develop our application logic, that is, server code to handle the request and response from the user and call functions which we developed.

Here is the code base for the server. We are using the express node module to create server routes:

```
var express = require('express'); 
var app = express(); 
var path = require('path'); 
var bodyParser = require('body-parser'); 
var mongoose = require('mongoose'); 
var config = require('./config'); 
var base58 = require('./base58.js'); 
 
// grab the url model 
var Url = require('./models/url'); 
 
app.use(bodyParser.json()); 
app.use(bodyParser.urlencoded({ extended: true })); 
 
app.use(express.static(path.join(__dirname, 'public'))); 
 
app.get('/', function(req, res){ 
  res.sendFile(path.join(__dirname, 'views/index.html')); 
}); 
 
app.post('/api/shorten', function(req, res){ 
  var longUrl = req.body.url; 
  var shortUrl = ''; 
 
  // check if url already exists in database 
  Url.findOne({long_url: longUrl}, function (err, doc){ 
    if (doc){ 
      shortUrl = config.webhost + base58.encode(doc._id); 
 
      // the document exists, so we return it without creating a new entry 
      res.send({'shortUrl': shortUrl}); 
    } else { 
      // since it doesn't exist, let's go ahead and create it: 
      var newUrl = Url({ 
        long_url: longUrl 
      }); 
 
      // save the new link 
      newUrl.save(function(err) { 
        if (err){ 
          console.log(err); 
        } 
 
        shortUrl = config.webhost + base58.encode(newUrl._id); 
 
        res.send({'shortUrl': shortUrl}); 
      }); 
    } 
 
  }); 
 
}); 
 
app.get('/:encoded_id', function(req, res){ 
 
  var base58Id = req.params.encoded_id; 
 
  var id = base58.decode(base58Id); 
 
  // check if url already exists in database 
  Url.findOne({_id: id}, function (err, doc){ 
    if (doc) { 
      res.redirect(doc.long_url); 
    } else { 
      res.redirect(config.webhost); 
    } 
  }); 
 
}); 
 
var server = app.listen(3000, function(){ 
  console.log('Server listening on port 3000'); 
}); 
```

First, we need the module we require, such as express for web server or body parser to parse HTTP parameter values.

Next, we need three modules. Refer to the following code:

```
// our sweet config file. 
var config = require('./config'); 
// Base 58 decoding, will discuss it in next section 
var base58 = require('./base58.js'); 
// grab the url model 
var Url = require('./models/url'); 
```

The URL module actually stores the URL and their count as we discussed in the preceding section.

Next, we defined routes and hit upon those routes doing certain actions. For example, on a root page request, we send the HTML content back to the server.

Then, we defined two APIs, one to shorten the URL and another to decode it. Both of these routes call the model function present in the URI module.

Whether it's encoding or decoding, we are using the base58 module function. Let's see what that module does.

Here is the code of the `base58.js` module:

```
var alphabet = "123456789abcdefghijkmnopqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ"; 
var base = alphabet.length; 
 
function encode(num){ 
  var encoded = ''; 
  while (num){ 
    var remainder = num % base; 
    num = Math.floor(num / base); 
    encoded = alphabet[remainder].toString() + encoded; 
  } 
  return encoded; 
} 
 
function decode(str){ 
  var decoded = 0; 
  while (str){ 
    var index = alphabet.indexOf(str[0]); 
    var power = str.length - 1; 
    decoded += index * (Math.pow(base, power)); 
    str = str.substring(1); 
  } 
  return decoded; 
} 
 
module.exports.encode = encode; 
module.exports.decode = decode; 
```

Here, we are doing actual encoding and decoding of the URL. This function encodes the string to the specified code and, once we hit the code, it returns the exact same URL back. This function and module is the heart of our code base.

Let's look at the user front facing application code, that is, our HTML code. Here is the `index.html` file and its code:

```
<!DOCTYPE html> 
<html> 
<head> 
  <meta charset="utf-8"> 
  <meta http-equiv="X-UA-Compatible" content="IE=edge"> 
  <meta name="viewport" content="width=device-width, initial-scale=1"> 
 
  <title>URL Shortener - coligo.io</title> 
  <link href='https://fonts.googleapis.com/css?family=Raleway' rel='stylesheet' type='text/css'> 
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css"> 
  <link href="css/styles.css" rel="stylesheet"> 
</head> 
<body> 
 
  <div class="site-wrapper"> 
 
    <div class="site-wrapper-inner"> 
 
      <div class="main-container"> 
 
        <div class="inner cover"> 
          <span class="glyphicon glyphicon-link"></span> 
          <h1>URL Shortener</h1> 
           
          <div class="row"> 
 
            <div class="col-lg-12"> 
              <div class="input-group input-group-lg"> 
                <input id="url-field" type="text" class="form-control" placeholder="Paste a link..."> 
                <span class="input-group-btn"> 
                  <button class="btn btn-shorten" type="button">SHORTEN</button> 
                </span> 
              </div> 
            </div> 
 
            <div class="col-lg-12"> 
              <div id="link"></div> 
            </div> 
 
          </div> 
 
        </div> 
 
      </div> 
 
    </div> 
 
  </div> 
 
  <script src="https://code.jquery.com/jquery-2.2.0.min.js"></script> 
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script> 
  <script src="javascripts/shorten.js"></script> 
</body> 
</html> 
```

We have done our code; let's test run it.

Run the following command in the terminal to start the application:

```
node app.js 
```

It should print the following in the console:

![](assets/bb3a0c9c-d2b4-4553-a5c7-59b664c7aa2e.png)

Now, open up the browser and hit `localhost:3000` (replace `localhost` with URL if deployed on server). You should see a similar screen to the one shown in the following screenshot:

![](assets/4ae6daa1-a2ad-482a-9bcf-e219b9888c6c.png)

Now, add an URL and hit the SHORTEN button. You should receive the encoded link. Refer to the following screenshot for reference:

![](assets/c59f4e86-c333-4bfe-93f8-547815afdeec.png)

Click on that URL and it will take you to the destination URL.

Let's check out the database entries. Open up Azure portal and go to the Cosmos DB MongoDB collection. You should see the entries over there similar to what is shown in the following screenshot:

![](assets/dbcd0367-36e7-40ff-96b9-2a3061747456.png)

Add more URLs to see more entries and diverse encoding in the IDs.

Let's look over security and permission.

# Security and permission

We can store permission levels (public/private) with each URL in the database. We can also create a separate table to store UserIDs that have permission to see a specific URL.

If a user does not have permission and tries to access an URL, we can send an error (HTTP 401) back.

Given that, we are storing our data in a NoSQL wide-column database like Cassandra, the key for the table storing permissions would be the Hash (or the KGS generated key), and the columns will store the UserIDs of those users that have permissions to see the URL.

# Summary

We have learned how to develop an efficient, real-time web application using the Node.js and MongoDB API of Cosmos DB. We have learned about the requirement gathering, functional design, and various inputs of the system.

We have developed the complete application using Node.js and learned it step by step. At the end, we had a test drive to see if it works, which it did :)

In the next chapter, we are going to dive deeper into technical complexity and build a real-time email tracking system using Cosmos DB.

# Building a Highly Available, Real-Time Email Tracking System using Cosmos

Have you ever used email marketing software such as Mailchimp or Mailerlite? If you have, then you will already be familiar with their email tracking feature. This feature allows you to keep track of an email's open count and to check whether a particular user has clicked on an email, which is great for any business.

If you are not used to such marketing software, then allow me to introduce you to it. Email marketing software allows you to send a huge number of emails—say, millions at a time—and track how many people have opened it and how many clicked on the button which leads to them buying your product. Some software even tracks your sales data, which is mind blowing.

Let me explain this to you with a scenario which will demonstrate how important it is; imagine that this book got released and the publisher, Packt, sent millions of its readers an email saying:

*"We have launched a book! Grab it with 30% discount"*

Now, there is a system which tells you that you sent a million emails and around 100K people opened it, of who 20K clicked on the button; Packt can then easily tell that they got sales of, say, 2K books as a result of that email.

So, there is some math that you can do to figure out the conversion rate! Pretty amazing, right?

Well then, let's build one and see how it works.

Welcome to the second to last chapter of this book. I am super pumped to get you through this chapter, as this is (in my opinion) one of the most interesting chapters in this book.

In this chapter, we are going to cover the following topics:

*   Is email tracking legal?
*   System design and assumptions
*   Choosing the database API in Cosmos DB
*   Writing our email sender
*   Tracking emails and storing tracking data
*   What's next? Maybe machine learning

OK, I hope you are interested in the topics so far. Let's jump in.

# Is email tracking legal?

Is email tracking legal? In a word: **NO**.

However, you can legally track a user who has given you permission to do so. Have you heard about **Privacy policy** and **Terms of Service** pages on websites? These pages usually declare that they will use your data to give you features like email tracking, sales analysis, and so on.

So, yes, email tracking is legal if users have given permission or if the platform owner has given a public declaration. Use it at your own risk.

Well, fun apart, there is no risk.

First, let me explain this to you in detail.

In general, there are two types of email on the internet:

*   Transactional emails
*   Mass emails

Transactional emails are things like passwords, credit card receipts, and confirmations (such as Hillary Clinton's emails), which are very confidential and must not be tracked. A delivery report from an SMTP server that the email is delivered is all that matters here.

Second comes mass email, such as marketing emails, new product launch announcements, new feature launch announcements, and so on. It's these kinds of email which are targeted to a wide range of audiences and on which senders are spending a lot of money. As the sender of such emails, I must know how they perform. This is where email tracking comes into the picture.

Ok, let's see how this thing is going to work. Design time now!

# System design and assumptions

There are basically two parts of the email system:

*   Email sender
*   Email tracker

I am going to show you, in a small block of code, how to send an email using Node.js with the tracking code inside the email. However, it won't be production grade (this is our assumption).

The email tracker, however, will be a production-grade design; this tracker will receive the event from the email and update the database accordingly. We will discuss which database API to use and everything else in the upcoming section.

Now, for the sake of design, here is a diagram:

![](assets/2b17a425-f0ca-43f6-9f69-e32d0af43b49.png)

As you can see in the preceding diagram, the email sender will pick up the record from some source which will contain an email list and send out emails to lots of people.

When people open, click on, and read those emails, the email tracker will track those events and record them in the database.

Now, system design is not just the diagram design; it has lot of things within it, such as capacity estimation, database diagrams, and so on.

We are not going to estimate email capacity, as we did in the last chapter, because I want to keep this chapter short and concise.

So, let's look over the assumptions:

*   The email sender code will contain static email addresses
*   The mail sender code will have no data source connection
*   The email sender will send a simple email with a tracking code
*   The email tracker will receive events
*   The email tracker will try to fetch as much information as it can from that event
*   The email tracker will add those events to the database

Ok, I hope the assumptions are clear. Let's go ahead and choose the database API.

# Choosing the database API in Cosmos DB

As you already know, at the time of writing, Cosmos DB provides four database APIs and we can choose or combine them together, as per our need. Right now, we have a requirement for the database which can write the data in an efficient way because, in email tracking, we are going to update our records frequently.

Our requirement for the read operation is not that huge: the tracking data can be easily served to the user or transferred to a data analytics engine.

So, here is the thing: if we had to manually install or choose the database, then these metrics would come into the picture. Cosmos DB is a managed database service with SLA of their consistency and latency, hence, any database API which your application can easily integrate with should work here as well.

I would prefer to go with MongoDB API support because of the following two reasons:

*   It is JSON-based, and can easily synchronize with the analytics engine
*   It offers native support in Node.js, and is easy to program and integrate

So yes, we are going to use MongoDB API in Cosmos DB to track and record our emails.

Let's dive into the code and write our simple email sender.

# Writing our email sender

In practice, the email sender is one of the most complex pieces of our system. I have actually designed an email sender using a queue system to send a huge volume of emails (more than a billion a month) and that is one of the reasons why I am not covering it completely: because it's really very complex.

However, for the sake of an explanation and simplicity, I have designed a simple process diagram of the email sender. Have a look:

![](assets/4887d324-a8d1-4a07-b011-113cfc67cd5b.png)

Allow me to explain how this works. Queuing is one of the core designs of a scalable distributed system. The queue provides your system with the distribution and communication between your processes.

In our design, I used the queue (message ordering could be your choice, I used **First in First out** (**FIFO**) to handle the vast amount of email messages; imagine the volume in the billions and you just can't do a query to a database for that every single time you want to shoot over some emails).

So, the producer here fetches the email records from the database, from an API call, or from any **remote procedure calls** (**RPC**) call.

The producer takes those emails, forms an email for each user, and adds a tracking code into each of the emails.

Now, hold on a sec, and let me explain what a tracking code is.

We need a piece of code which should be executed when a user opens up the email. Now, we can't put any JavaScript code in the email (even a low-priced email server would throw you in the soft bounce section), so how do we make sure we receive the event data?

Here is the magic part.

Have you heard about the image tag in HTML?

Yes, this one:

```
<img src='your photo url.jpeg' height='1200' width='800'> 
```

It can give you tracking info. Now wait, don't go crazy. Let me explain... again!

When a browser or an email client loads your email, it downloads the images from the server.

It makes an `HTTP GET` call to the path provided in the source of the image.

Now, what if we replace the path to an actual image with the path to our custom API? When the API gets hit, we know someone has opened up an email. We can include additional information in the URL, such as subscriber ID, email ID, and so on, allowing us to gather more information.

Pretty amazing, right?

Now, we already know how to create a link. We just made an URL shortener in the last chapter. Whenever the shortener decodes any email, you know that the link has been clicked.

Okay, enough of the producer. Once the producer generates the emails, it pushes those messages into the queue and its job is done.

Now, the queue stores those messages and delivers them to its consumers. Here comes the fun part.

In the diagram, I have shown one consumer receiving the message from the queue. In reality, you can send an email to multiple consumers at the same time.

*I know what you're thinking; none of the consumers will get the same message.* 

So let's say, you have 10 consumers at the queue end, you can configure the number of the messages they can receive at one particular time. You can send, say, 100 messages at a time.

So, if we send 100 messages to 10 consumers at a time, that is 1,000 messages at a time. Consumers will then push those 1,000 messages to the SMTP server to deliver them to your inboxes. This process goes on until the queue is empty, which it never will be in a production grade system.

I can't say with certainty, but this is, or is similar, to what every major piece email marketing software is using.

Okay, code time. Let's build a simple yet efficient email sender using Node.js.

First, create a new directory and run this command inside the directory to create your fresh Node.js project:

```
mkdir dirname && npm init --y 
```

Replace `dirname` with your folder name.

Once the project is created, you need to install the dependencies of that project. We are going to send emails, so of course we need a module which can handle that. We also need to handle the asynchronous flow of the node, so install a module for that too.

Run this command to install these two dependencies:

```
npm install --save nodemailer async 
```

As the name suggests, `nodemailer` is used for sending out an email and `async` will be used to handle flow control.

Let's jump in and code our server.

Here is the code for the emailer. I will explain it function by function:

```
const async = require("async");
const nodemailer = require("nodemailer"); 
// can be your email list from DB 
const listofemails = ["rwtc66@gmail.com","shahid@codeforgeek.com"]; 
const emailContent = "Hello World, this is Shahid Shaikh! <br>"; 
 
var success_email = []; 
var failure_email = []; 
 
class Mailer { 
    constructor() {       
      this.transporter = nodemailer.createTransport("SMTP",{ 
            host: 'smtp.gmail.com', 
            port: 587, 
            auth: { 
              user: '', 
              pass: '' 
           }, 
            tls: {rejectUnauthorized: false}, 
            debug:true 
          }); 
      this.invokeOperation(); 
    } 
 
    /* Invoking email sending operation at once */ 
 
    invokeOperation() { 
        var self = this; 
        async.each(listofemails,self.sendEmail,function(){ 
            console.log(success_email); 
            console.log(failure_email); 
        }); 
    } 
 
    sendEmail(Email,callback) { 
        console.log("Sending email to " + Email); 
        var self = this; 
        self.status = false; 
        async.waterfall([ 
            function(callback) { 
                var mailOptions = { 
                    from: 'shahid@codeforgeek.com', 
                    to: Email, 
                    subject: 'Hi ! This is from Async Script', 
                    text: emailContent + "<img src='http://domain.com/api/track/?email='+to>" 
                }; 
                transporter.sendMail(mailOptions, function(error, info) { 
                    if(error) { 
                        console.log(error) 
                        failure_email.push(Email); 
                    } else { 
                        self.status = true; 
                        success_email.push(Email); 
                    } 
                    callback(null,self.status,Email); 
                }); 
            }, 
            function(statusCode,Email,callback) { 
                    console.log("Will update DB here for " + Email + "With " + statusCode); 
                    callback(); 
            } 
            ],function(){ 
                //When everything is done return back to caller. 
                callback(); 
        }); 
    }     
} 
 
new Mailer(); //lets begin 
```

First, we declared our dependencies and also declared a few variables. We stored an email list in an array, which can be replaced with emails from the database or an API call.

We also declared two separate arrays to hold the successfully sent emails and rejected emails, respectively.

Next, we created a class in the constructor, creating our SMTP transport, using it to send a huge number of emails.

We created a Gmail SMTP and, by using Google email credentials, we are going to send the emails.

Google SMTP is just for demonstration purposes; you must use a fully-fledged SMTP server in production.

After the creation of the SMTP transport, we invoke our operation and, using that, we are going to perform a parallel iteration to the emails. The `async.each` function would call the function a number of times and then email count.

Now, hold on: what if the email count is in the millions? Does it create a million parallel processes?

Well yes, but it should not.

So, we can add a limit—say, 1,000 parallel processes—in the `async.each` function; iteration would then be limited.

Finally, in the send email function, we send the email to the destination coming from the list and store the result of that action in the respected array depending upon the condition.

This is by far the simplest email sender you will ever see. Trust me on that!

We invoke the program using the traditional `new` operator. You can extend this program to meet your production system needs with a few changes here and there, but it will work.

Now, regarding tracking, you will see in the email sending part that we are appending an image. Here is the code for the reference:

```
emailContent + "<img src='http://domain.com/api/track/?email='+to>" 
```

Here, the `src` attribute of the image is our API URL. You can replace the domain name with your app. We add the email ID to the URL to identify which user has done it.

Let's go ahead and see how we can track those emails.

# Tracking emails and storing tracking data

So far, we have discussed the architecture of our email sender and how we can build a scalable system which can send emails in large volumes.

Now, it's time to track those emails. Let's build and code a program that can track the email open rate, location, IP, device, that is, computer, laptop or phone, and agent, that is, browser.

We are also going to insert that data in the MongoDB database or Cosmos DB.

In the last section, we added the tracking code, which was an image tag using our API call as its source, into the email. Our API call is going to return the image.

Let's develop a sample code which will do the same thing.

Initiate a new Node.js project and install the following dependency:

```
npm i --S express 
```

Here is the sample code of the API:

```
const express = require('express'); 
const app = express(); 
 
app.get('/api/track',function(req,res) { 
   console.log('tracking code'+JSON.stringify(req.query)); 
   // sending the image back. 
   var buf = new Buffer([ 
      0x47, 0x49, 0x46, 0x38, 0x39, 0x61, 0x01, 0x00, 0x01, 0x00, 
      0x80, 0x00, 0x00, 0xff, 0xff, 0xff, 0x00, 0x00, 0x00, 0x2c, 
      0x00, 0x00, 0x00, 0x00, 0x01, 0x00, 0x01, 0x00, 0x00, 0x02, 
      0x02, 0x44, 0x01, 0x00, 0x3b 
  ]); 
  res.set('Content-Type', 'image/png'); 
  res.end(buf,'binary'); 
}); 
 
app.listen(3000); 
console.log("Started at 3000"); 
```

As you can see in the preceding code, we have exposed the GET API on the same path, that is, `/api/track`, and have returned the image.

Don't be afraid of the buffer: it is just a 1 x 1 image binary code. Basically, we need to send the image back to the browser, so that browser doesn't give a 404 error for the image. This image should be transparent and as small as possible, so as not to conflict with the actual email.

Now, let's try to extract the tracking information from the IP and HTTP request.

First, we need the IP.

You can get the IP from the request header. It can be in the following keys, depending upon the network, so we are adding all of them to the exception. See the following code:

```
let ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress || req.socket.remoteAddress ||req.connection.socket.remoteAddress; 
```

Next, we need the device type, location, and agent. We need to install an extra dependency for that. Run the following command to install those dependencies:

```
npm i --S geoip-lite express-device useragent 
```

Let's declare them in the code:

```
const geoip = require('geoip-lite'); 
const device = require('express-device'); 
const useragent = require('useragent')(true); 
```

Add the user agent middleware in the express, so that it captures every request:

```
app.use(device.capture({parseUserAgent: true})); 
```

We can extract the information from the request header, using the following code:

```
var agent = useragent.parse(req.headers['user-agent']); 
var ua = useragent.is(req.headers['user-agent']) 
var geoLocation = geoip.lookup(ip); 
```

We can extract the device from the request object using the `req.device` key. Refer to the complete code, as follows:

```
const express = require('express'); 
const geoip = require('geoip-lite'); 
const device = require('express-device'); 
var useragent = require('useragent')(true); 
const app = express(); 
 
app.use(device.capture({parseUserAgent: true})); 
app.enable('trust proxy'); 
 
app.get('/api/track',function(req,res) { 
   console.log('tracking code'+JSON.stringify(req.query)); 
   let ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress || req.socket.remoteAddress ||req.connection.socket.remoteAddress; 
   var agent = useragent.parse(req.headers['user-agent']); 
   var ua = useragent.is(req.headers['user-agent']) 
   let geoLocation = geoip.lookup(ip); 
   var buf = new Buffer([ 
      0x47, 0x49, 0x46, 0x38, 0x39, 0x61, 0x01, 0x00, 0x01, 0x00, 
      0x80, 0x00, 0x00, 0xff, 0xff, 0xff, 0x00, 0x00, 0x00, 0x2c, 
      0x00, 0x00, 0x00, 0x00, 0x01, 0x00, 0x01, 0x00, 0x00, 0x02, 
      0x02, 0x44, 0x01, 0x00, 0x3b 
  ]); 
  res.set('Content-Type', 'image/png'); 
  res.end(buf,'binary'); 
  let trackData = { 
      'trackingCode': JSON.stringify(req.query), 
      'ip': ip, 
      'deviceType': req.device.type, 
      'deviceName': req.device.name, 
      'device': req.device, 
      'agent': agent, 
      'location': geoLocation 
  }; 
  console.log(trackData); 
}); 
 
app.listen(3000); 
console.log("Started at 3000"); 
```

Now, whenever someone opens up the email, this API will be triggered and you will receive all the information required for tracking.

Let's see if it really works. Run the code and try to hit this API from the browser. If you are running it from your machine, then it will return `localhost:3000/api/track?code=1234456` and in terminal, something like this will be printed:

![](assets/8feff6da-e151-4001-a90b-ba1e857a2e82.png)

As you can see, we receive the information about device type, browser, and IP. Since I was running it in my local machine, the location retrieval was disabled. It works well on the server.

In the browser, you should see something similar to this:

![](assets/e7be8cff-5247-4f86-8263-3529a3fcfbfb.png)

That's the 1 x 1 image we returned from our code.

So, the tracking code works; what we need now is to add it to the database. As discussed, we are going to use the MongoDB API of Cosmos DB.

So, in order to use that, we first need to create the database. Go to the Azure portal and choose Cosmos DB from the left corner and create a new MongoDB API database and a new collection.

Once it is created, copy the access keys from this screen:

![](assets/69b98c74-4ff9-4f1e-9a7f-4578c338c402.png)

It's time to add the code!

First, install the dependency. Run the following command in the project directory:

```
npm i --S mongoose 
```

Create the configuration file and add this piece of code:

```
var config = {}; 
config.db = {}; 
config.db.uri = 'mongodb://packt-mongodb-2:lTsYGMpczByW0VB7kxCoAPZdSAKdDsJ4GVqWlvLWuVBDlX9XrPAXBwaHOLQWk9HWM5VtRDKOubqE6WN2U3b2ww==@packt-mongodb-2.documents.azure.com:10255/?ssl=true&replicaSet=globaldb'; 
config.db.host = 'packt-mongodb-2.documents.azure.com'; 
config.db.name = tracking; 
module.exports = config; 
```

Make sure you change the values to match your access keys.

Let's make the connection. Create a file, name it `connection.js`, and add the following code:

```
const mongoose = require('mongoose'); 
const config = require('./config.js'); 
mongoose.connect(config.db.uri); 
module.exports = mongoose.connection; 
```

Now, let us create the model code. To do so, create a new file, name it `trackmodel.js`, and place this code in it:

```
var mongoose = require('mongoose'); 
var connection = require('./connection'); 
var Schema = mongoose.Schema; 
 
connection.once('open',function() { 
  console.log("Connected...."); 
}); 
 
// create a schema for our links 
var trackSchema = new Schema({ 
  data: Array, 
  created_at: Date 
}); 
 
module.exports = mongoose.model('track', trackSchema); 
```

Let's add the model code to our application code and start saving the information in our collection.

Replace the collection name with the one you created at your end.

Add the following code to the dependency declaration:

```
var model = require('./trackModel'); 
```

Now, in the router, add this code at the end of the existing file:

```
var trackInfo = model({data: trackData}) 
trackInfo.save(function(err) { 
      if (err) { 
          console.log(err); 
      } 
}) 
```

Here is the complete code, for your reference:

```
const express = require('express'); 
const geoip = require('geoip-lite'); 
const device = require('express-device'); 
var useragent = require('useragent'); 
useragent(true); 
var model = require('./trackModel'); 
const app = express(); 
 
app.use(device.capture({parseUserAgent: true})); 
app.enable('trust proxy'); 
 
app.get('/api/track',function(req,res) { 
   console.log('tracking code'+JSON.stringify(req.query)); 
   let ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress || req.socket.remoteAddress ||req.connection.socket.remoteAddress; 
   var agent = useragent.parse(req.headers['user-agent']); 
   var ua = useragent.is(req.headers['user-agent']) 
   let geoLocation = geoip.lookup(ip); 
   var buf = new Buffer([ 
      0x47, 0x49, 0x46, 0x38, 0x39, 0x61, 0x01, 0x00, 0x01, 0x00, 
      0x80, 0x00, 0x00, 0xff, 0xff, 0xff, 0x00, 0x00, 0x00, 0x2c, 
      0x00, 0x00, 0x00, 0x00, 0x01, 0x00, 0x01, 0x00, 0x00, 0x02, 
      0x02, 0x44, 0x01, 0x00, 0x3b 
  ]); 
  res.set('Content-Type', 'image/png'); 
  res.end(buf,'binary'); 
  let trackData = { 
    'trackingCode': JSON.stringify(req.query), 
    'ip': ip, 
    'deviceType': req.device.type, 
    'deviceName': req.device.name, 
    'device': req.device, 
    'agent': agent, 
    'location': geoLocation 
  }; 
  var trackInfo = model({data: trackData}) 
  trackInfo.save(function(err) { 
      if (err){ 
        console.log(err); 
      } 
  }) 
  console.log(trackData); 
}); 
 
app.listen(3000); 
console.log("Started at 3000"); 
```

Now, run the code and hit the same API, that is, `http://localhost:3000/api/track?code=1234456`. You should now see the data in your Azure collection.

Congratulations! Now you can send any number of emails, add that tracking code to them, track the data, and store it using the code that we have developed.

I hope you have gained some good insights about the email tracking use case in this chapter.

# Summary

In this chapter, we have, of course, learned things related to email marketing! We learned how we can design a scalable email sender, which can send a huge number of emails.

We also looked at email tracking and figured out a way to track the emails that we send. We wrote the code to extract the tracking information, such as device, location, open rate, and so on. We then integrated the Cosmos DB MongoDB API to store that tracking information.

In the next chapter, we are going to learn more about the deployment part of the Cosmos DB and the best practices for developing a Cosmos DB application.

# Cosmos DB Deployment and Management

So far, we have designed, developed, and tested an application built on top of a Cosmos DB and Azure infrastructure. The time has come to deploy it and let the world know about your new app!

In this chapter, we are going to learn about the few deployment concepts of Cosmos DB.

We will also cover the following topics:

*   Turnkey distribution
*   Failover handling
*   Database deployment
*   Backup and restore
*   Deploying a custom app over the Azure network
*   Monitoring the traffic and usage

You can email me at `shahid@codeforgeek.com` or tweet me at `@codeforgeek`.

# Turnkey distribution

Before you say, "*Shahid, we have learned this in previous chapters, why do it again now?*", I want to hold you up there. I know we did cover this topic in our previous chapters, but I would like to revisit it, because it's really important for your application deployment plan.

If you are a DevOps, you can thank me later (they know what I mean).

Cosmos DB provides a competitive feature, which none of the other managed database providers, such as Amazon, Rackspace, and so on, offer, at least at the time of writing. Hence, it becomes a very important selling point of Cosmos DB.

Let's revisit it again and see how it can help your deployment plan. So, you can replicate your data across more than 30 regions on the planet with just a few clicks of a button (not really one click, but very simple). That's what turnkey global distribution means.

So, a possible use case in the deployment plan could be as follows. Suppose you have an SaaS-based application and you have a huge customer base in Germany, but your application is deployed at a New York data center. Now, based on the data transfer rate, you can spin up one read region in Germany (if they provide one there) or near to Germany to facilitate those customers in a much better way.

Mainly, the replication is meant for the failover conditions; if some node fails in a particular region, then another node with the same data takes over and customers never know that anything has changed.

For example, refer to the following screenshot from [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution System:*

![](assets/b09775c4-f8db-4fa4-a1f5-b255c775f374.png)

Here, my write region is based in Southeast Asia and I can pick and choose any read region from across the globe. There are tons of data centers in the states, very few in Russia, and none in Africa (which isn't fair; Africa is blooming in IT).

All you need to do is pick a region and see the magic happen; you get your replication done.

With this mention of replication, let's dive into the next important topic of deployment, that is, failover handling.

# Failover handling

When something fails, what's going to happen?

This is the key question of any deployment, because it matters. Consider applications like WhatsApp; if they go down, even for a minute, then the user base is going to go crazy. With your application, too, if the system fails, then you are going to lose users but, more importantly, you are also going to lose your reputation.

Cosmos DB is a managed database provider and I believe that they care about failover a lot. There are generally two ways to handle failover:

*   Automatic failover
*   Manual failover

Let's discuss each of them.

# Automatic failover

In rare cases of a system outage, automatic failover tries to recover and reassign the traffic to the existing regions. There are generally two cases where this applies:

*   Read region failover
*   Write region failover

If any read regions fails, Cosmos DB automatically marks them offline, in order to avoid any existing traffic trying to route to that region (to avoid 404 and 502 errors), and, using the internal consensus, figures out the next read region and drives the traffic to that region.

This is pretty much what Cosmos DB offers and this is, I believe, what every other database provider offers. The key feature is the re-syncing of the data when the affected region is online again. Cosmos DB updates that region with the data that it missed during the affected time and everything returns to a normal state again.

If any write region fails, then Cosmos DB looks for another write region. If your application has only one write region and that failed, Cosmos DB will then convert any read region, based on their relative priorities, to a write region and makes the application run smoothly.

When the original write region comes back online, it will re-sync the data and the original write region will become a write region again, as it was supposed to be.

We have seen automatic failover; now let's look at the manual failover.

# Manual failover

You can manually change the write region, depending on your needs.

For example, if you are aware that lots of people perform writes from India in the day time and your write region is in the United States, you can manually change the write region to India in IST time and change it back to the United States during the day time.

Although that's not suggested, you can also look over the few concepts of failover handling, such as the one I mentioned earlier, or based on constant traffic. It's totally up to you.

The key thing to take away here is that none of these manual failover tasks will result in data loss or application downtime. Play with them and the system will be online (99% of the time, 1% is for the disaster situation).

Alright, let's discuss database deployment!

# Database deployment

Cosmos DB is a managed database, on top of which we can run multiple database APIs and distribute data across the globe.

Unlike traditional database deployment, where we make changes in SQL or NoSQL data and deploy it on the server, you need to basically plug in your application with the database ID and you are good to go.

There is no need for more server administration or management handling; it's that simple.

Then, a question arises:

*Can we trust them with our data?*

This is one of the most frequently asked questions when it comes to managed database solutions. Yes, you can trust them with your data. In fact, they exist to provide you with the best possible solutions and, if something happens with your data, it's their responsibility to bring it back (if it's not corrupted, of course).

On the subject of trusting things with your data, let's quickly go over the way to backup and restore your data with Cosmos DB.

# Backup and restore

Cosmos DB automatically takes backups of all your data and keeps it updated at regular time intervals. The automatic backup operations are done without affecting the performance or availability of your database operations.

Cosmos DB backups are stored separately, in another storage service, and those backups are globally replicated for high time availability and to avoid regional disasters.

The automatic backups are made for scenarios when you accidentally delete your Cosmos DB container and later require data recovery.

The automatic backups are taken every four hours and at least two copies of the latest backup are stored. If you have deleted your database and information, you can contact the Azure team within four to eight hours to get your data back!

The automatic backups are stored in the blog storage and are kept in the same region as your master data to provide high availability in the case of failure. Automatic backups are also replicated across the region, in order to have a copy of the backups.

There is one key point you need to note, as mentioned: Azure automatically backs up your data every four hours and, if there is data corruption happening in your database, you will need to immediately drop or delete the database in order to save the backup from being corrupted.

Let's go ahead and see how one can deploy a custom app over the Azure network.

# Deploying a custom app over the Azure network

Azure provides various ways to deploy your application over their network. Depending on the type of the project—that is, whether it is a web application, a batch processing application, or any other type of app—you will need to make a choice of which matches to their services.

For example, let's say we want to deploy the Node.js application on the Azure network; we then need to choose the type of server we want to host our application on.

Azure provides both Windows and Linux-based machines where you can deploy your application.

Let's see we want to create one in the Linux box.

Open the Azure portal and click on Virtual Machines from the left menu. Click on the Create Virtual Machines button.

Refer to the following screenshot for help:

![](assets/c0d77cca-91cd-457a-a770-5e697069f51a.png)

Now, Azure will open up the compute screen; you will see lots of options regarding the operating system and virtual machine images. Refer to the following screenshot for reference:

![](assets/4e164a03-54b0-4161-a90d-3d89241d8f6a.png)

I am going ahead with an Ubuntu Server.

Click on the Ubuntu Server and choose the right version of the Ubuntu operating system. Use the Resource Manager deployment option and click on the Create button. Refer to the following screenshot for reference:

![](assets/7f5ea776-0f77-4271-a715-dd0d0ed9613d.png)

Now, give your virtual machine a name and choose an appropriate password. You can select create new resource or use existing resources. Set disk type to SSD for good performance. Refer to the following screenshot for reference:

![](assets/7dab8fd9-55de-4c8e-90cb-4dcf5a7ef1f6.png)

On the next screen, choose a size or use the default size. Skip option 3 and then create the virtual machine.

Once the virtual machine is created, you can log in to it via the terminal using the following command:

```
ssh username@ip  
```

Provide the password and you should be able to log in to the server.

Run this command whenever you do a fresh Ubuntu install:

```
sudo apt-get update
sudo apt-get upgrade 
```

You can install Node.js using this command or download it from the official site:

```
sudo apt-get install nodejs 
```

Now, if your code base is hosted at GitHub, you can use `git` command to download the source code:

```
git clone <<github url>>  
```

Install the required dependencies using the `npm install` command and run the project.

In this way, you can deploy any custom application over the Azure network.

Now, let's move ahead to the last topic of our chapter: monitoring the network and traffic.

# Monitoring traffic and usage

Azure provides in-depth monitoring of your databases and their usage. You can add monitoring alerts and check the network logs from your control panel itself. The information is presented in a neat and proper way, to provide as many details on-screen as possible.

To check the monitoring, log in to Azure portal and click on Monitor in the left menu. You should see a screen similar to the following screenshot:

![](assets/9a7ab8b2-1a94-435b-b673-46191fd8b0f6.png)

Now, click on Metrics in the left menu. It should open a screen similar to what is shown in the following screenshot:

![](assets/9ebad9a6-1171-4d15-971f-7c8536316a76.png)

You can change the filter and database to view the metrics for those databases.

You can also check the activity log of your account and resource group. Click on activity log from the left menu and choose your resource group and date range to view the activities. Refer to the following screenshot for reference:

![](assets/acbc2746-361b-4de4-a279-876b2f2bc00b.png)

This is a great way to check for suspicious activity.

Now, for the usage part, you can go to the cost management + billing screen to view the current usage of your account. I have got a free account for the month and need to upgrade it with a plan for further operations.

Explore more options in the Monitor screen and see what works for you.

Before we wrap up, let's quickly look at the security provided by Azure for our Cosmos DB.

# Database security

Cosmos DB data which is not moving around the network is encrypted and secured. You can be rest assured that, even if attackers somehow get your stored data, it will be extremely tough for them to decrypt it.

Let's look over the key security features offered by Azure:

*   **Secured network**: Cosmos DB provides an IP-based, rule-filtering firewall for advanced network protection
*   **Authorization**: Cosmos DB authorize users using hash-based authentication codes
*   **User management**: Cosmos DB provides secure user management using the resources concept
*   **Active directory support**: Cosmos DB integrates with widely known and secure active directory authentication systems
*   **Turnkey distribution and replication**: Using turnkey distribution, data is replicated and secured
*   **Failover handling**: You can use automatic or manual failover
*   **Automated backups**: Cosmos DB data is backed up regularly
*   **Database restoration**: You can restore your database from the backup
*   **Monitor for attacks**: By using monitoring, you can monitor attacks if they are happening to your server
*   **HTTPS/SSL/TLS encryption**: All data traveling via the Azure network is encrypted
*   **Encryption at rest**: All data stored in Azure machines is encrypted

Compared to the industry at large, Azure offers some very advanced features.

# Summary

We have learned how to perform the global distribution of data in Cosmos DB and why it's important as well. We also learned about failovers and the way to handle them.

We looked over how Cosmos DB performs the backup and how we can restore it in case of any emergencies. We then moved ahead and talked about how we can deploy the custom application in Azure network.

With deployment comes monitoring. Thanks to the Azure management portal, we can monitor our application usage and activities from the browser itself.

This is the last chapter of this book. Before I go, I would like say thank you and wish you luck for your upcoming app using the Cosmos DB infrastructure. If our book has helped you a bit during the process of building the app, do let us know; your feedback is our oxygen.

I hope and wish that this book is of some value to you; if it is, then please shoot me an email at `shahid@codeforgeek.com` or tweet me at `@codeforgeek`. Hoping to hear from you soon!

# Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

[![](assets/f4df836c-b61d-42c9-b8b8-40a015f802b8.png)](https://www.packtpub.com/big-data-and-business-intelligence/stream-analytics-microsoft-azure) 

**Stream Analytics with Microsoft Azure**

Anindita Basak, Krishna Venkataraman, Ryan Murphy, Manpreet Singh  

ISBN: 978-1-78839-590-8

*   Perform real-time event processing with Azure Stream Analysis
*   Incorporate the features of Big Data Lambda architecture pattern in real-time data processing
*   Design a streaming pipeline for storage and batch analysis
*   Implement data transformation and computation activities over stream of events
*   Automate your streaming pipeline using PowerShell and the .NET SDK
*   Integrate your streaming pipeline with popular Machine Learning and Predictive Analytics modelling algorithms
*   Monitor and troubleshoot your Azure Streaming jobs effectively

[![](assets/9d7e4424-fd7d-4fdd-8a48-17c47c7bf291.png)](https://www.packtpub.com/big-data-and-business-intelligence/learning-microsoft-azure-storage)

**Learning Microsoft Azure Storage**  
Mohamed Waly

ISBN: 978-1-78588-491-7

*   Understand Azure Storage types and determine the appropriate one for your needs
*   Design Azure Storage for Azure VMs according to best practices
*   Design and implement your SQL Database on Azure according to best practices
*   Learn how to work with Azure Backup
*   Learn how to work with Azure Site Recovery
*   Extend Azure Storage to StorSimple
*   Monitor storage metrics and logs and customize the Azure monitoring dashboard
*   Monitor and troubleshoot Azure Storage

# Leave a review - let other readers know what you think

Please share your thoughts on this book with others by leaving a review on the site that you bought it from. If you purchased the book from Amazon, please leave us an honest review on this book's Amazon page. This is vital so that other potential readers can see and use your unbiased opinion to make purchasing decisions, we can understand what our customers think about our products, and our authors can see your feedback on the title that they have worked with Packt to create. It will only take a few minutes of your time, but is valuable to other potential customers, our authors, and Packt. Thank you!