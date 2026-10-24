
Anita Graser, Ben Mearns, Alex Mandel, Víctor Olaya Ferrero, Alexander Bruy

## QGIS: Becoming a GIS Power User

## Learning Path

Master data management, visualization, and spatial analysis techniques in QGIS and become a GIS power user

![](img/2e88bf5938bf48a409c0a534a8775b21_0_0.png)

## QGIS: Becoming a GIS Power User

Master data management, visualization, and spatial analysis techniques in QGIS and become a GIS power user

A course in three modules

![](img/2e88bf5938bf48a409c0a534a8775b21_1_0.png)

BIRMINGHAM - MUMBAI

## QGIS: Becoming a GIS Power User

Copyright © 2017 Packt Publishing

All rights reserved. No part of this course may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this course to ensure the accuracy of the information presented. However, the information contained in this course is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this course.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this course by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Published on: February 2017

Production reference: 1170217

Published by Packt Publishing Ltd., Livery Place, 35 Livery Street, Birmingham B3 2PB, UK.

ISBN 978-1-78829-972-5

www.packtpub.com

## Credits

**Authors**

Anita Graser, Ben Mearns, Alex Mandel, Víctor Olaya Ferrero, Alexander Bruy

**Content Development Editor**

Aishwarya Pandere

**Production Coordinator**

Aparna Bhagat

**Reviewers**

Cornelius Roth, Ujaval Gandhi, Fred Gibbs, Gergely Padányi-Gulyás, Abdelghaffar KHORCHANI, Pablo Pardo, Mats Töpel, Jorge Arévalo, Olivier Dalang, Ben Mearns

## Preface

QGIS is a crossing point of the free and open source geospatial world. While there are a great many tools in QGIS, it is not one massive application that does everything, and it was never really designed to be that from the beginning. It is rather a visual interface to much of the open source geospatial world. You can load data from proprietary and open formats into spatial databases of various flavors and then analyze the data with well-known analytical backends before creating a printed or web-based map to display and interact with your results. What's QGIS's role in all this? It's the place where you check your data along the way, build and queue the analysis, visualize the results, and develop cartographic end products.

This learning path will teach you all that and more, in a hands-on learn-by-doing manner. Become an expert in QGIS with this useful companion.

### What this learning path covers

*Module 1, Learning QGIS, Third edition*, covers important features that enable us to create great maps. Then, we will cover labeling using examples of labeling point locations as well as creating more detailed road labels with road shield graphics. We will also cover how to tweak labels manually. We will get to know the print composer and how to use it to create printable maps and map books. Finally, we will cover solutions to present your maps on the Web.

*Module 2, QGIS Blueprints*, will demonstrate visualization and analytical techniques to explore relationships between place and time and between places themselves. You will work with demographic data from a census for election purposes through a timeline-controlled animation.

*Module 3, QGIS 2 Cookbook*, deals with converting data into the formats you need for analysis, including vector to and from raster, transitioning through different types of vectors, and cutting your data to just the important areas. It also shows you how to take QGIS beyond the out-of-the-box features with plugins, customization, and add-on tools.

### What you need for this learning path

**Module 1**

To follow the exercises in this book, you need QGIS 2.14. QGIS installation is covered in the first chapter and download links for the exercise data are provided in the respective chapters.

**Module 2**

You will need:

- QGIS 2.10
- A computer running OS X, Windows, or Linux

**Module 3**

We recommend installing QGIS 2.8 or later; you will need at least QGIS 2.4. During the writing of this book, several new versions were released, approximately every 4 months, and most recently, 2.14 was released. Most of the recipes will work on older versions, but some may require 2.6 or newer. In general, if you can, upgrade to the latest stable release or Long Term Support (LTS) version.

There are also a lot of side interactions with other software throughout many of these recipes, including—but not limited to—PostGIS 2+, GRASS 6.4+, SAGA 2.0.8+, and SpatiaLite 4+. On Windows, most of these can be installed using OSGeo4W; on Mac, you may need some additional frameworks from KyngChaos, or if you're familiar with Brew, you can use the OSGeo4Mac Tap. For Linux users, in particular Ubuntu and Debian, refer to the UbuntuGIS PPA and the DebianGIS blend.

Does all of this sound a little too complicated? If yes, then consider using a virtual machine that runs OSGeo-Live (http://live.osgeo.org). All the software is preinstalled for you and is known to work together.

Lastly, you will need data. For the most part, we've provided a lot of free and open data from a variety of sources, including the OSGeo Educational dataset (North Carolina), Natural Earth Data, OpenFlights, Wake County, City of Davis, and Armed Conflict Location & Event Data Project (ACLED). A full list of our data sources is provided here if you would like additional data.

We recommend that you try methods with the sample data first, only because we tested it.

Feel free to try using your own data to test many of the recipes; however, just remember that you might need to alter the structure to make it work. After all, that's what you'll be working with normally.

The following are the data sources for this book:

- OSGeo Educational Data: http://grass.osgeo.org/download/sample-data/
- Wake County, USA: http://www.wakegov.com/gis/services/pages/data.aspx
- Natural Earth Data: http://www.naturalearthdata.com/
- City of Davis, USA: http://maps.cityofdavis.org/library
- Stamen Designs: http://stamen.com/
- Armed Conflict Location & Event Data Project: http://www.acleddata.com/

### Who this learning path is for

If you are a user, developer, or consultant and want to know how to use QGIS to achieve the results you are used to from other types of GIS, then this learning path is for you. You are expected to be comfortable with core GIS concepts. This Learning Path will make you an expert with QGIS by showing you how to develop more complex, layered map applications. It will launch you to the next level of GIS users.

### Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this course – what you liked or disliked. Reader feedback is important for us as it helps us develop titles that you will really get the most out of.

To send us general feedback, simply e-mail feedback@packtpub.com, and mention the course's title in the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a course, see our author guide at www.packtpub.com/authors.

### Customer support

Now that you are the proud owner of a Packt course, we have a number of things to help you to get the most from your purchase.

**Downloading the example code**

You can download the example code files for this course from your account at http://www.packtpub.com. If you purchased this course elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register to our website using your e-mail address and password.
- 2. Hover the mouse pointer on the **SUPPORT** tab at the top.
- 3. Click on **Code Downloads & Errata**.
- 4. Enter the name of the course in the **Search** box.
- 5. Select the course for which you're looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this course from.
- 7. Click on **Code Download**.

You can also download the code files by clicking on the **Code Files** button on the course's webpage at the Packt Publishing website. This page can be accessed by entering the course's name in the **Search** box. Please note that you need to be logged in to your Packt account.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the course is also hosted on GitHub at https://github.com/PacktPublishing/QGIS-Becoming-a-GIS-Power-User.

We also have other code bundles from our rich catalog of books, videos and courses available at https://github.com/PacktPublishing/. Check them out!
