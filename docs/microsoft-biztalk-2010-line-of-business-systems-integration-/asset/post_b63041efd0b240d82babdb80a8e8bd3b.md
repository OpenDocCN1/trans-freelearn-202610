
![](img/23d1402649b6528b085cd6b900a2c9c3_0_0.png)

Professional Expertise Distilled

## Microsoft BizTalk 2010:
Line of Business
Systems Integration

A practical guide to integrating Line of Business systems with BizTalk Server 2010

Foreword by Tony Meleg, Senior Technical Product Manager
Microsoft Corporation

Kent Weare   Richard Seroter   Sergei Moukhovitski
Thiago Almeida   Carl Darski

![](img/23d1402649b6528b085cd6b900a2c9c3_0_1.png)

## Microsoft BizTalk 2010: Line of Business Systems Integration

A practical guide to integrating Line of Business systems with BizTalk Server 2010

- Kent Weare
- Richard Seroter
- Sergei Moukhovitski
- Thiago Almeida
- Carl Darski

![](img/23d1402649b6528b085cd6b900a2c9c3_1_0.png)

BIRMINGHAM - MUMBAI

## Microsoft BizTalk 2010: Line of Business Systems Integration

Copyright © 2011 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: July 2011

Production Reference: 1080711

Published by Packt Publishing Ltd.
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-849681-90-2

www.packtpub.com

Cover Image by Mark Holland (MJH767@bham.ac.uk)

## Credits

### Authors
- Kent Weare
- Richard Seroter
- Sergei Moukhovitski
- Thiago Almeida
- Carl Darski

### Reviewers
- Mick Badran
- Stephen W. Thomas
- Ben Cline

### Acquisition Editor
Kerry George

### Development Editor
Alina Lewis

### Technical Editor
Ajay Shanker

### Project Coordinator
Zainab Bagasrawala

### Proofreader
Aaron Nash

### Indexer
Monica Ajmera

### Graphics
Geetanjali Sawant

### Production Coordinator
Alwin Roy

### Cover Work
Alwin Roy

## Foreword

We know that it is rare in IT departments today that solutions do not have to integrate with other systems and even external entities. Systems just have to be connected to support the complex business processes and levels of automation that the business is demanding. Some of these systems are really at the core of the business. They can support operational functions like finance, human resources, and customer relationships. They can also be mission critical; if they stop for any length of time, the business would be significantly impacted.

We know that connecting systems is complicated and there are many choices, technologies, and products that play a role. Developers often build their own integration solutions and supporting infrastructure. The "last mile" of the integration problem is one of the hardest—how do you actually exchange data and messages between two systems—how do you hook them together?

The promise the industry at large has been looking at is the world of web services and service oriented architecture (SOA). We will never totally get to this world—there are too many systems, standards, and custom solutions that don't have "services" to call. But what if there was a solution and even a development framework that could on one end connect to these systems and on the other, expose the capabilities as "services". Let's call these "adapters". What if you could then hook these adapters into your integration platform and even into your own custom code? What if you could easily write your own "adapter"?

This book is all about how to take advantage of Microsoft's integration platform—BizTalk Server and how to use the various "adapters" that come with BizTalk Server to integrate with your line of business systems.

The Cloud is fast becoming an important part of this picture as well, to integrate with systems that live in the Cloud as well as to create processes and move data through the cloud. The book covers the cloud platforms being created to make this happen and practically how to do this today.

> This book is a fantastic resource for the developer wanting to understand the techniques as well as the practical mechanics of connecting systems together.

Tony Meleg

*Senior Technical Product Manager*
*Microsoft Corporation*

## About the Authors

**Kent Weare**, born in Regina, Saskatchewan, Canada, developed a love for Ice Hockey, Football and Technology. He attended the University of Regina where he obtained a Degree in Computer Science. After completing his undergrad degree, he spent time in India completing a Post Graduate diploma in Object Oriented Technology. He currently lives in Calgary, Alberta, Canada but remains a die-hard Saskatchewan Roughrider football fan.

