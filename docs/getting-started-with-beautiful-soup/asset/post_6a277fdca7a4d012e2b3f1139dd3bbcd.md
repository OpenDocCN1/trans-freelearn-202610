
![](img/c437561ffd11fadb571896e0a4ca4d18_0_0.png)

## Community Experience Distilled

# Getting Started with Beautiful Soup

Build your own web scraper and learn all about web scraping with Beautiful Soup

Vineeth G. Nair

![](img/c437561ffd11fadb571896e0a4ca4d18_0_1.png)

# Getting Started with Beautiful Soup

Build your own web scraper and learn all about web scraping with Beautiful Soup

**Vineeth G. Nair**

![](img/c437561ffd11fadb571896e0a4ca4d18_1_0.png)

## Getting Started with Beautiful Soup

Copyright © 2014 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: January 2014

Production Reference: 1170114

Published by Packt Publishing Ltd.

Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-78328-955-4

www.packtpub.com

Cover Image by Mohamed Raoof (raoofpmajeed@gmail.com)

## Credits

- **Author**: Vineeth G. Nair
- **Project Coordinator**: Jomin Varghese
- **Reviewers**: John J. Czaplewski, Christian S. Perone, Zhang Xiang
- **Proofreader**: Maria Gould
- **Indexer**: Hemangini Bari
- **Acquisition Editor**: Nikhil Karkal
- **Graphics**: Sheetal Aute, Abhinash Sahu
- **Senior Commissioning Editor**: Kunal Parikh
- **Production Coordinator**: Adonia Jones
- **Commissioning Editor**: Manasi Pandire
- **Cover Work**: Adonia Jones
- **Technical Editors**: Novina Kewalramani, Pooja Nair
- **Copy Editor**: Janbal Dharmaraj

## About the Author

**Vineeth G. Nair** completed his bachelors in Computer Science and Engineering from Model Engineering College, Cochin, Kerala. He is currently working with Oracle India Pvt. Ltd. as a Senior Applications Engineer.

He developed an interest in Python during his college days and began working as a freelance programmer. This led him to work on several web scraping projects using Beautiful Soup. It helped him gain a fair level of mastery on the technology and a good reputation in the freelance arena. He can be reached at `vineethgnair.mec@gmail.com`. You can visit his website at `www.kochi-coders.com`.

My sincere thanks to Leonard Richardson, the primary author of Beautiful Soup. I would like to thank my friends and family for their great support and encouragement for writing this book. My special thanks to Vijitha S. Menon, for always keeping my spirits up, providing valuable comments, and showing me the best ways to bring this book up. My sincere thanks to all the reviewers for their suggestions, corrections, and points of improvement.

I extend my gratitude to the team at Packt Publishing who helped me in making this book happen.

## About the Reviewers

**John J. Czaplewski** is a Madison, Wisconsin-based mapper and web developer who specializes in web-based mapping, GIS, and data manipulation and visualization. He attended the University of Wisconsin - Madison, where he received his BA in Political Science and a graduate certificate in GIS. He is currently a Programmer Analyst for the UW-Madison Department of Geoscience working on data visualization, database, and web application development. When not sitting behind a computer, he enjoys rock climbing, cycling, hiking, traveling, cartography, languages, and nearly anything technology related.

**Christian S. Perone** is an experienced Pythonista, open source collaborator, and the project leader of PyEvolve, a very popular evolutionary computation framework chosen to be part of OpenMDAO, which is an effort by the NASA Glenn Research Center. He has been a programmer for 12 years, using a variety of languages including C, C++, Java, and Python. He has contributed to many open source projects and loves web scraping, open data, web development, machine learning, and evolutionary computation. Currently, he lives in Porto Alegre, Brazil.

**Zhang Xiang** is an engineer working for the Sina Corporation.

> I'd like to thank my girlfriend, who supports me all the time.

## Table of Contents

