

# Learning Raphaël JS Vector Graphics

Over 70 code examples to create vector graphics and data visualizations!

**Damian Dawber**

**BIRMINGHAM - MUMBAI**

Copyright © 2013 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: May 2013

Production Reference: 1100513

Published by Packt Publishing Ltd. Livery Place 35 Livery Street Birmingham B3 2PB, UK. ISBN 978-1-78216-916-1 www.packtpub.com

Cover Image by Sujay Gawand (sujaygawand@gmail.com)

www.it-ebooks.info

## Credits

- **Author** Damian Dawber
- **Reviewers** Charles Thomas Lee Turver
- **Acquisition Editor** Mary Jasmine Nadar
- **Commissioning Editor** Harsha Bharwani
- **Technical Editor** Jalasha D'costa
- **Project Coordinator** Sneha Modi
- **Proofreader** Mario Cecere
- **Indexer** Rekha Nair
- **Production Coordinator** Shantanu Zagade
- **Cover Work** Shantanu Zagade

## About the Author

**Damian Dawber** is a web developer working on medium- to large-scale e-commerce websites and bespoke web and mobile applications. He works with both frontend and server-side technologies having had exposure to a wide range of projects working on behalf of small and medium enterprises through to FTSE 100 companies.

He started his career after being exposed to programming as it used to solve problems in physics and decided he wanted to write code on a daily basis thereafter.

> Naturally I would like to thank everyone who has in some way shaped my personal and career development up to now. In particular, I would like to thank my family, my colleagues, and my current and former employers for giving me the opportunity to develop in this industry.
>
> Special thanks must go to this book's technical reviewers, Charles Thomas and Lee Turver. Their input has been most valuable.
>
> I would also like to thank Tomás Alabés for providing me with content pertaining to the future of the Raphaël library.

## About the Reviewers

After leaving school and having spent significant periods in occupations as diverse as a trainee accountant and soldier, **Charles Thomas** supplied a software solution for a company he worked for as a laboratory supervisor in 1990. When he was made unemployed by his company in 1992, he completed further software training (HNC in Computer Studies in 1994).

Since 1994, he has always been engaged in projects that are on the "bleeding edge of technology" and he enjoys doing these very much. During his career he has worked globally for different companies in England, Europe, Canada, and USA as a software engineer (Some of these companies include the European Bank, GE (Amsterdam), Associated Newspapers / Daily Mail, and HSBC). He now lives and works in the UK and enjoys working as a graphical frontend developer on contract as required by interested companies.

He additionally did a smaller review for _Instant RaphaelJS Starter_ by Packt Publishing.

> I would like to thank Tomás Alabés (the main collaborator of the latest Raphaël release) for his contribution that provided information on the future of Raphaël, which appears in the last chapter of the book.

**Lee Turver** is a PHP developer based in Manchester with close to 10 years' experience working with various web-related technologies. Now working for one of the leading digital agencies in the North West, building high-quality solutions for high-profile clients.

You can follow Lee on Twitter @LeeTurver.

## www.PacktPub.com

# Support files, eBooks, discount offers and more

You might want to visit www.PacktPub.com for support files and downloads related to your book.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

http://PacktLib.PacktPub.com

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can access, read and search across Packt's entire library of books.

# Why Subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print and bookmark content
- On demand and accessible via web browser

# Free Access for Packt account holders

If you have an account with Packt at www.PacktPub.com, you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

## Table of Contents

| Preface | 1 |
| --- | --- |
| Chapter 1: The Graphical Web | 5 |
| Vector drawing on the Web | 6 |
| Vector drawing libraries | 7 |
| The SVG specification | 8 |
| Working with Raphaël rather than SVG directly | 9 |
| Applications of Raphaël | 9 |
| Downloading Raphaël | 10 |
| Creating Raphaël JavaScript applications | 11 |
| Project structure and optimization | 11 |
| Summary | 11 |
| Chapter 2: Basic Drawing with Raphaël | 13 |
| The drawing context | 13 |
| Canvas coordinates | 15 |
| Drawing basic shapes | 16 |
| Embedding images | 18 |
| Element attributes | 18 |
| Basic fills | 19 |
| Image fills | 19 |
| Applying strokes | 20 |
| Other attributes | 21 |
| href | 21 |
| opacity | 22 |
| clip-rect | 22 |
| Applying gradients | 23 |
| Linear gradients | 23 |
| Radial gradients | 25 |
| Grouping elements | 27 |
| Working with text | 28 |
| Embedding custom fonts | 29 |
| Summary | 30 |
| Chapter 3: Drawing Paths | 31 |
| Path drawing concepts | 32 |
| Path drawing commands | 33 |
| The moveto command | 34 |
| The lineto commands | 35 |
| The closepath command | 37 |
| Drawing curves | 38 |
| Quadratic Bézier curves | 38 |
| Cubic Bézier curves | 41 |
| Drawing arcs | 43 |
| Utility methods for working with paths | 46 |
| Element.getTotalLength() | 46 |
| Element.getPointAtLength(length) | 47 |
| Element.getSubpath(from, to) | 48 |
| Catmull-Rom curves | 49 |
| Summary | 50 |
| Chapter 4: Transformations and Event Handling | 51 |
| Basic transformations and event handling | 52 |
| Basic transformations | 52 |
| Translation | 53 |
| Rotation | 53 |
| Scaling | 55 |
| Basic event handling | 55 |
| Registering basic event handlers | 55 |
| Unregistering basic event handlers | 57 |
| Working with matrices | 58 |
| Transformation matrices | 58 |
| Using transformation matrices | 58 |
| The drag-and-drop functionality | 60 |
| The Element.drag() method | 60 |
| The onstart event handler | 60 |
| The onend event handler | 60 |
| The onmove event handler | 60 |
| Dragging by example | 61 |
| Dropping elements | 62 |
| Bounding box overlapping | 62 |
| Bounding box inside bounding box | 63 |
| Summary | 64 |

