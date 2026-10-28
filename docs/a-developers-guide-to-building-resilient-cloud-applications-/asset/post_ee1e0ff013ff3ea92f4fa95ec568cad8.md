
# <packt>

# 1ST EDITION

# A Developer’s Guide to Building Resilient Cloud Applications with Azure

Deploy applications on serverless and event-driven architecture using a cloud database

# HAMIDA REBAI TRABALSI

Foreword by Lori Lalonde, Senior cloud solution architect manager, Microsoft

# A Developer’s Guide to Building
Resilient Cloud Applications
with Azure

Deploy applications on serverless and event-driven
architecture using a cloud database

Hamida Rebai Trabelsi

![](img/66aceec30d3e7dead2f71bb68a887688_1_0.png)

BIRMINGHAM—MUMBAI

# A Developer’s Guide to Building Resilient Cloud Applications with Azure

Copyright © 2023 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Group Product Manager:** Rahul Nair
**Publishing Product Manager:** Surbhi Suman
**Senior Editor:** Divya Vijayan/Romy Dias
**Technical Editor:** Arjun Varma
**Copy Editor:** Safis Editing
**Project Coordinator:** Ashwin Kharwa
**Proofreader:** Safis Editing
**Indexer:** Hemangini Bari
**Production Designer:** Aparna Bhagat
**Marketing Coordinator:** Nimisha Dua

First published: January 2023

Production reference: 1250123

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80461-171-5

https://www.packtpub.com

To my mother, Rafika Boukari, and to the memory of my father, Mongi Rebai, for their sacrifices, their blessings, and the constant support that they have given to me. To my husband, Mohamed Trabelsi, for supporting me during my writing journey. To my son, Rayen Trabelsi, and my baby daughter, Eya Trabelsi, for their smiles every day, which always keep me motivated.

## Foreword

On March 11, 2020, the World Health Organization officially declared a pandemic amid a global COVID-19 outbreak. As the world went into lockdown, it was imperative that businesses pivot and leverage innovative ways to continue their operations. We witnessed organizations employing creative techniques across a variety of industries. Many retail businesses provided online ordering and curbside pick-up when they were required to close their doors or limit the number of customers in-store.

When grocery store shelves were barren and struggling to restock, restaurants also leveraged their direct suppliers to offer grocery kits including produce, dairy, and raw meats to their customers. Fitness studios offered online virtual classes to keep their members active and engaged. Doctors provided online virtual appointments for their patients. Offices that traditionally required employees to be on-site transitioned to enable remote work using collaboration tools such as Microsoft Teams. What did they all have in common? They leveraged cloud services.

In his first earnings call during the pandemic, Satya Nadella, CEO of Microsoft, stated, “We’ve seen two years of digital transformation in two months.” Enabling remote work, accelerating the digital transformation to the cloud, and building innovative solutions allowed organizations to not only survive but also thrive.

Three years later, the world has slowly gone back to a new normal – a hybrid world partaking in experiences online as well as in person. Yet, organizations are not slowing down. They are accelerating their digital transformation and application modernization initiatives. Gartner predicts that cloud spending will continue to increase through to 2025, with enterprise organizations continuing to invest in their shift to the cloud. I see this firsthand in my day-to-day work in leading a team of cloud solution architects that helps organizations to modernize and innovate in Azure.

What does this mean for you, whether you’re an architect, developer, student, or aspiring technologist? As the demand for cloud-native skills continues to grow, now is the time to learn these crucial development skills, or risk getting left behind. A simple search for “cloud native” jobs on LinkedIn yields thousands of results. This is an in-demand skill that serves as a career accelerator. You have taken the first step by buying Hamida’s book, which is a wise investment on your part.

Hamida draws on years of experience in developing cloud-native solutions in Azure and has distilled her expertise for you here. This is a comprehensive resource that goes beyond the question “what is cloud-native application development?” This is not another book on containers and orchestration services. Hamida provides you with the “how-to.” She delivers the roadmap to build an end-to-end cloud-native application, from development to deployment.

You will walk away ready to put into practice everything that you learn within these pages. You will gain a deeper understanding of which cloud service to use, why, and how it all comes together to create and deploy a scalable, reliable, highly available application.

Good luck on your journey!

Lori Lalonde

Senior cloud solution architect manager, Microsoft

## Contributors

### About the author

**Hamida Rebai Trabelsi** has been working in the computing domain for over 12 years. She started her professional career in Tunisia working for **multinational corporations (MNCs)** as a software developer, then served as a .NET consultant at CGI, Canada. She is currently a senior advisor and information and solution integration architect at Revenu Québec, Canada. She has been awarded as Most Valuable Professional in Developer Technologies and Microsoft DevHeros by Microsoft and holds several Azure certifications. Besides being a Microsoft Certified Trainer and a member of the .NET Foundation, Hamida is a blogger, an international speaker, and one of the finalists in the Women in IT Award in Canada in 2019.

> I have been overwhelmed by the support that I have received from all of the team. They encouraged me during this period, and I feel proud that I was able to finish this book and share my experience as a developer to start with Azure. To the memory of my father, Mongi Rebai, I offer this book to you and I hope that you were with me, sharing in this new adventure that is added to my professional career. Thank you to my mum, Rafika Boukari, for your sacrifices and your support. Thank you to my husband, Mohamed Trabelsi, for believing in me. Every day, I wake up and see my son, Rayen Trabelsi, and my baby girl, Eya Trabelsi, and I feel more motivated, so thank you for this energy.

### About the reviewers

**Ryan Mangan** is a cloud and an end user computing technologist focusing on application migration and modernization. He's an international speaker on a wide range of topics, an author, and has helped customers and technical communities over the past decade. Ryan is a chartered fellow of the British Computer Society, a Microsoft **Most Valuable Professional (MVP)**, as well as a VMware vExpert, and **Very Important Parallels Professional (VIPP)**.

**Stefano Demiliani** is a Microsoft MVP in business applications, a **Microsoft Certified Solution Developer (MCSD)**, an Azure Certified Architect, and an expert in other Microsoft-related technologies. His main activity is architecting and developing enterprise solutions based on the entire stack of Microsoft technologies (mainly focused on ERP and the cloud). He has worked with Packt Publishing on many IT books related to Azure cloud applications and Dynamics 365 Business Central and is a speaker at conferences around Europe. You can reach him on Twitter (@demiliani) or LinkedIn.
