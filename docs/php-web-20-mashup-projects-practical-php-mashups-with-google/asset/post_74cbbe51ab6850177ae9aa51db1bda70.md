
![](img/c637937d44400f1da21fb7cca101de80_0_0.png)

## PHP Web 2.0
Mashup Projects

Create practical mashups in PHP, grabbing and mixing data from Google Maps, Flickr, Amazon, YouTube, MSN Search, Yahoo!, Last.fm, and 411Sync.com

## PHP Web 2.0 Mashup Projects

Create practical mashups in PHP, grabbing and mixing data from Google Maps, Flickr, Amazon, YouTube, MSN Search, Yahoo!, Last.fm, and 411Sync.com

Shu-Wai Chow

[PACKT]
PUBLISHING
BIRMINGHAM - MUMBAI

## PHP Web 2.0 Mashup Projects

**Create practical mashups in PHP, grabbing and mixing data from Google Maps, Flickr, Amazon, YouTube, MSN Search, Yahoo!, Last.fm, and 411Sync.com**

Copyright © 2007 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, Packt Publishing, nor its dealers or distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: September, 2007

Production Reference: 1070907

Published by Packt Publishing Ltd.
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-847190-88-8

www.packtpub.com

Cover Image by Vinayak Chittar (vinayak.chittar@gmail.com)

## Credits

**Author**
Shu-Wai Chow

**Project Manager**
Patricia Weir

**Reviewer**
Stoyan Stefanov

**Project Coordinator**
Abhijeet Deobhakta

**Senior Acquisition Editor**
Douglas Paterson

**Indexer**
Bhushan Pangaonkar

**Development Editor**
Nikhil Bangera

**Proofreader**
Cathy Cumberlidge

**Technical Editors**
Adil Rizwan
Ajay. S

**Production Coordinator**
Shantanu Zagade

**Editorial Manager**
Dipali Chittar

**Cover Designer**
Shantanu Zagade

## About the Author

**Shu-Wai Chow** has worked in computer programming and information technology for the past eight years. He started his career in Sacramento, California, spending four years as the webmaster for Educaid, a First Union Company, and another four years at Vision Service Plan as an application developer. Through the years, he has become proficient in Java, JSP, PHP, ColdFusion, ASP, LDAP, XSLT, and XSL-FO. Shu has also been the volunteer webmaster and a feline adoption counselor for several animal welfare organizations in Sacramento.

He is currently a software engineer at Antenna Software in Jersey City, New Jersey, and is finishing his studies in Economics at Rutgers, the State University of New Jersey.

Born in the British Crown Colony of Hong Kong, Shu did most of his alleged growing up in Palo Alto, California. He lives on the Jersey Shore with seven very demanding cats, four birds that are too smart for their own good, a tail-less bearded dragon, a betta who needs her tank cleaned, a dermestid beetle colony, a cherished Fender Stratocaster, and a beloved, saint-like fiancé.

I received a lot of help from many different people and companies on this book.

First and foremost, thank you to the people at Packt Publishing, especially Doug Paterson, Nikhil Bangera, Adil Rizwan, and Abhijeet Deobhakta, for a professional working experience, giving me this opportunity, and once again, for their faith in me (which I still don’t completely understand).

Thank you, Stoyan Stefanov, for your great review comments. Your insight and suggestions really improved this book’s content and personally pushed me further.

Each chapter deserves some special attention.

Chapter 2: Thank you to the folks at the UPC Internet Database and Amazon, Inc. for their permission. Thanks especially to everyone at UPC Internet Database for answering my questions.

Chapter 3: Thank you to Yahoo! and Microsoft for their permission, and the prompt service and **assistance from their legal departments**. Special thanks are given to Data Access Corporation and Vincent Oorsprong for their helpful and educational SOAP services. Data Access Worldwide (`www.dataaccess.com`) delivers advanced products and services to help customers build great applications and get more value from their data.

Chapter 4: Thanks to the people at YouTube and Last.fm for their permission. Special thanks go to Last.fm for their interest in my project.

