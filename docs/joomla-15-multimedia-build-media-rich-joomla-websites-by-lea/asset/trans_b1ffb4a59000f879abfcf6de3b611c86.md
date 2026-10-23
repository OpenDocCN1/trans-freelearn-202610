

![](img/0151410972a5c5ea0aab26416e44b1da_0_0.png)

# Joomla! 1.5 多媒体

通过学习嵌入和显示多媒体内容来构建媒体丰富的 Joomla! 网站

# Joomla! 1.5 Multimedia

Build media-rich Joomla! websites by learning to embed and display multimedia content

Allan Walker

![](img/0151410972a5c5ea0aab26416e44b1da_1_0.png)

# Joomla! 1.5 Multimedia

Copyright © 2010 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: January 2010

Production Reference: 1250110

Published by Packt Publishing Ltd.
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-847197-70-2

www.packtpub.com

Cover Image by Filippo (filosarti@tiscali.it)

## About the Author

**Allan Walker** grew up in New Zealand, and now lives in the United Kingdom, where he runs an online business consultancy and development company called "Amplify" (www.projectamplify.com). In recent years, Allan has been involved with a number of large-scale Joomla! projects, helping to establish Joomla! within the UK government and corporate sectors.

Allan has been a mentor for the 2009 Joomla! Google Summer of Code program, and is a member of the Joomla! Marketing Team.

> > When first starting to write this title, I assumed a couple of full-on weekends might break the back of the book. Nine months later, I have a better understanding of what efforts go into a project like this.

I would like to thank anyone who has contributed in some way to the development of Joomla!. The product provides a diverse solution for projects, and through the ever-growing extensions directory, most users can now configure a Joomla!-based project, rather than have to build it. It is the design and simplicity of Joomla! which leads to its success, thus allowing non-technical users to build complex projects easily.

For many of us who have based services around Joomla!, it has helped put food on the table. For this I am grateful. It is the generosity of people's time, skills, and the community support that keeps the project rolling, and Joomla!'s features improving with every release. If you are interested in helping the product and community, please see information about contributing at www.joomla.org.

For the lost evenings and weekends, my love and thanks go to my wife Georgina. Thank you for the continued support and patience.

## About the Reviewers

**Jose Argudo** is a web developer from Valencia, Spain. After finishing his studies he started working for a web design company. Then, six years later, he decided to start working as a freelancer.

Now that some years have passed, he thinks it's the best decision he has ever taken, a decision that let him work with the tools he likes, such as Joomla!, CodeIgniter, CakePHP, jQuery, and other known open source technologies.

