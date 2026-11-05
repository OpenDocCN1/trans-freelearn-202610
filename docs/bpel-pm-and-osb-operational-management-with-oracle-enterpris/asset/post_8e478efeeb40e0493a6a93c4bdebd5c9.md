
![](img/7529bbd615e859608c21b965162348b2_0_0.png)

## BPEL PM and OSB Operational Management with Oracle Enterprise Manager 10g Grid Control

Manage the operational tasks for multiple BPEL and OSB environments centrally

## BPEL PM and OSB Operational Management with Oracle Enterprise Manager 10g Grid Control

Manage the operational tasks for multiple BPEL and OSB environments centrally

Narayan Bharadwaj

![](img/7529bbd615e859608c21b965162348b2_1_0.png)

BIRMINGHAM - MUMBAI

## BPEL PM and OSB Operational Management with Oracle Enterprise Manager 10g Grid Control

Copyright © 2010 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, Packt Publishing, nor its dealers or distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: August 2010

Production Reference: 2160810

Published by Packt Publishing Ltd.
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-847197-74-0

www.packtpub.com

Cover Image by Sandeep Babu (sandyjb@gmail.com)

## Credits

**Author**
Narayan Bharadwaj

**Reviewers**
Hans Forbrich
Hector R. Madrid
Arvind Maheshwari
Matt Wright

**Acquisition Editor**
James Lumsden

**Development Editor**
Ved Prakash Jha

**Technical Editor**
Rukhsana Khambatta

**Copy Editor**
Sanchari Mukherjee

**Editorial Team Leader**
Gagandeep Singh

**Project Team Leader**
Priya Mukerjee

**Project Coordinator**
Ashwin Shetty

**Proofreader**
Sandra Hopper

**Indexer**
Rekha Nair

**Production Coordinator**
Adline Swetha Jesuthas

**Cover Work**
Adline Swetha Jesuthas

## About the Author

**Narayan Bharadwaj** (aka Nadu) has dabbled in several different areas of the business software market. He has more than a decade of experience working in disparate technologies such as service-oriented architecture, systems management, and everything in between. Other technology areas include cloud computing, database, middleware, and IT management software. While dabbling in these technologies, Narayan wore several hats—from software consultant, database administrator, application developer, to product manager. Prior to these gigs, he went to school at the University of Maryland, and the Indian Institute of Technology, Kharagpur, to pursue various technical degrees. Thinking this was not good enough, he added an MBA from the Haas School of Business at the University of California, Berkeley.

Narayan spent close to a decade at Oracle USA Inc at Redwood Shores, CA, where he was the group product manager for management of middleware and service-oriented architecture products. During his stint at the Application and Systems Management Products division, Narayan led the charge into new markets such as real-user monitoring, Java application diagnostics, middleware management, and service-oriented architecture management. This included key acquisitions such as Moniforce, ClearApp, and AmberPoint. More recently, Narayan has moved to the clouds in San Francisco, leading Salesforce.com's platform strategy and execution with respect to cloud management and cloud data analytics. He lives in San Mateo, CA, with his wife and two-year-old son.

When he is not commuting to work, pursuing degrees, or wearing different hats, Narayan loves to wield a racket on a badminton or tennis court, spike a volley ball, or run the half marathon once in a long while.

> I am constantly inspired by the two women in my life—my mother and my wife. Thank you for being there for me.

## About the Reviewers

**Hans Forbrich** is a well-known member of the Oracle Community. He started with Oracle products in 1984 and has kept abreast of nearly all of Oracle's Core Technologies. As ACE Director, Hans has been invited to present at Oracle Open World and various Oracle User Group meetings around the world. His company, Forbrich Computer Consulting Ltd., is well established in western Canada. Hans specializes in delivering Oracle University training through Oracle University and partners such as Exit Certified.

Although his special interests include Oracle Spatial, OracleVM, and Oracle Enterprise Linux, Hans has been particularly excited about the advances in Oracle SOA, Oracle WebLogic, and Oracle Grid Control.

Hans has been a technical reviewer for a number of Packt Publishing books, including "Mastering Oracle Scheduler in Oracle 11g Databases," "Oracle 10g/11g Data and Database Management Utilities," and "Oracle VM Manager 2.1.2."

> I wish to thank my wife Susanne, and the Edmonton Opera, for their patience while I work on these reviews as well as on my own book.

**Hector R. Madrid** is the author of the "Oracle 10g/11g Data and Database Management Utilities" book. He is currently working as a freelance consultant; he collaborates with Oracle University as a certified instructor for the DBA, Java, and Application Server course tracks. He is a highly respected Oracle professional with 20 years of experience as a full-time DBA. He works with a wide range of DBA requirements starting with the daily DBA duties to the tasks related to mission-critical and high-availability systems. He was the first Oracle Certified Master in Latin America. He obtained a Master's Degree in Computer Sciences from the Metropolitan Autonomous University (UAM) and he has presented different technical papers at several Oracle conferences.

> I want to thank my family for their patience and the time I borrowed from them during the revision of this book.

**Arvind Maheshwari**, a senior Software Development Manager for the Oracle Enterprise Manager development team, is focused on building management solutions for middleware. He has more than 15 years of experience in the IT industry and has played the role of developer, consultant, architect, and technical manager in the financial, manufacturing, and telecom industries, developing enterprise solutions that are deployed in high-availability architectures.

**Matt Wright** is a director at Rubicon Red, an independent consulting firm helping customers enable enterprise agility and operational excellence through the adoption of emerging technologies such as Service-Oriented Architecture (SOA), Business Process Management (BPM), and Cloud Computing.

With over 20 years of experience in building enterprise-scale distributed systems, Matt first became involved with SOA shortly after the initial submission of SOAP 1.1 to the W3C in 2000, and has worked with some of the early adopters of BPEL since its initial release in 2002. Since then, he has been engaged in some of the earliest SOA-based implementations across EMEA and APAC.

Prior to Rubicon Red, Matt held various senior roles within Oracle, most recently as Director of Product Management for Oracle Fusion Middleware in APAC, where he was responsible for working with organizations to educate and enable them in realizing the full business benefits of SOA in solving complex business problems.

As a recognized authority on SOA, Matt is a regular speaker and instructor at private and public events. He also enjoys writing and publishes his own blog (http://blog.rubiconred.com). Matt holds a B.Sc. (Eng) in Computer Science from Imperial College, University of London.

## Table of Contents

- Preface 1
- Chapter 1: Grid Control, BPEL, and OSB Overview 9
- Grid Control 10
- Architecture and deployment 11
- Installing Grid Control 13
- Home page 13
- Oracle BPEL Process Manager overview 14
- Oracle SOA Suite overview 15
- Oracle Service Bus overview 16
- Summary 16
- Chapter 2: Discovering BPEL PM 17
- Support for managing BPEL PM 18
- BPEL PM target discovery 18
- Navigating to application servers 19
- Discovering a BPEL PM target 19
- Navigating to the BPEL PM target home page 22
- Configuring a BPEL PM target 23
- Summary 24
- Chapter 3: BPEL Process Monitoring 27
- Challenges 28
- Solution 28
- Step-by-step exercises 28
- Navigating to the BPEL PM target home page 28
- Navigating to the BPEL process home page 29
- Creating a SOAP test to monitor a partner link 31
- Creating a SOAP test to monitor a BPEL process 35
- Testing the SOAP tests 38
- Summary 39
