
![](img/f02f759059b86af67e38b543d498a0d8_0_0.png)

Community Experience Distilled

## Learning Alfresco Web Scripts

Learn a powerful way to successfully implement unique integration solutions with Alfresco

![](img/f02f759059b86af67e38b543d498a0d8_0_1.png)

Ramesh Chauhan

Packt Publishing — open source community experience distilled  
Birmingham — Mumbai

Copyright © 2014 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: November 2014

Production reference: 1311014

Published by Packt Publishing Ltd., Livery Place, 35 Livery Street, Birmingham B3 2PB, UK.

ISBN 978-1-78439-060-0

www.packtpub.com

## Credits

| Role | People |
|---|---|
| Author | Ramesh Chauhan |
| Project Coordinator | Kartik Vedam |
| Reviewers | Snig Bhaumik, Dhaval Joshi, Bernd Krumböck, Piergiorgio Lucidi |
| Proofreaders | Simran Bhogal, Maria Gould, Ameesha Green, Paul Hindle |
| Commissioning Editor | Dipika Gaonkar |
| Indexers | Mariammal Chettiyar, Rekha Nair, Tejal Soni |
| Acquisition Editor | Kevin Colaco |
| Production Coordinator | Arvindkumar Gupta |
| Content Development Editor | Priyanka Shah |
| Cover Work | Arvindkumar Gupta |
| Technical Editor | Nikhil Potdukhe |
| Copy Editors | Relin Hedly, Deepa Nambiar |

## About the Author

**Ramesh Chauhan** is presently working as a lead consultant at CIGNEX Datamatics. He has core IT experience of around 9 years. Having strong expertise in Alfresco, he has implemented and delivered customized business solutions in Alfresco for customers across the globe and has extensively used Alfresco in multiple production projects. He earned his Bachelor of Engineering degree in Information Technology from Nirma Institute of Technology, Gujarat University, India. He also contributes to Alfresco community forums.

## Acknowledgments

I would first like to extend my gratitude to Lord Swaminarayan and my guru, H. H. Pramukh Swami Maharaj, for all the blessings. I would like to thank my mom and dad for their unconditional love and support. Special thanks to my loving and caring wife, Priyanka Chauhan, who has always encouraged me and supported me throughout the journey of this book. Big thanks to my one-year-old daughter, Misri, who has been the source of inspiration for me.

I would like to thank Packt Publishing for this opportunity. I would like to thank all the technical reviewers for their valuable feedback and give my thanks to Kevin Colaco, Priyanka Shah, and Nikhil Potdukhe from the Packt Publishing team. It was really nice working with the entire Packt Publishing team. I sincerely appreciate Priyanka Shah, the content development editor, and Nikhil Potdukhe, the technical editor, who helped a lot in fixing my writing style with their valuable feedback.

I would like to thank our management team at CIGNEX Datamatics for encouraging me to write this book as a part of the community contribution from our organization. My sincere thanks to Mr. Munwar Shariff (CTO, CIGNEX Datamatics) for all his support. I would also like to thank the entire Alfresco practice team at CIGNEX Datamatics for being very supportive. Thanks to Aadit Majmudar for helping me whenever needed while writing this book. I would like to thank Dhaval Joshi especially, who has been my very good friend and colleague at CIGNEX Datamatics. He has always been with me whenever I needed his help and input. Thanks to him and my friend, Chirayu Joshi, for believing in me, supporting me, and motivating me to write this book.

## About the Reviewers

**Snig Bhaumik** is the Technical Director at InfoAxon Technologies Limited (www.infoaxon.com) and is responsible for designing solutions for content and knowledge management powered by open source technologies. He has diverse experience of over 13 years in open source technologies such as Alfresco, Liferay, and Pentaho, and Microsoft .NET.

Snig's areas of expertise include knowledge management, collaborative content management, business process automation, social networks, business intelligence, search, and emerging technologies. He acts as a senior consultant on many projects, driving BI, KM, and ECM agendas with senior stakeholders. He is a sought-after speaker in these areas and also writes regularly in the form of white papers and blogs. His works can be accessed at http://blog.infoaxon.com/, http://onalfresco.blogspot.in/, and https://github.com/SnigBhaumik/.

He has a lot of experience in delivering open-source-powered solutions to global customers in the fields of international development, social housing, construction, banking, retail, and insurance. He is an active participant in various open source communities and has contributed functionalities to some of the leading open source platforms.

As an open source enthusiast, Snig is an active contributor to several open source communities:

- He has authored *Alfresco Cookbook 3*, Packt Publishing (http://www.amazon.com/Alfresco-3-Cookbook-Snig-Bhaumik/dp/184951108X)
- He led the development and management of the roadmap for Alfresco add-ons on Alfresco and InfoAxon's marketplace (http://www.infoaxon.com/solutions/addons/#all)
- His key add-ons released on Alfresco include:
  - Bootfresco (https://github.com/SnigBhaumik/Bootfresco)
  - Calendar Event Participant (https://addons.alfresco.com/addons/calendar-event-participant)

**Bernd Krumböck** is a self-employed IT professional. In the last 7 years, he has implemented and supported many projects for the NGO/NPO Volkshilfe NÖ (NPO for nursing and other services with about 1,000 employees in Lower Austria). One of these projects is a document management system based on Alfresco, which is needed for an ISO standardization process. As a result, he has become the new maintainer of the Oracle Server support for the Alfresco Community extension.

Besides this, he is working on various business applications that are mainly based on Java/J2EE. However, his skills are also used for server, database, and network administration.

A few years ago, he was employed as a system administrator at REWE Group, Austria. His principal task was the administration of the HP-UX, Linux, and Oracle database systems. During this time, he contributed various code to CUPS and other projects.

In his free time, he works on various small projects that extend his smart home system. One of these projects resulted in a Linux kernel driver for a USB-CAN adapter from 8 devices (https://github.com/krumboeck/usb2can).

**Piergiorgio Lucidi** is an open source Enterprise Content Management (ECM) specialist and a certified Alfresco trainer at Sourcesense. Sourcesense is a European open source systems integrator providing consultancy, support, and services around key open source technologies.

He works as a mentor, technical leader, and software engineer, and has 10 years of experience in the areas of ECM and system integration. He is an expert in integrating ECM solutions in web and portal applications.

He regularly contributes to the Alfresco community as an Alfresco Wiki Gardener. During Alfresco DevCon 2012 in Berlin, he was named the Alfresco Community Star.

He contributes to the Apache Software Foundation as a PMC member and committer of Apache ManifoldCF and is the project leader of the CMIS, Alfresco, and Elasticsearch connectors. He is a project leader and committer of the JBoss community. He also contributes to some of the projects of the JBoss Portal platform.

He is a speaker at various conferences dedicated to Java, Spring Framework, open source products, and technologies related to the ECM and WCM world.

He is an author, technical reviewer, and affiliate partner at Packt Publishing. For Packt Publishing, he has co-authored *Alfresco 3 Web Services* with Ugo Cei, and *GateIn Cookbook* with Ken Finnigan and Luca Stancapiano. As a technical reviewer, he has also contributed to the books *Alfresco 3 Cookbook*, *Alfresco Share*, and *Alfresco 4 Enterprise Content Management Implementation*, all published by Packt Publishing.

As an affiliate partner, he also writes and publishes book reviews on his website Open4Dev (http://www.open4dev.com/).

> I would like to thank Packt Publishing for another great opportunity to contribute to a project dedicated to the Alfresco platform.
