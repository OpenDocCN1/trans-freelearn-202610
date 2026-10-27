

![](img/371b9a67300a03b7b335eb8a6fe55d94_0_0.png)

Community Experience Distilled

# Bootstrap for ASP.NET MVC

将 Bootstrap 整合到你的 ASP.NET MVC 项目中，让你的网站更友好、更具动态性

Pieter van der Westhuizen

![](img/371b9a67300a03b7b335eb8a6fe55d94_0_1.png)

# Bootstrap for ASP.NET MVC

将 Bootstrap 整合到你的 ASP.NET MVC 项目中，让你的网站更友好、更具动态性

**Pieter van der Westhuizen**

![](img/371b9a67300a03b7b335eb8a6fe55d94_1_0.png)

BIRMINGHAM - MUMBAI

# Bootstrap for ASP.NET MVC

Copyright © 2014 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: August 2014

Production reference: 1130814

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-78398-728-3

www.packtpub.com

Cover image by Bartosz Chucherko (chucherko@gmx.com)

## Credits

**Author**
Pieter van der Westhuizen

**Project Coordinator**
Kartik Vedam

**Reviewers**
Jaco Fouché
Tinus Smit
Stephan Swart

**Proofreaders**
Bridget Braund
Paul Hindle

**Commissioning Editor**
Amarabha Banerjee

**Indexer**
Hemangini Bari

**Acquisition Editor**
Rebecca Youé

**Production Coordinators**
Conidon Miranda
Nilesh R. Mohite
Nitesh Thakur

**Content Development Editor**
Priyanka S

**Cover Work**
Nitesh Thakur

**Technical Editor**
Mrunal Chavan

**Copy Editors**
Janbal Dharmaraj
Deepa Nambiar
Karuna Narayanan
Alfida Paiva

## About the Author

**Pieter van der Westhuizen** is a freelance software and web developer specializing in ASP.NET MVC, web technologies, and MS Office development. He started his career in web development using classic ASP, Visual InterDev, HoTMetaL, and FrontPage.

Pieter has over 15 years of experience in the IT industry and is also one of the people fortunate enough to have his hobby become his full-time profession. He is also a technology evangelist for Add-in Express (www.add-in-express.com), which focuses on tools for Microsoft Office integration.

This is Pieter's first foray into book writing although he has been blogging since 2007 on his personal blog at www.mythicalmammoth.com and on the Add-in Express blog since 2010. He lives with his wife and Harrier (the dog, not the bird or Jump Jet) in Pretoria, South Africa.

## Acknowledgments

To everyone who contributed to this book, thank you! A big thanks to the team at Packt Publishing, especially Rebecca, Priyanka, Kartik, and Aboli, for their guidance, advice, and professionalism.

Thanks to my three technical reviewers, Jaco, Tinus, and Stephan, for patiently reading each chapter, reviewing each line of code, and contributing suggestions. You helped shape this book into what it is.

I'd like to express my greatest love and gratitude towards my wife, Andrea, for all her support, impromptu editing sessions, being a soundboard for brainstorming sessions, and the general motivational pep talks during the writing of this book.

To my parents — even though they were not exactly sure what this book was about — thank you for all your support and for allowing me to tinker with the family computer from an early age.

I'd also like to extend a special thank you to Eugene Starostin from Add-in Express, who allowed me to blog about an idea that subsequently turned into this book.

## About the Reviewers

**Jaco Fouché** has been involved with software development for over 22 years and is currently a database architect and software developer at SilverBridge in Pretoria, South Africa. He holds a B.Sc degree in Computer Science from the University of Pretoria. He has worked with different technologies but specializes in Microsoft technologies with specific emphasis on ASP.NET MVC and Microsoft SQL Server.

With a number of years in the coding field, **Tinus Smit** has a passion for good, clean code and solving problems. A C# .NET developer at his core, he has also gained extensive knowledge of Microsoft SQL, Microsoft Dynamics CRM, JavaScript, Entity Framework, and MVC.

The CRM experience is not limited to clicking a few buttons and dragging fields around in a form. He has had a hand in solving complex business problems with CRM's workflow and plugin engines, often integrating with other systems in the process. Over the years, he has learned exactly where CRM fits best, what the product can do, where and how to extend it, and the best practices thereof.

However, in the end, these are merely tools to solve problems. In Johannesburg, South Africa, he finds himself working tirelessly on projects for many companies over the years (from small to enterprise-sized), using these tools and experience. He sees every new challenge as an opportunity to learn something new and finds ways to teach what he has learned. Armed with a database and Visual Studio, he uses pragmatism and logic to help users reach their goal.

He can be reached on Twitter at @CodingWithTinus or on his blog at http://codingwithtinus.wordpress.com.

**Stephan Swart** has been interested in software development for the last 30 years. He has used a variety of technologies, programming languages, and platforms. Starting as a Cobol developer, embracing most of the latest technologies that were released at the time, he has been involved in .NET development since the very first release of .NET. In the last few years, he has lived and worked as a senior .NET developer using Web, Windows, and mobile technologies for a number of different companies in Europe, Canada, and South Africa. Among the technologies he used recently are ASP.NET MVC, jQuery, Knockout, C#, VB .NET, WPF, Silverlight, MVVM, Xamarin, and many more.

# www.PacktPub.com

# Support files, eBooks, discount offers, and more

You might want to visit www.PacktPub.com for support files and downloads related to your book.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/371b9a67300a03b7b335eb8a6fe55d94_8_0.png)

```
http://PacktLib.PacktPub.com
```

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can access, read and search across Packt's entire library of books.

# Why subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print and bookmark content
- On demand and accessible via web browser

# Free access for Packt account holders

If you have an account with Packt at www.PacktPub.com, you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

Table of Contents
Chapter 2: Using Bootstrap CSS and HTML Elements 29
The Bootstrap grid system 30
Bootstrap grid options 30
Bootstrap HTML elements 31
Bootstrap tables 32
Styling Bootstrap tables 36
Bootstrap contextual table classes 37
Bootstrap buttons 39
Form layout and elements 41
Horizontal forms 41
Vertical/Basic forms 42
Inline forms 43
Bootstrap validation styles 44
Creating editor templates for primitive types 47
Creating editor templates for nonprimitive types 48
Bootstrap image classes 50
Summary 52
Chapter 3: Using Bootstrap Components 53
The Bootstrap navigation bar 53
List groups 56
Badges 57
The media object 57
Page headers 59
Breadcrumb 60
Pagination 60
Input groups 64
Button dropdowns 66
Alerts 67
Progress bars 69
The basic progress bar 69
Contextual progress bars 70
Striped and animated progress bars 71
Dynamically updating the progress bar's percentage 71
Summary 74
Chapter 4: Using Bootstrap JavaScript Plugins 75
Data attributes versus the programmatic API 76
Cascading dropdowns 77
Modal dialogs 80

