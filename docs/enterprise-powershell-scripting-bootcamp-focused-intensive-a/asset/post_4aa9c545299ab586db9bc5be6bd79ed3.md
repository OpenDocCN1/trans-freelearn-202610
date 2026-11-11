
Brenton J.W. Blawat

## Enterprise PowerShell
Scripting Bootcamp

The fastest way to learn PowerShell scripting

Packt

![](img/a27a138d6586c10902d8e86ab3384834_0_0.png)

## Enterprise PowerShell Scripting Bootcamp

The fastest way to learn PowerShell scripting

Brenton J.W. Blawat

![](img/a27a138d6586c10902d8e86ab3384834_2_0.png)

BIRMINGHAM - MUMBAI

## Enterprise PowerShell Scripting Bootcamp

Copyright © 2017 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: May 2017

Production reference: 1160517

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham B32 5PB, UK.

ISBN 978-1-78728-828-7

www.packtpub.com

## Credits

| **Author** | **Project Coordinator** |
|---|---|
| Brenton J.W. Blawat | Shweta H Birwatkar |
| **Reviewer** | **Proofreader** |
| Tim Amico | Safis Editing |
| **Commissioning Editor** | **Indexer** |
| Kartikey Pandey | Tejal Daruwale Soni |
| **Acquisition Editor** | **Graphics** |
| Namrata Patil | Tania Dutta |
| **Content Development Editor** | **Production Coordinator** |
| Amrita Noronha | Nilesh Mohite |
| **Technical Editor** | **Cover Work** |
| Jovita Alva | Nilesh Mohite |
| **Copy Editors** |  |
| Safis Editing |  |
| Laxmi Subramanian |  |

## About the Author

**Brenton J.W. Blawat** is an entrepreneur, strategic technical advisor, multi-published author, and enterprise architect who has a passion for the procurement of technology in profit-based organizations. Brenton is business-centric, while technology minded, and has many years of experience bridging the gap between technical staff and decision makers in organizations. He takes pride in his ability to effectively communicate to a diverse audience and provide strategic direction for large and small organizations alike.

Since 2013, he has authored and published multiple books with Packt Publishing, including:

- *PowerShell 3.0 WMI Starter*
- *(Co-authored) PowerShell: Automating Administrative Tasks*
- *Mastering Windows PowerShell Scripting*

Brenton currently works at CDW as an enterprise architect in Strategic Solutions and Services. CDW is a leading multibrand technology solutions provider to business, government, education, and healthcare organizations in the United States, Canada, and the United Kingdom. A Fortune 500 company with multinational capabilities, CDW was founded in 1984 and employs approximately 8,500 workers. For the year ending December 31, 2016, the company generated net sales of nearly $14 billion. For more information about CDW, visit www.CDW.com.

As the author of this book, Brenton always extends himself to the PowerShell community. You can follow Brenton on Twitter at @brentblawat or his blog at http://www.masteringposh.com. The author is always open for discussions on the book and will provide feedback to readers, as time allows. If you are an academic institution, Brenton engages with multiple colleges for guest speaking. You can contact Brenton on Twitter for more information.

Happy coding!

## About the Reviewer

**Tim Amico** is a passionate technology leader with years of consulting experience around designing, implementing, and managing Endpoint and Mobility management infrastructures for small organizations to large global enterprise environments. He has continued to define technology solutions that help solve complex business problems and deliver highly customized solutions to clients.

He currently works at LMI as a systems engineer, managing their System Center Configuration Manager, Active Directory, and Azure IaaS environments. LMI is a not-for-profit government consulting firm that designs and implements solutions to some of the toughest problems facing government managers in logistics, information technology, and resource allocation. For more information about LMI, visit www.lmi.org.

You can follow Tim on twitter at @tim_amico or his blog at http://www.deploymentlife.com.

## eBooks, discount offers, and more

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at customercare@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/a27a138d6586c10902d8e86ab3384834_7_0.png)

https://www.packtpub.com/mapt

Get the most in-demand software skills with Mapt. Mapt gives you full access to all Packt books and video courses, as well as industry-leading tools to help you plan your personal development and advance your career.

## Why subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print, and bookmark content
- On demand and accessible via a web browser

## Customer Feedback

Thanks for purchasing this Packt book. At Packt, quality is at the heart of our editorial process. To help us improve, please leave us an honest review on this book's Amazon page at https://www.amazon.com/dp/1787288285.

If you'd like to join our team of regular reviewers, you can e-mail us at customerreviews@packtpub.com. We award our regular reviewers with free eBooks and videos in exchange for their valuable feedback. Help us be relentless in improving our products!

Digitized by the Internet Archive
in 2023 with funding from
Kahle/Austin Foundation

I would like to dedicate this book to my wife, Rachel, whose love and support drives me to be a better person every day.

## Table of Contents

- Preface v
- Chapter 1: Getting Started with Enterprise PowerShell Scripting 1
  - Windows server scanning script 2
  - Performance considerations 3
  - PowerShell scripting templates 4
  - Summary 5
- Chapter 2: Script Structure, Comment Blocks, and Script Logging 7
  - Overall script structure 8
  - Comment blocks 9
  - Script logging 11
    - Creating the logging files 11
    - Creating a windows event log source 14
    - Creating the logging function 15
  - Summary 20
- Chapter 3: Working with Answer Files 21
  - Answer files 22
    - XML answer files 23
    - Reading XML answer files 25
    - XML tag function 27
  - Summary 31
- Chapter 4: String Encryption and Decryption 33
  - RijndaelManaged encryption 34
    - Creating random salt, initialization vector, and passwords 36
  - Encrypting and decrypting strings 38
    - Decrypting strings 45
    - Securing the password 48
      - Decryption with encoded password 50
  - Summary 54
- **Chapter 5: Interacting with Services, Processes, Profiles, and Logged on Users** 55
  - Windows services 56
  - Managing Windows processes 64
  - Identifying logged on users 69
  - User profiles 71
  - Summary 75
- **Chapter 6: Evaluating Scheduled Tasks** 77
  - Scheduled tasks 78
  - Identifying scheduled tasks running as alternate users 86
  - Summary 88
- **Chapter 7: Determining Disk Statistics** 89
  - Disk statistics 90
    - Retrieving physical and logical disk information 90
    - The DriveType property 92
    - Converting disk space to MB and GB 94
    - Free Space property 96
  - Disk information script 98
  - Summary 101
- **Chapter 8: Windows Features and Installed Software Detection** 103
  - Windows features 104
  - Installed software detection 108
    - Scanning executables in \Program Files 111
  - Summary 114
- **Chapter 9: File Scanning** 115
  - File scanning 115
  - Excluding long paths 118
  - Excluding files 123
  - Summary 128
- **Chapter 10: Optimizing Script Execution Speed** 129
  - Script execution speed 130
    - Speed considerations for the write-host cmdlet 131
    - Speed considerations for the write-progress cmdlet 137
    - Speed consideration - switches versus if statements 140
  - Summary 145
- **Chapter 11: Improving Performance by Using Regular Expressions** 147
  - Using regular expressions 148
  - Dynamically building expressions 151
