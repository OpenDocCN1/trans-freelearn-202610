
## Mastering
Cyber Intelligence

Gain comprehensive knowledge and skills to conduct threat intelligence for effective system defense

## Mastering Cyber Intelligence

Gain comprehensive knowledge and skills to conduct threat intelligence for effective system defense

Jean Nestor M. Dahj

![](img/d47f26598025b2da017dd41fcbb6e602_1_0.png)

BIRMINGHAM—MUMBAI

## Mastering Cyber Intelligence

Copyright © 2022 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author(s), nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Group Product Manager:** Vijin Boricha
**Publishing Product Manager:** Shrileka Malpani
**Senior Editor:** Arun Nadar
**Content Development Editor:** Yasir Ali Khan
**Technical Editor:** Rajat Sharma
**Copy Editor:** Safis Editing
**Project Coordinator:** Ajesh Devavaram
**Proofreader:** Safis Editing
**Indexer:** Hemangini Bari
**Production Designer:** Joshua Misquitta
**Marketing Coordinator:** Hemangi Lotlikar

First published: April 2022
Production reference: 1010422

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80020-940-4
www.packt.com

To my father, Muwawa Salam Tjoppen, and my mother, Isamanga Olwey Therese, for their sacrifices and values instilled in me. In loving memory of my youngest brother, Muwawa Legi Yao Bob. To my family for their support. To God for the gift of life, which allows me to keep working hard, hoping, and dreaming.

- Jean Nestor M. Dahj

## Contributors

## About the author

Jean Nestor M. Dahj is an experienced data scientist, cybersecurity researcher and analyst, and telecom professional with wide technical and scientific abilities. His skills have led him to work in the areas of data science, network probing, penetration testing and hacking, threat intelligence, and network analytics. He has built a wide range of skill sets through experience, training, and consultancy, including skills in cryptography, computer forensics, malware design and analysis, and data product development.

Jean Nestor holds a master's degree (M-Tech) in electrical engineering from the University of South Africa. He is currently pursuing a Ph.D. in the same field at the University of Johannesburg, South Africa. His work history includes the likes of Huawei Technologies, Commprove Technologies, Siftcon Forensic Services, Metro Teleworks, and Nanofritech Consulting & Research Lab – an organization he co-founded. He is currently a full stack data scientist at Rain Networks, part of a dynamic team providing and developing various data solutions.

He currently lives in Pretoria, South Africa, and is originally from Kikwit, a small city in the Democratic Republic of Congo.

*Special thanks to everyone who has supported me through the journey of writing this amazing book.*

## About the reviewer

**Max van Kralingen** is a cyber threat intelligence consultant with particular expertise in threat actor profiling, cyber fusion centers, OSINT investigations, MITRE's ATT&CK framework, social engineering, and breach and attack simulation. He holds a BA in political science, an MA in security and intelligence studies, and a PGDip in advanced security and digital forensics.

> *I would like to thank my wonderful parents. My mother, for giving me a love of investigations, conversation, philosophy, psychology, and Lord of The Rings. My father, who gave me a love of history, politics, and Dire Straits. Finally, to the two most influential teachers in my life, who saw in me a spark of what I would become. John Carnegie, for the stories, code, and Star Trek. Julian Richards, for the art and science of intelligence analysis.*

## Table of Contents

## Preface

## Section 1: Cyber Threat Intelligence Life Cycle, Requirements, and Tradecraft

1

## Cyber Threat Intelligence Life Cycle

Technical requirements 4

Intelligence data collection 14

Cyber threat intelligence – a global overview 4

Intelligence data processing 15

Characteristics of a threat 6

Analysis and production 17

Threat intelligence and data security challenges 6

Threat intelligence dissemination 20

Importance and benefits of threat intelligence 9

Threat intelligence feedback 21

Planning, objectives, and direction 11

Summary 22

2

## Requirements and Intelligence Team Implementation

Technical requirements 24

Requirements development 33

Threat intelligence requirements and prioritization 25

Operational environment definition 34

Prioritizing intelligence requirements 29

Network defense impact description 36

Current cyber threats – evaluation 40

Developing a course of action 41
Intelligence preparation for intelligence requirements 42
Intelligence team layout and prerequisites 44

Intelligence team implementation 46
Intelligence team structuring 46
Intelligence team application areas 48
Summary 49

## 3

## Cyber Threat Intelligence Frameworks

Technical requirements 52
Intelligence frameworks – overview 52
Why cyber threat frameworks? 53
Cyber threat framework architecture and operating model 54
Lockheed Martin's Cyber Kill Chain framework 59
Use case – Lockheed Martin's Cyber Kill Chain model mapping 62
Integrating the Cyber Kill Chain model into an intelligence project 64
Benefits of the Cyber Kill Chain framework 66
MITRE's ATT&CK knowledge-based framework 67

How it works 68
Use case – ATT&CK model mapping 73
Integrating the MITRE ATT&CK framework 74
Benefits of the ATT&CK framework 75
Diamond model of intrusion analysis framework 76
How it works 77
Use case – Diamond model of intrusion analysis 78
Integrating the Diamond model into intelligence projects 79
Benefits of the Diamond model 80
Summary 81

## 4

## Cyber Threat Intelligence Tradecraft and Standards

Technical requirements 84
The baseline of intelligence analytic tradecraft 84
Note 1 – Addressing CTI consumers' interests 85
Note 2 – Access and credibility 85

Note 3 – Articulation of assumptions 86
Note 4 – Outlook 87
Note 5 – Facts and sourcing 87
Note 6 – Analytic expertise 89
Note 7 – Effective summary 89

## 5
Goal Setting, Procedures for CTI Strategy, and Practical Use Cases

- Note 8 – Implementation analysis 90
- Note 9 – Conclusions 90
- Note 10 – Tradecraft and counterintelligence 91
- Understanding and adapting ICD 203 to CTI 92
- Understanding the STIX standard 94
- Using STIX for cyber threat analysis 95
- Specifying threat indicator patterns using STIX 95
- Using the STIX standard for threat response management 96
- Threat intelligence information sharing 96
- Understanding the STIX v2 standard 99
- Understanding the TAXII standard 104
- How TAXII standard works 105
- AFI14-133 tradecraft standard for CTI 109
- Analytic skills and tradecraft 111
- Additional topics covered in AFI14-133 112
- Summary 112

- Technical requirements 114
- Commercial TIPs 123
- The threat intelligence strategy map and goal setting 115
- Open-source TIPs 124
- Objective 1 – Facilitate and support real-time security operations 116
- Case study 1 – CTI for Level 1 organizations 125
- Objective 2 – Facilitate an effective response to cyber threats 117
- Objective 125
- Objective 3 – Facilitate and support the proactive tracking of cyber threats 118
- Strategy 125
- Objective 4 – Facilitate and support the updating and implementation of security governance 119
- Example 126
- TIPs – an overview 120
- Case study 2 – CTI for Level 2 organizations 128
- Case study 3 – CTI for Level 3 organizations 129
- Objective 128
- Objective 129
- Strategy 128
- Strategy 129
- Example 128
