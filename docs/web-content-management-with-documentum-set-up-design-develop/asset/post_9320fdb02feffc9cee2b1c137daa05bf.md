
![](img/b18244e6621ecc39cef03ff6c0e55e7c_0_0.png)

From Technologies to Solutions

## Web Content Management with Documentum

Set up, Design, Develop, and Deploy Documentum Applications

Concise, practical information on Documentum Web Content Management to get the most from this system

Gaurav Kathuria

[PACKT] PUBLISHING

## Web Content Management with Documentum

Set up, Design, Develop, and Deploy
Documentum Applications

Concise, practical information on Documentum Web Content Management to get the most from this system

Gaurav Kathuria

![](img/b18244e6621ecc39cef03ff6c0e55e7c_1_0.png)

BIRMINGHAM - MUMBAI

## Web Content Management with Documentum

## Set up, Design, Develop, and Deploy Documentum Applications

Copyright © 2006 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, Packt Publishing, nor its dealers or distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: June 2006

Production Reference: 1130606

Published by Packt Publishing Ltd.
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.
ISBN 1-904811-09-4
www.packtpub.com

Cover Image by www.visionwt.com

## Credits

| **Author** Gaurav Kathuria | **Indexer** Niranjan Jahagirdar |
|---|---|
| **Development Editor** Douglas Paterson | **Proofreader** Chris Smith |
| **Assistant Development Editor** Nikhil Bangera | **Production Coordinator** Manjiri Nadkarni |
| **Technical Editor** Ashutosh Pande | **Layouts and Illustrations** Shantanu Zagade |
| **Editorial Manager** Dipali Chittar | **Cover Designer** Shantanu Zagade |

## About the Author

Gaurav Kathuria completed his B. Tech. (Hons.) in Chemical Engineering from I.I.T. Kharagpur in the year 2000 and has since been a prominent performer in diverse software fields, from IT services through product development to software consultancy.

He has a rich experience of designing, developing, and managing software systems using object-oriented languages and technologies like Java/J2EE and Documentum.

He started working with Documentum 4i in the year 2001 and has ever since had an extensive experience architecting/designing complex Documentum 4i and 5x projects.

He has also given in-house training on Documentum system architecture, fundamentals, and Web Publisher in many of the organizations he has worked in.

This book is dedicated to

God: Who has always showered his choicest blessings on me and given me much more than I ever wanted in my life. I thank Him for all that he has done for me.

My family: My father (Mr. P.N. Kathuria) has always been a guiding star in my life, mentoring and steering me through thick and thin. Extremely diligent and sincere in all his endeavors; I have learnt and am still learning a lot from him.

My mother (Mrs. Sarita Kathuria) has selflessly devoted her entire life for the well-being of our sweet little family. She has always been the shoulder I cried on when I was in distress and she has been the one who praised me most when others disapproved me.

My sister (Ms. Gunjan Kathuria) is the sweet little sister I always wanted in my life. Her affection and care has given a new meaning to my life.

My wife (Mrs. Gunjan Grover) has blossomed our house with love and respect for everyone. Her mere presence fills up and completes the missing bit in my life...

My friends: Neeraj Jain, Nisha, Hima, Nishant Anchal, and Abhishek Singh, who have always been by my side, making this world a better place to live in.

Documentum team mates: Mansoor Sheikh, Arnab Ghosh, Amit Kapur, Prashant Shukla, Gajendra Sahu, Gurmeet Singh, Prasen Misra, Tanveer Haider, Arpana Bansal, Preeti Dua, Kapil Bharati, Akash Narang, Kesavan, Usha Parolkar, Anjali Nanda, and other software professionals with whom I have worked on various Documentum projects. They all have been a source of inspiration for me in some way or the other.

I thank you all for your love and support!

## Acknowledgements

Dr. Louay Fatoohi, Dr. Douglas Paterson, and the entire crew at Packt Publishing Ltd. for honoring me by publishing this book in the most stringent of timeframes.

