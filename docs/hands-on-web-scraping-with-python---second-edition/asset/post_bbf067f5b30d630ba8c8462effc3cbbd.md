
![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_0_0.png)

# Hands-On
# Web Scraping with Python

Extract quality data from the web using effective Python techniques

2ND EDITION

ANISH CHAPAGAIN

# Hands-On
## Web Scraping with Python

Extract quality data from the web using effective
Python techniques

Anish Chapagain

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_1_0.png)

BIRMINGHAM—MUMBAI

# Hands-On Web Scraping with Python

Copyright © 2023 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Group Product Manager: Rohit Rajkumar

Publishing Product Manager: Bhavya Rao

Book Project Manager: Aishwarya Mohan

Senior Editor: Rashi Dubey

Technical Editor: Simran Ali

Copy Editor: Safis Editing

Language Support Editor: Safis Editing

Proofreader: Safis Editing

Indexer: Tejal Daruwale Soni

Production Designer: Shankar Kalbhor

DevRel Marketing Coordinators: Nivedita Pandey and Namita Velgekar

First published: July 2019
Second edition: October 2023

Production reference: 1050923

Published by Packt Publishing Ltd.
Grosvenor House
11 St Paul's Square
Birmingham
B3 1RB, UK.

ISBN 978-1-83763-621-1

www.packtpub.com

To my daughter, Aasira, and my family and friends.
To my wife, Rakshya, for being my loving partner throughout my life journey.

Special thanks to Atmeshwar Chapagain, Shiba Chapagain, Ashish Chapagain, Peter, and Prof. W. J. Teahan. This book is dedicated to you all.

- Anish Chapagain

# About the reviewers

**Sanjan Rao** is a senior data scientist with over five years of experience in machine learning, deep learning, and cybersecurity solutions. He has a master’s degree with a specialty in machine learning and artificial intelligence. His primary interests lie in natural language processing, natural language inference, and reinforcement learning.

**Joel Edmond Nguemeta** is a full stack web developer from Cameroon. He is highly experienced in software development with Python, Django, JavaScript, and React in both monolithic and microservices architectures. He has worked as a freelance mentor/assessor at OpenClassrooms, supporting students in Python/Django and JavaScript/React application development, as well as assessing them in the Python/Django and JavaScript/React application developer courses. Furthermore, he has worked as a mobile developer at Tamangwa Shipping. He spends his time sharing his knowledge with everyone through his YouTube channel (proxidev). He is the co-founder of the start-up EasyLern.

# Table of Contents

Preface xiii

# Part 1: Python and Web Scraping

1

# Web Scraping Fundamentals 3

Technical requirements 4

CSS 16

What is web scraping? 4

Data-finding techniques used in web pages 17

Understanding the latest web technologies 5

HTML source page 17

HTTP 5

Developer tools 19

HTML 9

Summary 25

XML 12

Further reading 26

JavaScript 13

2

# Python Programming for Data and Web 27

Technical requirements 28

URL handling and operations 39

Why Python (for web scraping)? 28

requests – Python library 40

Accessing the WWW with Python 31

Implementing HTTP methods 48

Setting things up 31

GET 49

Creating a virtual environment 33

POST 50

Installing libraries 35

Summary 52

Loading URLs 37

Further reading 52

# Part 2: Beginning Web Scraping

# 3

# Searching and Processing Web Documents 57

- Technical requirements 57
- Introducing XPath and CSS selectors to process markup documents 58
- The Document Object Model (DOM) 59
- XPath 60
- CSS selectors 64
- Using web browser DevTools to access web content 68
- HTML elements and DOM navigation 69
- XPath and CSS selectors using DevTools 71
- Scraping using lxml – a Python library 72
- lxml by example 73
- Web scraping using lxml 77
- Parsing robots.txt and sitemap.xml 81
- The robots.txt file 81
- Sitemaps 83
- Summary 84
- Further reading 85

# 4

# Scraping Using PyQuery, a jQuery-Like Library for Python 87

- Technical requirements 87
- PyQuery overview 88
- Introducing jQuery 89
- Exploring PyQuery 89
- Installing PyQuery 89
- Loading a web URL 91
- Element traversing, attributes, and pseudo-classes 91
- Iterating using PyQuery 95
- Web scraping using PyQuery 96
- Example 1 – scraping book details 97
- Example 2 – sitemap to CSV 99
- Example 3 – scraping quotes with author details 102
- Summary 107
- Further reading 108

# 5

# Scraping the Web with Scrapy and Beautiful Soup 109

- Technical requirements 109
- Web parsing using Python 110
- Introducing Beautiful Soup 111
- Installing Beautiful Soup 111
- Exploring Beautiful Soup 112
- Web scraping using Beautiful Soup 121
- Web scraping using Scrapy — 124
- Setting up a project — 125
- Creating an item — 128
- Implementing the spider — 128
- Exporting data — 129
- Deploying a web crawler — 130
- Summary — 134
- Further reading — 134

# Table of Contents

# Part 3: Advanced Scraping Concepts

# 6

- Working with the Secure Web — 137
- Technical requirements — 137
- Exploring secure web content — 138
  - Form processing — 138
  - Cookies and sessions — 139
  - User authentication — 141
- HTML `<form>` processing using Python — 142
- User authentication and cookies — 147
- Using proxies — 151
- Summary — 155
- Further reading — 155

# 7

- Data Extraction Using Web APIs — 157
- Technical requirements — 157
- Introduction to web APIs — 158
  - Types of API — 159
  - Benefits of web APIs — 160
- Data formats and patterns in APIs — 162
  - Example 1 – sunrise and sunset — 163
  - Example 2 – GitHub emojis — 164
- Example 3 – Open Library — 165
- Web scraping using APIs — 166
  - Example 1 – holidays from the US calendar — 166
  - Example 2 – Open Library book details — 168
  - Example 3 – US cities and time zones — 169
- Summary — 171
- Further reading — 171

# 8

- Using Selenium to Scrape the Web — 173
- Technical requirements — 173
- Introduction to Selenium — 174
- Advantages and disadvantages of Selenium — 175
- Use cases of Selenium — 175

# 9

# Using Regular Expressions and PDFs 195

Technical requirements 196
Overview of regex 196
Regex with Python 197
re (search, match, and findall) 198
re.split 201
re.sub 202
re.compile 202
Regex flags 204
Using regex to extract data 206

Example 1 – Yamaha dealer information 206
Example 2 – data from sitemap 208
Example 3 – Godfrey’s dealer 210
Data extraction from a PDF 211
The PyPDF2 library 212
Extraction using PyPDF2 213
Summary 217
Further reading 218

# Part 4: Advanced Data-Related Concepts

# 10

# Data Mining, Analysis, and Visualization 221

Technical requirements 222
Introduction to data mining 222
Predictive data mining 224
Descriptive data mining 224
Handling collected data 225
Basic file handling 225
JSON 227
CSV 229

SQLite 233
Data analysis and visualization 234
Exploratory Data Analysis using ydata_profiling 235
pandas and plotly 237
Summary 246
Further reading 247
