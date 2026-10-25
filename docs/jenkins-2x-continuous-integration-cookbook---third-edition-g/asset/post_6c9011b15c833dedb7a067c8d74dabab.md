
## Jenkins 2.x Continuous Integration Cookbook
### Third Edition

Over 90 recipes to produce great results using pro-level practices, techniques, and solutions

Mitesh Soni, Alan Mark Berg

![](img/2a4d2e09d9af2c678ea8f43ea344059d_0_0.png)
![](img/2a4d2e09d9af2c678ea8f43ea344059d_0_1.png)
![](img/2a4d2e09d9af2c678ea8f43ea344059d_1_0.png)

BIRMINGHAM - MUMBAI

Copyright © 2017 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: June 2012

Second edition: January 2015

Third edition: October 2017

Production reference: 1261017

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78829-794-3

www.packtpub.com

## Credits

### Authors

Mitesh Soni

Alan Mark Berg

### Reviewers

Juan Vicente Herrera Ruiz de Alejo

Javier Delgado

Tim Ysewyn

### Commissioning Editor

Vijin Boricha

### Acquisition Editor

Chandan Kumar

### Content Development Editor

Deepti Thore

### Technical Editor

Sneha Hanchate

### Copy Editors

Laxmi Subramanian

Safis Editing

### Project Coordinator

Shweta H Birwatkar

### Proofreader

Safis Editing

### Indexer

Tejal Daruwale Soni

### Graphics

Tania Dutta

### Production Coordinator

Deepika Naik

## About the Authors

**Mitesh Soni** is an avid learner with 10 years of experience in the IT industry. He is an SCJP, SCWCD, VCP, IBM UrbanCode, and IBM Bluemix certified professional and Certified Scrum Master. He loves DevOps and cloud computing and he also has an interest in programming in Java. He finds design patterns fascinating. He believes a picture is worth a thousand words.

He occasionally contributes to etutorialsworld.com. He loves to play with kids, fiddle with his camera, and take photographs at Indroda Park. He is addicted to taking pictures without knowing many technical details. He lives in the capital of Mahatma Gandhi's home state.

Mitesh has also authored the following books with Packt:

- Jenkins Essentials, Second Edition
- DevOps Bootcamp
- Implementing DevOps with Microsoft Azure
- DevOps for Web Development
- Jenkins Essentials
- Learning Chef

> "I've missed more than 9,000 shots in my career. I've lost almost 300 games. 26 times, I've been trusted to take the game-winning shot and missed. I've failed over and over and over again in my life. And that is why I succeed."
>
> — Michael Jordan.

