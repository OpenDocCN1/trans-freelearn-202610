
# Data Engineering
with Apache Spark,
Delta Lake, and Lakehouse

Create scalable pipelines that ingest, curate, and aggregate complex data in a timely and secure way

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_0_0.png)

# Data Engineering
with Apache Spark,
Delta Lake, and
Lakehouse

Create scalable pipelines that ingest, curate, and aggregate complex data in a timely and secure way

Manoj Kukreja

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_1_0.png)

# Data Engineering with Apache Spark, Delta Lake, and Lakehouse

Copyright © 2021 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Group Product Manager: Kunal Parikh

Publishing Product Manager: Sunith Shetty

Senior Editor: Davud Sugarman

Content Development Editor: Joseph Sunil

Technical Editor: Sonam Pandey

Copy Editor: Safis Editing

Project Coordinator: Aparna Ravikumar Nair

Proofreader: Safis Editing

Indexer: Tejal Daruwale Soni

Production Designer: Prashant Ghare

First published: October 2021

Production reference: 1170921

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80107-774-3

www.packt.com

In memory of my two fathers, my mother, and my father-in-law, whom I lost recently. Hope you keep showering your blessings from far and beyond. To my darling wife Roopika, a simple thank you is not good enough. To my daughter Saesha who inspired me to write this book, and to my other daughter Shagun who is living my other dream.

I love you all.

– Manoj Kukreja

## Foreword

I don't think it is required anymore to explain the importance data plays in the modern world. Many of the online services we use today are powered by data systems of varying complexity. Machine learning emerged as a powerful tool for certain tasks in big part thanks to modern data platforms that can collect, process, and distribute large volumes of data. These data platforms also provide a strong foundation for data analytics and help drive real world decisions in healthcare, business, and education.

One of the challenges that I have faced when I have started working in data space, which I'm sure is familiar to many of you, is the lack of good books and other educational resources on the topic. There is a lot of documentation on specific tools that you can use, but knowledge of what good architectures look like and which patterns work in which specific cases only resides with a few experts who have done this type of work before. This is especially true when it comes to emerging cloud technologies.

Manoj is one of such experts since he has been implementing data platforms for companies in different industries for the last 10 years. In this book, he will share his knowledge and experience on architecting and implementing modern data platforms using Microsoft Azure, one of the leading cloud providers for data workloads. You will learn practical skills on implementing data lake architectures, implementing data pipelines, and running them in a production environment. Books like this are useful because they give you a download of the knowledge and experience that Manoj has accumulated over the years and you know that recipes and advice he gives will work in a real-life implementation because he has implemented similar systems many times before.

Getting practical data engineering skills is a great investment in your career and knowing patterns that you can rely on will spare you lots of trial and error and will help you get results much faster. Hope you enjoy it!

Danil Zburivsky,
Senior Consultant at Sourced Group
Data Engineering and Cloud Infrastructure specialist
Ottawa, Ontario, Canada

## Contributors

## About the author

**Manoj Kukreja** is a Principal Architect at Northbay Solutions who specializes in creating complex Data Lakes and Data Analytics Pipelines for large-scale organizations such as banks, insurance companies, universities, and US/Canadian government agencies. Previously, he worked for Pythian, a large managed service provider where he was leading the MySQL and MongoDB DBA group and supporting large-scale data infrastructure for enterprises across the globe. With over 25 years of IT experience, he has delivered Data Lake solutions using all major cloud providers including AWS, Azure, GCP, and Alibaba Cloud. On weekends, he trains groups of aspiring Data Engineers and Data Scientists on Hadoop, Spark, Kafka and Data Analytics on AWS and Azure Cloud.

## About the reviewers

**Rajeesh Punathil** is currently a Data Architect with a prominent energy company based in Canada. He leads the digital team in building cloud data analytics solutions aimed to help businesses arrive at right questions and answers. In prior roles he has designed data analytics pipelines in AWS and Azure stack for major companies like Sunlife, CIBC, City National bank, Hudsons Bay etc. He holds a B Tech in Electronics and Telecom Engineering from University of Calicut and has over 15 Years of IT experience. He consider himself as a forever student, and his constant striving for knowledge has made him an expert and leader in the field of data analytics and big data. He has completed AWS Big Data Analytics certification to his credit.

**Dang Trung Anh** is a principal engineer and software architect with over 15 years of experience in delivering big software systems for various customers across many areas. He has spent most of his career writing many software systems that support millions of users. He has had various roles from an ordinary full-stack developer, through software architect, to director of engineering. He is the author of many viral articles related to software engineering on Medium.

# Table of Contents

Preface

## Section 1:
Modern Data Engineering and Tools

### 1
The Story of Data Engineering and Analytics

- The journey of data 4
- computing 8
- Exploring the evolution of data analytics 5
- Adoption of cloud computing 10
- Data storytelling 11
- Core capabilities of storage and compute resources 7
- The monetary power of data 13
- Availability of varying datasets 7
- Organic growth 13
- The paradigm shift to distributed
- Summary 17

### 2
Discovering Storage and Compute Data Lakes

- Introducing data lakes 20
- Discovering data lake architectures 33
- Exploring the benefits of data lakes 21
- The CAP theorem 34
- Adhering to compliance frameworks 24
- Segregating storage and compute in a data lake 26
- Summary 51

### 3

# Data Engineering on Microsoft Azure

- Introducing data engineering in Azure 53
- Performing data engineering in Microsoft Azure 56
- Data processing services in Microsoft Azure 65
- Self-managed data engineering services (IaaS) 56
- Data engineering as a service (SaaS) 68
- Azure-managed data engineering services (PaaS) 59
- Data cataloging and sharing services in Microsoft Azure 75
- Opening a free account with Microsoft Azure 76
- Summary 77

## Section 2: Data Pipelines and Stages of Data Engineering

### 4

# Understanding Data Pipelines

- Exploring data pipelines 82
- Components of a data pipeline 82
- Process of creating a data pipeline 85
- Discovery phase 86
- Design phase 88
- Development phase 92
- Deployment phase 93
- Running a data pipeline 93
- Sample lakehouse project 95
- Summary 96

### 5

# Data Collection Stage - The Bronze Layer

- Architecting the Electroniz data lake 98
- The cloud architecture 100
- The pipeline design 103
- The deployment strategy 104
- Understanding the bronze layer 105
- Configuring data sources 106
- Data preparation 106
- Configuring data destinations 117
- Building the ingestion pipelines 119
- Building a batch ingestion pipeline 119
- Testing the ingestion pipelines 141