Johnson Stephen: My pal, colleague, and a software testing maestro. He 'unknowingly' guided me to write a book as a precursor to establishing myself and starting up my own consultancy in future.

M. Scott Roth: Author of 'A Beginner's Guide to Developing Documentum Desktop Applications'.

Scott applauded my decision to write this book on Documentum technology and constantly provided the much needed support and zeal.

Anil Baid: The owner and head of 'Solutions Infosystems'. He has been an extremely helping hand for me, without whom this book would have never seen the light of day.

Rakesh Dahiya: The Facilities manager at 'Solutions Infosystems' who guided me often regarding the various publishing avenues available and the tips and tricks of the trade.

Ashwin Razdan: Media Manager, whatistesting.com; an extremely versatile personality who assisted me in getting the book shaped up to the right standards by providing the much needed direction and support.

Sachin Jain: The Accounts and legal head at 'Solutions Infosystems' whose valuable advice steered me clear of several difficult situations during the book authoring process.

Pankaj Jain and Pradeep Gautam ('Econsultants India').

My sincere apologies to those whose names might have inadvertently been missed out from this list. You all are very important to me.

## Table of Contents

- Preface 1
- Chapter 1: Content and Documentum 5
  - 1.1 Need for an Effective CMS 7
  - 1.2 Qualities of a Good CMS 8
  - 1.3 Why Documentum? 10
  - 1.4 Documentum Features 11
  - 1.5 Summary 12
- Chapter 2: Documentum Essentials 13
  - 2.1 Documentum Cornerstone 13
  - 2.2 Docbase 13
  - 2.3 DocBroker 15
  - 2.4 DocApp 16
  - 2.5 Object Types 16
  - 2.6 Attributes 18
    - 2.6.1 Object ID (Object Identifier: r_object_id Attribute) 19
    - 2.6.2 Attribute Types 19
  - 2.7 DQL 21
  - 2.8 API 22
  - 2.9 Cabinets and Folders 23
  - 2.10 Versioning 24
  - 2.11 Lifecycles 25
  - 2.12 Workflows 26
  - 2.13 Summary 28
- Chapter 3: Documentum Advanced Concepts 29
  - 3.1 DMCL 29
  - 3.2 DFC 29
  - 3.3 BOF 30
  - 3.4 WDK 31
  - 3.5 Web Publisher 32
  - 3.6 ACL 33

## Table of Contents

- 3.7 Alias Set 34
- 3.8 Users and Groups 36
  - 3.8.1 Users 36
  - 3.8.2 Groups 36
- 3.9 Renditions 37
- 3.10 Registered Tables 38
- 3.11 Data Dictionary 38
- 3.12 Methods and Jobs 39
  - 3.12.1 Methods 39
  - 3.12.2 Jobs 40
- 3.13 Summary 42
- Chapter 4: Web Content Management System 43
  - 4.1 When Should you Use a WCM System? 43
  - 4.2 When Should you Not Use a WCM System? 44
  - 4.3 Documentum WCM Architecture 45
  - 4.4 How do you Enter Content in the System? 46
    - 4.4.1 Web Publisher 46
  - 4.5 Where and How is the Content Stored? 47
    - 4.5.1 Content Server 47
  - 4.6 How do you Perform System Administration? 48
    - 4.6.1 Documentum Administrator 48
  - 4.7 How do you Create Multiple Renditions of the Content? 50
    - 4.7.1 Content Rendition Services 50
  - 4.8 How do you Publish Content to a Website? 52
    - 4.8.1 Site Caching Services 52
    - 4.8.2 SCS Architecture 53
  - 4.9 How do you Query the Published Content for Displaying on Websites? 54
    - 4.9.1 Documentum JDBC Services 54
  - 4.10 How do you Deploy Website Content to a Server Farm? 55
    - 4.10.1 Site Deployment Services 55
  - 4.11 Summary 56
- Chapter 5: Setting Up the Documentum Suite 57
  - 5.1 Installing Content Server 58
    - 5.1.2 Prerequisites 59