Kent began his career at a small Internet startup before taking on a Junior role with the Saskatchewan Government. Since then he has worked on projects for the Canadian Federal Government, a multi-national bank in the United States, Health Care projects in Eastern and Western Canada and has spent the last five years employed by a large Electricity Distribution company in Calgary. Kent's current responsibilities involve managing a Microsoft Solutions team that supports BizTalk, Exchange, Office Communication Server (OCS), and System Center.

During Kent's time at the Federal Government, he had an opportunity to participate in his first BizTalk project. Seven years later he is still "hooked" on BizTalk, having worked with every BizTalk version released since. In 2008, Kent was awarded his first Microsoft MVP award for BizTalk Server. He continues to be active in the BizTalk community and recently received his fourth consecutive MVP award. Kent maintains an active blog at http://kentweare.blogspot.com and may be seen presenting BizTalk related material at local and international user groups.

I would first off like to thank my parents (Keith and Joyce) for their unconditional love, support, and direction growing up. They are strong proponents of working hard, treating others with respect, and taught me the difference between right and wrong.

To my twin brother (Kurt), sister (Kim), and their families, thank you for your interest in the book and the encouragement to keep plugging away at it until it was completed.

Throughout my career many people have seen something in me that I haven't necessarily seen in myself. I am convinced that without this support, I would have never been in a position get into the MVP program or write this book. Special thanks to Les Phillips, Dave Patel, Ron Naidu, Vasu Iyengar, Lucie Duval, Neal Nishikawa, Darren Jeffrey, Brian Dempsey, and Alan Skiffington for giving me an opportunity and then giving me the tools to be successful. I would also like to thank my mentors Nipa Chakravarti and Karl Smith for their insight and challenging me to grow as a Leader.

Lastly, this book would not have been a reality without the support of my loving wife Melissa and daughter Brooke. Writing a book is a huge undertaking that consumed late nights, early mornings, and pretty much any spare time in-between. Thank you Melissa for your patience, support, and putting up with me sitting in front of the computer for hours on end while I worked on the book. Brooke, thank you for not hitting the blue (power) button too many times while I was writing. At times you really pushed the limits of Microsoft Word's auto-recovery feature.

**Richard Seroter** is a solutions architect for an industry-leading biotechnology company, a Microsoft MVP for BizTalk Server, and a Microsoft Connected Technology Advisor. He has spent the majority of his career consulting with customers as they planned and implemented their enterprise software solutions. Richard started his career working for two global IT consulting firms that gave him exposure to a diverse range of industries, technologies, and business challenges. Then, he joined Microsoft as a SOA/BPM technology specialist where his sole objective was to educate and collaborate with customers as they considered, designed, and architected BizTalk solutions. One of those customers liked Richard enough to bring him onboard full time as an architect after they committed to using BizTalk Server as their enterprise service bus. Once the BizTalk environment was successfully established, Richard transitioned into a solutions architect role where he now helps to identify enterprise best practices and apply good architectural principles to a wide set of IT initiatives.

Richard is the author of two books including *Applied Architecture Patterns on the Microsoft Platform* (Packt Publishing, 2010), which discusses where to use which part of Microsoft's platform technologies. He is also the author of *SOA Patterns for BizTalk Server 2009* (Packt Publishing, 2009), which takes a look at how to apply good SOA principles to a wide variety of BizTalk scenarios.

> I'd like to thank Kent for bringing me onto this interesting project and allowing me to contribute some chapters. I have a sick passion for enterprise integration and Kent provided me a perfect outlet for more research into the topic.
>
> Thanks to all of my co-workers who have inevitably influenced my thinking and made me a better architect and technologist. My manager, Nancy Lehrer, continues to be an exceptional mentor on my jagged path to superstardom. Finally, thanks to my family and my boys (Noah the human, Watson the dog) as they put up with late nights and absentmindedness.
