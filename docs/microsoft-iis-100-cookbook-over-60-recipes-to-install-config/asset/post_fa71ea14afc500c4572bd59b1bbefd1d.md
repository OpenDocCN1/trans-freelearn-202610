
## Microsoft IIS 10.0 Cookbook

Task-oriented recipes to manage your web server with ease

Ashraf Khan

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_1_0.png)

BIRMINGHAM - MUMBAI

## Microsoft IIS 10.0 Cookbook

Copyright © 2017 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: June 2017

Production reference: 1220617

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78712-667-1

www.packtpub.com

## About the Author

**Ashraf Khan** works as the Head Of Network Operating System Unit, with a diverse background in production systems, Network Administration, and Solutions Architecture. He has the MCSE-Server, MCSE-Messaging, MCSE-Cloud, CCNA, ITIL & Prince2 certifications. He has more than 13 years of experience in sectors such as IT infrastructure and private and government sectors located in India and the UAE and remote support for USA and worldwide for physical and virtual server environment IT management, implementation, planning, disaster recovery, testing, and technical support.

I would like to express my gratitude to the many people who saw me through this book and to all those who provided support, talked things over, read, wrote, offered comments, allowed me to quote their remarks, and assisted in the editing, proofreading, and design.

I would like to thank my editors Heramb Bhavsar and Sharon Raj and reviewer, Anwarul Haque, for enabling me to publish this book. Above all, I want to thank my wife, Shama, and the rest of my family, who supported and encouraged me in spite of all the times it took me away from them. It was a long and difficult journey for them.

I would like to thank Sharon Raj for helping me in the process of selection and editing. Thanks to Packt who encouraged me.

Last but not least, I beg forgiveness of all those who have been with me over the years and whose names I have failed to mention.

## About the Reviewer

**Anwarul Haque** has been creating applications professionally since 2004. Haque has worked as a consultant for over 12 years in the enterprise and consumer domains. He is a Microsoft SharePoint 2007 Application Development certified since 2007, Microsoft Dynamics NAV 2013 C/Side Development certified since 2014, and Microsoft Certified Professional Developer since 2014. He also worked on SharePoint 2010 and 2013 on-premises, online SharePoint Office 365, and IIS Server management with hosting management skills at the administration level. He managed web servers at various platforms and technology applications hosted on Windows IIS Server, Azure Server, and Unix Server. Apart from IIS Server, he also managed and hosted websites on Apache Tomcat Server.

In the development area, he has worked on various IDEs, tools, and technologies, such as Microsoft Office Designer applications for SharePoint development, Microsoft CRM Portal, ASP.Net, VB.Net, ADO.Net, WCF, SSRS, ASP 3.0, Entity Framework, SQL Server 2005, 2008, 2012, 2015, Oracle, Visual Basic 6.0 & VBA macro, VB Script, JavaScript, Ajax, jQuery, dynamic Excel reports and charts, and add-on developers for Microsoft Office products. He has also used subversion control tools such as Microsoft Visual SourceSafe, SVN, and the IBM Rational ClearCase tool.

As a professional, he has worked with small and large enterprises in Matrix InfoSoft, MetaOption LLC, and currently works as a software analyst in GCE Solutions, which is based in New Delhi.

You can follow him on LinkedIn at https://www.linkedin.com/in/anwarul-haque-b4824b21/.

*I would like to thank my lovely wife, Zeenat Haque, for her patience, support, and understanding!*

- How to do it... 44
- How it works... 44

## Chapter 2: Creating an Application Pool in IIS 10.0 45

### Introduction 45

### Understanding application pools in IIS 10.0 46

- Getting ready 46
- How to do it... 47
- How it works... 49

### Installation of lower framework version 49

- Getting ready 49
- How to do it... 50
- How it works... 57

### Creating different application pools in IIS 10.0 57

- Getting ready 57
- How to do it... 57
- How it works... 60

### Configuration of application pool on IIS 10.0 61

