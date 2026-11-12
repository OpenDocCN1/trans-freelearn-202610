
## UiPath Associate Certification Guide

The go-to guide to passing the Associate certification exam with the help of mock tests and quizzes

Niyaz Ahmed | Lahiru Fernando | Rajaneesh Balakrishnan

## UiPath Associate Certification Guide

The go-to guide to passing the Associate certification exam with the help of mock tests and quizzes

Niyaz Ahmed

Lahiru Fernando

Rajaneesh Balakrishnan

![](img/84b06ad2832c53c5f677f8ce4c80f184_1_0.png)

BIRMINGHAM—MUMBAI

## UiPath Associate Certification Guide

Copyright © 2022 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

- **Group Product Manager**: Rohit Rajkumar
- **Publishing Product Manager**: Ashitosh Gupta
- **Senior Editor**: Hayden Edwards
- **Content Development Editor**: Feza Shaikh
- **Technical Editor**: Saurabh Kadave
- **Copy Editor**: Safis Editing
- **Project Coordinator**: Manthan Patel
- **Proofreader**: Safis Editing
- **Indexer**: Tejal Daruwale Soni
- **Production Designer**: Joshua Misquitta
- **Marketing Coordinator**: Deepak Kumar

First published: June 2022
Production reference: 1170622

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80107-273-1
www.packt.com

To my father – your guiding hand on my shoulder will remain with me forever.
– Niyaz Ahmed

I'd like to thank my father, Sesha, my wife, Mihiri, and my daughter, Emmie, for holding my hand throughout all the hardships of my life. Also, special thanks go to my two amazing mentors, Erik Gillet and Lucas Pimenta, for always motivating and guiding me. Lastly, I would like to thank the entire UiPath Community, the UiPath team, my team at Boundaryless Group, and all my friends for all the opportunities and for helping me change my life in many ways.
– Lahiru Fernando

To anyone who wants to pass the UiPath Associate certification, start learning UiPath, change to a career in UiPath, or simply know about the technology – I'm sure this book will undoubtedly help you achieve your UiPath goals. Thanks to the amazing team efforts by the co-authors, reviewers, and the Packt Publishing team in creating the contents in the most simplified way to help anyone learn UiPath and earn the UiPath Associate Certification.

To my wife, Anusree, son, Saijesh, and daughter, Dakshya, for kindly letting me experience the writing process of this book.
– Rajaneesh Balakrishnan

## Contributors

## About the authors

**Niyaz Ahmed** is a technical program manager at UiPath, where he is working toward enabling the future and current workforce for the automation-ready world. He manages the UiPath Academic Alliance and Learning Partner program by working with leading institutions, including universities, colleges, and high schools. He is also an instructor at Udacity for the RPA Developer Nanodegree program. In addition, he continues to evangelize RPA by mentoring learners to develop professional-level skills focused on developing and deploying software robots.

> *A special thanks to Alok Shrivastava, Rajesh Nambiar, Leah Hojem, and all my colleagues at UiPath who have helped me in the different phases of the book.*

**Lahiru Fernando** is a UiPath MVP, executive team lead for RPA, UiPath community leader, mentor, lecturer, and public speaker, and has over 11+ years of experience in the IT industry. He started his career in 2011 after completing a degree in specialized software engineering at Staffordshire University. He has been engaged in multiple business intelligence projects in the banking, finance, market research, and agriculture sectors. Moving into RPA in 2019, he has led multiple successful RPA projects in the finance, retail, banking, child welfare, oil and gas, telecommunication, and apparel sectors as a lead RPA solution architect. He also leads the RPA enablement and training fronts for Boundaryless Automation.

Rajaneesh Balakrishnan is the founder of 2AutomateAnything and has been a UiPath Advanced Certified RPA developer and UiPath MVP since 2020. He started his career as a quality analyst, progressed to become a software testing practice lead, and then became an RPA lead with immense automation tool implementation and multiple domain experience, including BFSI. Having over 15 years of experience in IT, he has successfully implemented RPA in UiPath for many reputable clients and has saved a significant amount of time and cost to organizations. He is also a passionate teacher, conducting various corporate and personalized training events and guiding the careers of many toward RPA in UiPath. He used to share educational content on YouTube, Udemy, and LinkedIn to support the UiPath community.

## About the reviewers

After earning his master’s degree in computer science, **Mukesh Kala** started his career as a software developer. He has over 8 years of professional experience and presently works as a senior RPA analyst, automating work in the banking and financial services industry.

A strong believer in the power of learning and knowledge sharing, Mukesh regularly attends UiPath community events, training and helping out beginners in technology. Mukesh was recognized as a UiPath MVP in 2021 and is behind the YouTube channel _Tutorials by Mukesh Kala_. In his leisure time, he loves to read books and speedcubing.

**Andy Menon** is a UiPath MVP for 2021. In his day job, he is an integrations engineer, with experience in integrating systems for the enterprise.

Andy is also the founder of RPA Vanguard and the namesake channel on YouTube, teaching RPA with real-world examples. His Udemy course on enterprise RPA is ranked amongst the top 20% of almost 100,000 courses on CourseMarks!

In his free time, Andy dabbles in hobby electronics, 3D printing, and the **Internet of Things (IoT)**, experimenting with Arduino, Raspberry Pi, and the ESP8266 family of microcontrollers.

Andy is also a Kindle-published author of the book _Give Yourself IoT Super Powers_.

He has published several of his hobby projects and designs online at Instructables and Thingiverse.

## Table of Contents

## Preface

## Part 1: Importance of RPA

### 1 The Automation Journey and Identifying Suitable Business Processes

- Technical requirements ... 4
- Understanding why automation is driving digital transformation ... 5
- Implementing RPA in business ... 7
- Payroll processing ... 9
- Client information management ... 9
- Invoice processing ... 10
- Financial statement reconciliation ... 10
- Call center automation ... 10
- Loan processing ... 11
- What can RPA automate? ... 11
- Identifying processes for RPA ... 13
- Understanding the stages of an RPA journey ... 17
- Discovery ... 17
- Build ... 18
- Manage ... 19
- Run ... 19
- Engage ... 19
- Measure ... 19
- Knowing the RPA resources and responsibilities in an RPA project ... 20
- RPA solution architect ... 20
- RPA developer ... 20
- BA ... 20
- Implementation manager ... 21
- Infrastructure engineer/manager ... 21
- RPA support ... 21
- Summary ... 21

### 2 UiPath Ecosystem

- Technical requirements 24
- An overview of the UiPath Enterprise RPA platform 24
- Discover 26
- Build 27
- Manage 28
- Run 29
- Engage 29
- Setting up UiPath Studio 30
- Exploring the different editions 30
- Installing UiPath Studio 31

- Exploring the UiPath Studio interface 36
- The Designer panel 38
- The Project panel 38
- The Activities panel 40
- The Properties panel 40
- The Variables panel 41
- The Argument panel 42
- The Output panel 43
- The Outline panel 44
- Summary 44

### 3 Introducing UiPath Orchestrator

- Technical requirements 46
- Creating your first cloud Orchestrator instance 46
- Introducing the UiPath Cloud platform 47
- Home screen and Resource Center 48
- Actions 48
- Apps 49
- Processes 51
- Data Service 52
- AI Fabric 53
- Admin 53
- Introducing UiPath Orchestrator 58

- Understanding the fundamental concepts of UiPath Orchestrator 58
- Robots 59
- Folders 60
- Environment 62
- Organizational units 62
- Packages 62
- Processes 64
- Job 66
- Triggers 68
- Assets 72
- Queues 75
- Summary 79