Chapter 5: Thank you to 411Sync and the California Highway Patrol for their assistance. A big thank you goes to **Manish Lachwani of** 411Sync, who is probably the most patient person in the world.

Chapter 6: Thank you to Google, Flickr, and Jo Walsh for permission to be included. A big thanks to Jo Walsh for her help and insight. Special thanks to the people at Google, whose enthusiasm made me think that this book wasn’t such a nutty idea.

I neglected to thank Ed Hansen and Keith Easterbrook in the last book. They were the ones that forced us to use *WebSphere Application Developer*. Without WSAD, I never would have used Eclipse, and I never would have used PHPEclipse, which means that I never would have written the first book, and without the first book, there would not be this second book. So, thank you, gentlemen, and I apologize for the oversight.

Thank you to Kaya’s Kitchen of Belmar, New Jersey for making the best vegetarian cuisine in the whole wide world. If you ever find yourself on the Jersey Shore, you absolutely must visit this restaurant.

Finally, all hail the Palo Alto Duck Pond, Hobee’s on El Camino and Arastradero, Dodger Stadium, and the Davis Greenbelt.

## About the Reviewer

**Stoyan Stefanov** is a Yahoo! web developer, Zend Certified Engineer, book author and contributor to the international PHP community. His personal blog is at http://www.phpied.com.

Once upon a time, a diviner told me that I would meet an angel on Earth in the month of February. This book is dedicated to that February angel and love of my life, Anneliese Strunk.
You have brought more happiness, inspiration, and joy to my life than I could ever have imagined.

## Table of Contents

- Preface 1
- Chapter 1: Introduction to Mashups 7
- Web 2.0 and Mashups 9
- Importance of Data 9
- User Communities 10
- How We Will Create Mashups 11
- More Mashups 12
- Chapter 2: Buy it on Amazon 13
- Project Overview 13
- XML-RPC 14
- XML-RPC Structure 14
- XML-RPC Request 15
- Arrays 18
- Struct 19
- XML-RPC Response 20
- Working with XML-RPC in PHP 21
- Making an XML-RPC Request 22
- Serializing Data with XML-RPC Encode Request 22
- Calling XML-RPC Using Sockets 29
- Processing an XML-RPC Response 31
- Creating an XML-RPC Parser Class 32
- Testing Our XML-RPC Parser Class 33
- Using PEAR to Handle XML-RPC 35
- REST 38
- Working with REST in PHP 39
- Making a REST Request 40
- A GET and POST Refresher 40
- Using Sockets to Initiate a REST Request 41
- Creating GET and POST Request Functions 42
- Making a REST Parser Class 43
- Testing Our REST Parser Class 45
- Processing a REST Response 47
- Basic Walkthrough with PHP and SAX 48
- Creating a SAX Parser Class 54
- Internet UPC Database API 58
- Amazon API 61
- A Tour of ECS 62
- Anatomy of an ECS REST Request 63
- Location of Service 63
- Mashing Up 65
- Product Lookups 66
- Handling Amazon's XML Responses 70
- Summary 79
- Chapter 3: Make Your Own Search Engine 81
- Project Overview 81
- SOAP 82
- Web Services Descriptor Language (WSDL) With XML Schema Data (XSD) 83
- Basic WSDL Structure 84
- definitions Element 84
- types Element 85
- message Element 91
- portType Element 93
- binding Element 95
- service Element 96
- The SOAP Message 97
- Envelope 97
- Header 98
- Body 98
- Fault 100
- PHP's SoapClient 101
- Creating Parameters 102
- Instantiate the SoapClient 103
- Instantiating in WSDL Mode 104
- Instantiating in Non-WSDL Mode 104
- Making the Call and Using SoapClient Methods 105
- Handling the SOAP Response 108
- Microsoft Live Search Web Service 112
- Using Search 112
- Yahoo! Search Web Service 116
- Using Web Search 116
- Mashing Up 119
- Summary 123