**Alan Mark Berg**, BSc, MSc, PGCE, has been the lead developer at Central Computer Services at the University of Amsterdam since 1998. He is currently working in an Innovation Work Group that accelerates the creation of new and exciting services. In his famously scarce spare time, he writes. Alan has a bachelor's degree, two master's degrees, a teaching qualification, and quality assurance certifications. He has also coauthored two Packt Publishing books about Sakai (http://sakaiproject.org), a highly successful open source learning management platform used by millions of students around the world. He has won a couple of awards, including the Sakai Fellowship and Teaching With Sakai Innovation Award (TWSIA).

Alan enjoys working with talent; this forces him to improve his own competencies. This motivation is why Alan enjoys working in energetic, open source communities of interest. At the time of writing, he is on the board of directors of the Apereo Foundation and is the community officer for its Learning Analytics Initiative (https://confluence.sakaiproject.org/display/LAI/Learning+Analytics+Initiative).

In previous incarnations, Alan was a QA director, a technical writer, an Internet/Linux course writer, a product line development officer, and a teacher. He likes to get his hands dirty with building, gluing systems, exploring data, and turning it into actionable information. He remains agile by ruining various development and acceptance environments and generally rampaging through the green fields of technological opportunity.

## About the Reviewers

**Juan Vicente Herrera Ruiz de Alejo** has a degree in computer science. He is the leader of the Madrid DevOps meetup. He is the teacher of DevOps master's program at Alcala de Henares University in Madrid and has been working in IT companies for the past 12 years.

He also works as a volunteer at an NGO dedicated to fighting breast cancer.

He is currently working for Logtrust.com and has worked for companies such as Colt telecom, Everis, and BBVA.

_I would like to thank my wife, Emma, and my entire family for all their support._

**Javier Delgado** is an automation fanatic, continuous tasks (inspection, testing, and delivery) evangelist, and perpetual new-knowledge addict.

He works as a continuous delivery expert at ING Bank NV.

## www.PacktPub.com

For support files and downloads related to your book, please visit www.PacktPub.com.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/2a4d2e09d9af2c678ea8f43ea344059d_6_0.png)

https://www.packtpub.com/mapt

Get the most in-demand software skills with Mapt. Mapt gives you full access to all Packt books and video courses, as well as industry-leading tools to help you plan your personal development and advance your career.

## Why subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print, and bookmark content
- On demand and accessible via a web browser

## Customer Feedback

Thanks for purchasing this Packt book. At Packt, quality is at the heart of our editorial process. To help us improve, please leave us an honest review on this book's Amazon page at https://www.amazon.com/dp/1788297946.

If you'd like to join our team of regular reviewers, you can e-mail us at customerreviews@packtpub.com. We award our regular reviewers with free eBooks and videos in exchange for their valuable feedback. Help us be relentless in improving our products!

I would like to dedicate this book to all those empty bus stands, benches, cycles, cameras, roads, and trees who have witnessed my anger, emotions, love, and hostility. Without them, people would have come across real me (Pun Intended).

I apologize in advance if I have missed any name. I have tried to recollect every name with whom I have shared good memories. I don't have a good ‘memory’ and that is a blessing as well as a curse at times.

![](img/2a4d2e09d9af2c678ea8f43ea344059d_8_0.png)

On a serious note, I would like to dedicate this book to Shreyansh (Shreyu-My sister (Jigisha)'s baby boy) showed me the power of innocence and smile, Vinay Kher for teaching me how to improve always and keeping my text messages in his mobile, my parents who are always there silently and pray for me, Simba (Priyanka Agashe) for supporting and encouraging me all the time and forced me to believe in myself, Indian Army, and all brave soldiers in uniform for protecting us.

Ashita A, Sourabh Mishra and Gowri, thanks for showing me mirror whenever it was required. I value your presence in my life. It was fun to learn about life from you. I might have forgotten everything you taught me the next day. Sorry for being childish but I can’t change it.

I am grateful to Rinka, Bhagyashri, Avanti, Aishwarya P, Vijay, Priya, Harshal, Tejas, Sharvil P, Apoorva S, Nandan, Bhavna, Vishakha S, Pradnya B, Viral I, Chaitali, Sudeep, Amit R, Manisha Y, Vaishnavi, Parinda, Arpita, Jinesh, Vihan, Saurabh S, and Raghav H, who have always helped me, made me smile, and made my life easier in last few years or so.

I want to say thanks... and share my gratitude for everything I’ve been blessed with. I would like to thank Jigisha-Nitesh, Dada-Dadi, Priyanka-Hemant, Anupama-Mihir, Nalini and family, Kim and Yaashi, Kirti, Bindiya, Jai Jamba, Nitesh, Munal, Ashish B, Mayur Mothliya, Rohini, Aakanksha, Yohan Wadia, Rohan C, Chintan Solanki, Vijay Patel, Nikul, Paresh, Raju, Yogendra, Ajay, Ruchi, Navrang O, Dharmesh R, my village friends, Prakash, Mitul, Kanak, Bapu, Ravi, Vimal, Chitrang, Krimali, Saputara Group, Subhash, Ranjit, Rangusinh, Setan, Jayesh and his family, Ramesh and his family, Vijay Prajapati, Munni Bhabhi and her family and teachers.

Divya, Deepti, and Sneha... this wouldn’t have been possible without you guys. Thank you for inspiring me and keeping me on my toes.