- Preface — 1
- **Chapter 1: Installing Beautiful Soup** — 7
  - **Installing Beautiful Soup** — 7
    - Installing Beautiful Soup in Linux — 7
      - Installing Beautiful Soup using package manager — 8
      - Installing Beautiful Soup using pip or easy_install — 9
      - Installing Beautiful Soup using pip — 9
      - Installing Beautiful Soup using easy_install — 9
    - Installing Beautiful Soup in Windows — 10
      - Verifying Python path in Windows — 10
    - Installing Beautiful Soup using setup.py — 12
  - **Using Beautiful Soup without installation** — 12
  - **Verifying the installation** — 13
  - **Quick reference** — 13
  - **Summary** — 14
- **Chapter 2: Creating a BeautifulSoup Object** — 15
  - **Creating a BeautifulSoup object** — 15
    - Creating a BeautifulSoup object from a string — 16
    - Creating a BeautifulSoup object from a file-like object — 16
    - Creating a BeautifulSoup object for XML parsing — 18
      - Understanding the features argument — 19
  - **Tag** — 22
    - Accessing the Tag object from BeautifulSoup — 22
    - Name of the Tag object — 23
    - Attributes of a Tag object — 23
  - **The NavigableString object** — 24
  - **Quick reference** — 24
  - **Summary** — 25
- **Chapter 3: Search Using Beautiful Soup** — 27
  - Searching in Beautiful Soup — 27
  - Searching with find() — 28
  - Finding the first producer — 29
  - Explaining find() — 30
  - Searching with find_all() — 37
  - Finding all tertiary consumers — 37
  - Understanding parameters used with find_all() — 38
  - Searching for Tags in relation — 40
  - Searching for the parent tags — 40
  - Searching for siblings — 42
  - Searching for next — 44
  - Searching for previous — 45
  - Using search methods to scrape information from a web page — 46
  - Quick reference — 51
  - Summary — 52
- **Chapter 4: Navigation Using Beautiful Soup** — 53
  - Navigation using Beautiful Soup — 53
  - Navigating down — 55
  - Using the name of the child tag — 55
  - Using predefined attributes — 56
  - Special attributes for navigating down — 59
  - Navigating up — 60
  - The .parent attribute — 60
  - The .parents attribute — 61
  - Navigating sideways to the siblings — 61
  - The .next_sibling attribute — 62
  - The .previous_sibling attribute — 62
  - Navigating to the previous and next objects parsed — 63
  - Quick reference — 63
  - Summary — 64
- **Chapter 5: Modifying Content Using Beautiful Soup** — 65
  - Modifying Tag using Beautiful Soup — 65
  - Modifying the name property of Tag — 66
  - Modifying the attribute values of Tag — 68
  - Updating the existing attribute value of Tag — 68
  - Adding new attribute values to Tag — 69
  - Deleting the tag attributes — 70
  - Adding a new tag — 71
  - Modifying string contents — 73
  - Using .string to modify the string content — 74
  - Adding strings using .append(), insert(), and new_string() — 75
  - **Deleting tags from the HTML document** — 77
    - Deleting the producer using decompose() — 77
    - Deleting the producer using extract() — 78
    - Deleting the contents of a tag using Beautiful Soup — 79
  - **Special functions to modify content** — 80
  - **Quick reference** — 84
  - **Summary** — 86
- **Chapter 6: Encoding Support in Beautiful Soup** — 87
  - **Encoding in Beautiful Soup** — 88
    - Understanding the original encoding of the HTML document — 89
    - Specifying the encoding of the HTML document — 89
  - **Output encoding** — 90
  - **Quick reference** — 92
  - **Summary** — 92
- **Chapter 7: Output in Beautiful Soup** — 93
  - **Formatted printing** — 93
  - **Unformatted printing** — 94
  - **Output formatters in Beautiful Soup** — 95
    - The minimal formatter — 98
    - The html formatter — 98
    - The None formatter — 99
    - The function formatter — 99
  - **Using get_text()** — 100
  - **Quick reference** — 101
  - **Summary** — 102
- **Chapter 8: Creating a Web Scraper** — 103
  - **Getting book details from PacktPub.com** — 103
    - Finding pages with a list of books — 104
    - Finding book details — 107
  - **Getting selling prices from Amazon** — 109
  - **Getting the selling price from Barnes and Noble** — 111
  - **Summary** — 112
- **Index** — 113