His desire to learn and share his knowledge has led him to be a regular reviewer of books from Packt, such as Joomla! With Flash (http://www.packtpub.com/joomla-with-flash/book), Joomla! 1.5 SEO (http://www.packtpub.com/joomla-1-5-search-engine-optimization-seo/book), Magento 1.3 Theme Design (http://www.packtpub.com/magento-1-3-theme-design/book), or Symfony 1.3 web application development (http://www.packtpub.com/symfony-1-3-web-application-development/book).

Recently he has even published his own book, CodeIgniter 1.7, which you can also find at Packt's site (http://www.packtpub.com/improve-coding-productivity-with-codeigniter-1-7/book). If you work with PHP... take a look at it!

If you want to know more about him, you can check his site www.joseargudo.com.

> > To my girlfriend and to my brother, I wish we will always be together.

**Joseph L. LeBlanc** is a freelance Joomla! developer specializing in Joomla!, PHP, and JavaScript. Joseph can be found teaching Joomla! classes and speaking at conferences. He has authored a book on Joomla! extension programming and a Lynda.com video series on using Joomla!.

## Table of Contents

| Contents | Page |
|---|---:|
| **Preface** | **1** |
| **Chapter 1: Getting Started with Joomla! Multimedia** | **7** |
| &nbsp;&nbsp;**What is multimedia?** | **7** |
| &nbsp;&nbsp;&nbsp;&nbsp;Categories of multimedia | 8 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Linear | 8 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Non-linear | 8 |
| &nbsp;&nbsp;**What is Joomla!?** | **10** |
| &nbsp;&nbsp;&nbsp;&nbsp;Including multimedia in your Joomla! site | 12 |
| &nbsp;&nbsp;**Multimedia use in your Joomla! CMS** | **14** |
| &nbsp;&nbsp;&nbsp;&nbsp;Text | 15 |
| &nbsp;&nbsp;&nbsp;&nbsp;Images | 16 |
| &nbsp;&nbsp;&nbsp;&nbsp;Audio | 17 |
| &nbsp;&nbsp;&nbsp;&nbsp;Video | 19 |
| &nbsp;&nbsp;**Multimedia and web accessibility** | **20** |
| &nbsp;&nbsp;&nbsp;&nbsp;Can a multimedia website be accessible? | 22 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The Web Content Accessibility Guidelines (WCAG) | 22 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Section 508 | 22 |
| &nbsp;&nbsp;&nbsp;&nbsp;More information on web accessibility | 23 |
| &nbsp;&nbsp;&nbsp;&nbsp;Web browser support for multimedia | 24 |
| &nbsp;&nbsp;**Summary** | **26** |
| **Chapter 2: Managing Your Joomla! Media** | **27** |
| &nbsp;&nbsp;**Overview of the Joomla! Media Manager** | **28** |
| &nbsp;&nbsp;**Uploading media using the Media Manager** | **28** |
| &nbsp;&nbsp;&nbsp;&nbsp;Media Manager settings | 28 |
| &nbsp;&nbsp;&nbsp;&nbsp;The view | 32 |
| &nbsp;&nbsp;&nbsp;&nbsp;Organizing your content | 33 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Creating a new directory | 34 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Deleting files | 34 |

Uploading your content 35
  Single file upload 35
  Multiple file uploads 36
Managing media using the Media Manager 36
Alternative method of managing files and media 37
  FTP clients 38
  FTP programs 38
  Connecting using an FTP program 39
FTP via a web browser 42
  Connecting to FTP via a web browser 43
  Uploading and downloading 45
  Web browser plugins 45
  Third-party file management extensions 47
  eXtplorer 47
  Joomla! Flash Uploader 48
Summary 48
Chapter 3: Text, Characters, and Fonts in Your Joomla! Site 49
Overview of text, characters, and fonts 50
Fonts and their effect on website users 52
Which font is best to use? 53
  Sans-serif fonts 54
    Verdana 54
    Arial and Helvetica 54
    Trebuchet MS 54
    Lucinda Sans family 54
  Serif fonts 55
    Times New Roman / Times 55
    Georgia 55
    Palatino Linotype / Palatino 55
Joomla! Templates and Cascading Style Sheets 56
  What is a Joomla! Template? 56
    Template CSS 57
    CSS font properties 59
    Meet the units 61
Alternative methods to use custom fonts in your web pages 65
Joomla! text and typography extensions 66
  Frontend font size adjuster (M) 67
  JsIFR3 (P) 67
  capDropper (P) 68
Browser support and accessibility around fonts 68
Summary 71

# Chapter 4: Adding and Managing Image Content ........................................ 73

+   **Images and why we use them in websites** ........................................ 73
+   **Image formats and which ones to use** ........................................ 75
+   　Digital images ........................................ 76
+   　Lossy and lossless data compression ........................................ 77
+   　Image formats ........................................ 78
+   　　GIF ........................................ 79
+   　　JPEG ........................................ 80
+   　　PNG ........................................ 80
+   　　SVG ........................................ 81
+   **Including images in your content articles and modules** ........................................ 82
+   　Adding an image using the Joomla! Article image button ........................................ 82
+   　Adding an image using the Editor Image button ........................................ 86
+   　Image placement using custom HTML code ........................................ 88
+   **Creating image galleries and slideshows** ........................................ 93
+   　Image management extensions, and how to install them ........................................ 94
+   　　Image gallery extensions ........................................ 96
+   　　Image slideshows ........................................ 103
+   **Joomla! Template images** ........................................ 106
+   　How to adjust your template images ........................................ 109
+   **Browser support and image accessibility** ........................................ 112
+   **Summary** ........................................ 113

# Chapter 5: Using Audio in Your Joomla! Website ........................................ 115

+   **What is audio?** ........................................ 115
+   **Audio and the Internet** ........................................ 116
+   **Audio formats** ........................................ 117
+   　Audio compression ........................................ 118
+   　　Audio codecs ........................................ 118
+   　Audio file format ........................................ 118
+   　Bit rates ........................................ 120
+   　How to compress and encode audio ........................................ 121
+   **Embedding and displaying audio in Joomla! Articles** ........................................ 122
+   　The source ........................................ 123
+   　The encoding ........................................ 123
+   　The upload ........................................ 123
+   　　Via FTP ........................................ 123
+   　　Via the Media Manager ........................................ 124
+   　Editors ........................................ 125
+   　Creating the article ........................................ 127

