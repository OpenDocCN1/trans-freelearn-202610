
# Microsoft Sentinel
in Action

# Second Edition

Architect, design, implement, and operate Microsoft Sentinel as the core of your security solutions

# Microsoft Sentinel
in Action
*Second Edition*

Architect, design, implement, and operate Microsoft Sentinel as the core of your security solutions

Richard Diver

Gary Bushey

John Perkins

Packt
BIRMINGHAM—MUMBAI

# Microsoft Sentinel in Action

# Second Edition

Copyright © 2022 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Group Product Manager: Vijin Boricha

Publishing Product Manager: Meeta Rajani

Senior Editor: Arun Nadar

Content Development Editor: Sulagna Mohanty

Technical Editor: Arjun Varma

Copy Editor: Safis Editing

Project Coordinator: Shagun Saini

Proofreader: Safis Editing

Indexer: Vinayak Purushotham

Production Designer: Vijay Kamble

First published: May 2020

Second edition: January 2022

Production reference: 1021221

Published by Packt Publishing Ltd.

Livery Place

35 Livery Street

Birmingham

B3 2PB, UK.

ISBN 978-1-80181-553-6

www.packt.com

## Contributors

## About the authors

**Richard Diver** is a senior technical business strategy manager for the Microsoft Security Solutions group, focused on developing security partners. Based in Chicago, Richard works with advanced security and compliance partners to help them build solutions across the entire Microsoft platform, including Microsoft Sentinel, Microsoft Defender, Microsoft 365 security solutions, and many more. Prior to Microsoft, Richard worked in multiple industries and for several Microsoft partners to architect and implement cloud security solutions for a wide variety of customers around the world. Any spare time he gets is usually spent with his family.

**Gary Bushey** is an Azure security expert with over 25 years of IT experience. He got his start early on when he helped his fifth-grade math teacher with their programming homework and worked all one summer to be able to afford his first computer, a Commodore 64. When he sold his first program, an apartment management system, at 14 he was hooked. During his career, he has worked as a developer, consultant, trainer, and architect. When not spending time in front of a computer, you can find him hiking in the woods, taking pictures, or just picking a direction and finding out what is around the next corner.

**John Perkins** is the founder and principal of Threat Angler, a cybersecurity service provider that specializes in managed services, professional services, and training with a focus on delivering cybersecurity outcomes to customers of all shapes and sizes. John has over 20 years of experience in cybersecurity and has contributed to nearly all cybersecurity disciplines during his career. He has experience with numerous applications, including Microsoft Sentinel, and has designed, built, and led managed security services for several large service providers. In his free time, John enjoys spending time with his family, traveling, and staying active.

## About the reviewers

**Ashwin Patil** currently works as a senior program manager for **Microsoft Threat Intelligence Center (MSTIC)** and has over 10 years' experience entirely focused on security monitoring and incident response, defending enterprise networks. In his current role, he primarily works on threat hunting, detection research in **Kusto Query Language (KQL)** for Microsoft Sentinel, and developing Jupyter notebooks written in Python/R to do threat hunting and investigation across a variety of cloud and on-premises security event log data sources. He has a bachelor's degree in computer engineering and possesses various SANS certifications, including GCIA, GCFE, and GCIH in the field of **Digital Forensics and Incident Response (DFIR)**.

**Dennis Pike** is the original sales engineer at Island, a stealth mode security startup. He would tell you more, but they may put him on an island with nothing but a volleyball. Born in Kentucky, he surprisingly can't stand bourbon and ended up a nationally ranked beer judge instead. He holds a BSc in systems engineering from the University of Virginia and has spent the last 25 years working in IT, including as a Global Black Belt – Advanced Security Analytics at Microsoft where he focused on Microsoft Sentinel.

> *I want to thank my wife, Heather, for her patience, love, and support.*

**Rod Trent** is a senior cloud security advocate for Microsoft and an Microsoft Sentinel global SME helping customers migrate from existing SIEMs to Microsoft Sentinel to achieve the promise of better security through improved efficiency without compromise. He is a husband, dad, and first-time grandfather (so speak slowly and loudly). He spends his spare time (if such a thing does truly exist) simultaneously watching *Six Million Dollar Man* episodes and writing KQL queries.

# 4

# Integrating Threat Intelligence with Microsoft Sentinel

Introduction to TI 92

Understanding STIX and TAXII 94

Choosing the right intel feeds for your needs 95

Implementing TI connectors 96

Enabling the data connector 96

Registering an app in Azure AD 98

Configuring the MineMeld TI feed 104

Confirming the data is being ingested for use by Microsoft Sentinel 113

Summary 116

Questions 116

Further reading 116

# 5

# Using the Kusto Query Language (KQL)

Running KQL queries 118

Introduction to KQL commands 120

Tabular operators 122

Query statements 139

The let statement 139

Scalar functions 140

The ago() function 140

String operators 140

Summary 142

Questions 142

Further reading 142

# 6

# Microsoft Sentinel Logs and Writing Queries

An introduction to the Microsoft Sentinel Logs page 144

Navigating through the Logs page 144

The page header 145

The Tables pane 155

The Queries pane 159

The Functions pane 159

The Filter pane 160

The KQL code window 164

Running a query 168

The Results window 168

Learn more 176

Reviewing alternative storage options 87

Summary 89

Questions 89

Further reading 89

# Section 3: Security Threat Hunting

# 7

# Creating Analytic Rules

An introduction to Microsoft Sentinel Analytics 185

Types of analytic rules 186

Navigating through the Analytics home page 188

Creating an analytic rule 197

Creating a rule from a rule template 197

Creating a new rule using the wizard 198

Managing analytic rules 218

Summary 219

Questions 220

Further reading 220

# 8

# Creating and Using Workbooks

An overview of the Workbooks page 222

The workbook header 223

The Templates view 224

Workbook detail view 225

Missing required data types 225

Saved template buttons 226

Walking through an existing workbook 228

Creating workbooks 230

Creating a workbook using a template 231

Creating a new workbook from scratch 232

Editing a workbook 234

Advanced editing 237

Managing workbooks 239

Workbook step types 241

Text 242

Query 243

Metric 247

Parameters 248

Links/tabs 254

Groups 258

Advanced Settings 260

Style 264

Summary 265

Questions 265

Further reading 266

Writing a query 176

The billable data ingested 177

Map view of logins 178

Other useful tables 179

Summary 180

Questions 181

Further reading 181
