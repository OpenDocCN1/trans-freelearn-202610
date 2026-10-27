

## Salesforce Lightning 报表与仪表板

Copyright © 2017 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: August 2017

Production reference: 1020817

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78829-738-7

www.packtpub.com

## Credits

**Author**
Johan Yu

**Copy Editor**
Stuti Srivastava

**Reviewers**
Chamil Madusanka
Tom Blamire

**Project Coordinator**
Prajakta Naik

**Commissioning Editor**
Merint Mathew

**Proofreader**
Safis Editing

**Acquisition Editor**
Karan Sadawana

**Indexer**
Francy Puthiry

**Content Development Editor**
Siddhi Chavan

**Production Coordinator**
Nilesh Mohite

**Technical Editor**
Tiksha Sarang

## Foreword

Before you dive into Johan's amazing book about Salesforce technology, I want to invite you to be a part of something special. We are in the midst of a major shift, and we're leaning into it with the full power of Salesforce. Our mission is to empower people to blaze their own trails, as well as to motivate and educate everyone in our community with how we live our values of trust, growth, innovation, and equality.

If you're reading this book, you are a Trailblazer and have more power than ever to make positive changes happen in your companies, communities, and careers. Doing well and doing good is about the power of working together to help companies be better for their customers, and businesses and to be major drivers of change. We want every individual to realize their full potential in their careers and lives. With this shift, we can generate hundreds of billions of dollars in GDP, create millions of new jobs, and help new Trailblazers learn the skills they need to succeed.

I'd like to introduce you to a few of the concepts that are central to this shift and core to our community: Ohana, Trailblazer, and Equality.

**Ohana:** This is defined broadly as "family" in Hawaiian, but at Salesforce, it means to connect, collaborate, learn, and have fun together. As one Salesforce Ohana, we all need to be Trailblazers, moving forward together on the same trail with the intentional purpose of delivering customer success and a path into the Salesforce economy. This, in turn, will generate hundreds of billions of dollars in GDP, create millions of new jobs, and help new Trailblazers learn the skills they need to succeed.

**Trailblazer:** This is a pioneer; an innovator; a lifelong learner; a mover and shaker; a leader who leaves a path for others to follow; and, most importantly, someone who inspires and drives change, and is committed to making the world a better place. This is all about being your best through a constant commitment to learning and giving back--and that's what this community is all about.

**Equality:** At Salesforce, works together with our whole Ohana--our employees, customers, partners, community organizations, and the whole tech industry--to build a path forward to equality for all. We are taking action across four key pillars: equal pay, equal opportunity, equal education, equal rights.

Let me leave you with this--ask yourself what will you learn this year? What will you teach? Will you be a mentor or a mentee? What’s the Salesforce path you want to follow? I encourage you to BE BOLD in your life, career, and community--be a part of this amazing shift!

**Erica Kuhl**
Vice President, Community - Salesforce

## About the Author

**Johan Yu** has more than 19 years of experience working in the IT sector across MNCs and at a leading Salesforce consulting company in the Asia-Pacific region. He has spent more than 12 years working with Salesforce technology, starting his career as a developer, team leader, and technical manager, among many other challenging roles.

Based in Singapore, Johan holds 12X active Salesforce certifications, ranging from Administrator to Architect/Designer certifications. In his spare time, he enjoys writing blogs and answering questions in the Salesforce Success Community.

In May 2014, Johan became the first Salesforce MVP, not only from Singapore, but also from Southeast Asia. He is also the leader of the Salesforce Singapore User Group and is keen to help members solve issues related to configuration, implementation, and adoption, until more technical issues arrive.

> > First of all, I am grateful to Almighty God for enabling me to complete this book. After living and breathing with Salesforce technologies for more than a decade, it’s a miracle and my happiest moment to write my second book on Salesforce. I would like to thank the creator of Salesforce for the invention of such a great technology. Thanks to the Salesforce MVP family and the entire Salesforce team for building a great Salesforce community around the world #ohana. I wish to express my sincere gratitude to my reviewers, Chamil Madusanka and Tom Blamire, for completing the review and feedback of this book. I would also like to thank the whole Salesforce Singapore User Group family; my co-leaders; my buddies Vladimir Vujatovic, Chrisila Low, and Raphael Fraga for the dedication to the community; and many Salesforce User Group leaders around the world--I can’t name each of you. Another big thanks to the team at Packt for their guidance and support which made this book happen. I would also like to thank my employer, church, family, and friends. Thanks to Ps Jacub Suria and Ps Yenny Sari, Minister of the Indonesian Family Church, Singapore, for the down-to-earth kindness and passion in giving all of us sermons and tasty food! Lastly, huge thanks and love to my wife, Novida Lunardi, for being a part of my life--her understanding, continued support, and encouragement helped me get this book completed.

**Tom Blamire** is a dedicated Salesforce administrator and evangelist who accidentally fell into Salesforce in 2014. He has come from an office administration background, working in various industries, and tries to apply that knowledge and skillset to every organization he has worked in.

I would like to thank Johan for initially asking me to be a part of this amazing adventure; my GIF Squad colleagues, Amy, Ben, Stuart, Pat, Bill, Justice, Stacey, Monica, Melinda, and Sam for their continued support, loyalty, inspiration, and dedication to the cause; Marc Benioff and Parker Harris for making the greatest platform on earth and helping change the lives of so many people for the better; the Salesforce Success Community, Trailhead, and Awesome Admin Jobs Team for doing what you do on a day-to-day basis and constantly supporting the #salesforceohana; Tony Prophet and team for helping to make the world a more educated and less critical place to be; and, most importantly, thank you to my wife and unborn son for everything, as without you, life would be a dark and empty place to be.

## www.PacktPub.com

For support files and downloads related to your book, please visit www.PacktPub.com.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/72d01bea5a9ccf02f9452aab7505d9de_9_0.png)

https://www.packtpub.com/mapt

Get the most in-demand software skills with Mapt. Mapt gives you full access to all Packt books and video courses, as well as industry-leading tools to help you plan your personal development and advance your career.

# Why subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print, and bookmark content
- On demand and accessible via a web browser