- Getting ready 61
- How to do it... 62
- How it works... 67

### Hosting .NET web page 68

- Getting ready 68
- How to do it... 68
- How it works... 72

### Hosting different versions of a .NET website 73

- Getting ready 73
- How to do it... 73
- How it works... 75

### Testing different versions of a website 75

- Getting ready 76
- How to do it... 76
- How it works... 77

## Chapter 3: Hosting Multiple Websites on IIS 10.0 79

### Introduction 79

### Hosting multiple websites 80

- Getting ready 80
- How to do it... 80
- How it works... 87

### Creating a website folder 87

- Getting ready 87
- How to do it... 87
- How it works... 90

### Configuring websites 90

- Getting ready 91
- How to do it... 91
- How it works... 94

### Configuring ports 94

- Getting ready 94
- How to do it... 94
- How it works... 98

### Configuring the website IP 99

- Getting ready 99
- How to do it... 99
- How it works... 104

### Deploying websites 104

- Getting ready 104
- How to do it... 104
- How it works... 107

### Testing websites 107

- Getting ready 108
- How to do it... 108
- How it works... 110

## Chapter 4: Constructing Virtual Directories in IIS 10.0 111

### Introduction 111

### Constructing a virtual directory in IIS 10.0 112

- Getting ready 112
- How to do it... 112
- How it works... 115

### Understanding IIS 10.0 virtual directories 115

- Getting ready 116
- How to do it... 116
- How it works... 117

### Configuring virtual directories in IIS 10.0 117

- Getting ready 117
- How to do it... 117
- How it works... 122

### Configuring virtual directories with different application pools 122

- How it works... 160

### Creating a wildcard host 160

- Getting ready 160
- How to do it... 161
- How it works... 166

### Configuring a wildcard host 166

- Getting ready 166
- How to do it... 166
- How it works... 170

### Uploading .NET web pages to a wildcard host 171

- Getting ready 171
- How to do it... 171
- How it works... 174

### Testing uploaded website pages 174

- Getting ready 174
- How to do it... 175
- How it works... 179

## Chapter 7: Deploying IIS 10.0 on Nano Server 180

### Introduction 180

### Understanding IIS 10.0 on Nano Server 181

- Getting ready 181
- How to do it... 181
- How it works... 184

### Installing IIS 10.0 on Nano Server 184

- Getting ready 185
- How to do it... 185
- How it works... 194

### Managing IIS 10.0 on Nano Server 194

- Getting ready 194
- How to do it... 194
- How it works... 198

### Creating an IIS 10.0 website on Nano Server 198

- Getting ready 198
- How to do it... 198
- How it works... 202

### Configuring an IIS 10.0 website on Nano Server 202

- Getting ready 202
- How to do it... 202
- How it works... 207

### Uploading website pages 208

- Getting ready 208
- How to do it... 208
- How it works... 210

### Testing uploaded web pages 210

- Getting ready 211
- How to do it... 211
- How it works... 213

## Chapter 8: Configuring IIS Administration with PowerShell Cmdlets 214

### Introduction 214

### IIS administration with PowerShell cmdlets 215

- Getting ready 215
- How to do it... 216
- How it works... 222

### Creating an advanced IIS 10.0 website on Nano Server 222

- Getting ready 222
- How to do it... 222
- How it works... 229

### Configuring IIS 10.0 websites on Nano Server 230

- Getting ready 230
- How to do it... 230
- How it works... 235

### Uploading IIS 10.0 websites to Nano Server 235

- Getting ready 235
- How to do it... 235
- How it works... 239

### Testing uploaded web pages 240

- Getting ready 240
- How to do it... 240

## Chapter 9: Enabling ASP.NET Core with IIS on Nano Server 246

### Introduction 246

### Understanding ASP.NET Core with IIS on Nano Server 247

- Getting ready 247
- How to do it... 247
- How it works... 250

### Configuring ASP.NET Core with IIS on Nano Server 250

- Getting ready 251
