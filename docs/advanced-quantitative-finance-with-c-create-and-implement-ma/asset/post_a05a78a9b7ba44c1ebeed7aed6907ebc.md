
## Community Experience Distilled
## Advanced Quantitative Finance with C++
Create and implement mathematical models in C++ using Quantitative Finance
Alonso Peña, Ph.D.
[PACKT] PUBLISHING open source*

## Advanced Quantitative Finance with C++
Create and implement mathematical models in C++ using Quantitative Finance
Alonso Peña, Ph.D.
![](img/29d9d2cdc85db7e20e91367a93390be2_1_0.png)

## Advanced Quantitative Finance with C++
Copyright © 2014 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: June 2014

Production reference: 1180614

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-78216-722-8

www.packtpub.com

Cover image by VTR Ravi Kumar (vtrravikumar@gmail.com)

## Credits

**Author**
Alonso Peña, Ph.D.

**Project Coordinator**
Harshal Ved

**Reviewers**
Marco Airoldi
Joseph Smidt

**Proofreader**
Clyde Jenkins

**Commissioning Editor**
Grant Mizen

**Graphics**
Sheetal Aute
Ronak Dhruv
Valentina Dsilva
Disha Haria
Abhinash Sahu

**Acquisition Editor**
Harsha Bharwani

**Indexer**
Hemangini Bari

**Content Development Editor**
Amit Ghodake

**Technical Editor**
Humera Shaikh

**Production Coordinator**
Kyle Albuquerque

**Copy Editor**
Laxmi Subramanian

**Cover Work**
Nilesh Bambardekar

## About the Author

Alonso Peña, Ph.D. is an SDA Professor at the SDA Bocconi School of Management in Milan. He has worked as a quantitative analyst in the structured products group for Thomson Reuters Risk and for Unicredit Group in London and Milan. He holds a Ph.D. degree from the University of Cambridge on Finite Element Analysis and the Certificate in Quantitative Finance (CQF) from 7city Learning, the U.K. He has lectured and supervised graduate and post-graduate students from the universities of Oxford, Cambridge, Bocconi, Bergamo, Pavia, Castellanza, and the Politecnico di Milano. His area of expertise is the pricing of financial derivatives, in particular, structured products.

He has publications in the fields of Quantitative Finance, applied mathematics, neuroscience, and the history of science. He has been awarded the Robert J. Melosh Medal – first prize for the best student paper on Finite Element Analysis, Duke University, USA; and the Rouse Ball Travelling Studentship in Mathematics, Trinity College, Cambridge. He has been to the Santa Fe Institute, USA, to study complex systems in social sciences.

His publications include the following:

- The One Factor Libor Market Model Using Monte Carlo Simulation: An Empirical Investigation
- On the Role of Behavioral Finance in the Pricing of Financial Derivatives: The Case of the S&P 500
- Option Pricing with Radial Basis Functions: A Tutorial
- Application of extrapolation processes to the finite element method
- On the Role of Mathematical Biology in Contemporary Historiography

He is currently working as a tutor for CQF (Fitch Learning) and a visiting faculty for the Indian Institute for Quantitative Finance, Mumbai.

He lives in Italy with his wife Marcella, his daughters Francesca and Isabel, and his son Marco.

## Acknowledgments

I would like to thank many people who have made this book a reality. First the magnificent support, enthusiasm, and patience of the entire team at Packt Publishing, particularly Harsha, Amit, Humera, and Harshal. To Dr. Pattabi Raman (Numerical Solution (U.K.) Ltd.), for his expert advice on C++. To Dr. Marco Airoldi for his knowledgeable and detailed review of the book. To the SDA Bocconi School of Management including my colleagues and students from the MBA, graduate, and undergraduate courses. To the many persons I have been privileged to work with and to teach from the Universities of Cambridge, Oxford, Bocconi, LIUC Castellanza, Bergamo, Pavia, and Politecnico di Milano. The many extraordinary quants from the Certificate in Quantitative Finance, Fitch Learning, London, as well as from Unicredit Group and Thomson Reuters. Finally, to my wife, Marcella, and my children, Francesca, Isabel, and Marco – you all always remind me that "The true voyage of discovery consists not in seeking new landscapes but in having new eyes to see" (Marcel Proust).

## About the Reviewer

**Marco Airoldi** received his Ph.D. in Theoretical Condensed Matter Physics in 1995 from the International School for Advanced Studies (SISSA). He moved definitively to finance in 1999. Marco has been chosen as the head of financial engineering in one of the top financial institutions in Italy.

His expertise includes the Monte Carlo simulation for option pricing and pricing system architectures.

## Support files, eBooks, discount offers, and more

You might want to visit www.PacktPub.com for support files and downloads related to your book.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at www.PacktPub.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at service@packtpub.com for more details.

At www.PacktPub.com, you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

![](img/29d9d2cdc85db7e20e91367a93390be2_7_0.png)

http://PacktLib.PacktPub.com

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can access, read and search across Packt's entire library of books.

### Why subscribe?

- Fully searchable across every book published by Packt
- Copy and paste, print and bookmark content
- On demand and accessible via web browser

### Free access for Packt account holders

If you have an account with Packt at www.PacktPub.com, you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

## Table of Contents

- Preface 1
- **Chapter 1: What is Quantitative Finance?** 5
  - Discipline 1 – finance (financial derivatives) 5
  - Discipline 2 – mathematics 8
  - Discipline 3 – informatics (C++ programming) 9
  - The Bento Box template 10
  - Summary 12
- **Chapter 2: Mathematical Models** 13
  - Equity 13
  - Foreign exchange 17
  - Interest rates 20
  - Short rate models 20
  - Market models 22
  - Credit 25
  - Structural models 26
  - Intensity models 28
  - Summary 31
- **Chapter 3: Numerical Methods** 33
  - The Monte Carlo simulation method 34
  - Algorithm of the MC method 35
  - Example of the MC method 37
  - The Binomial Trees method 39
  - Algorithm of the BT method 39
  - Example of the BT method 42
  - The Finite Difference method 44
    - Algorithm of FDM 46
    - Example of the FD method 48
    - Summary 50
- **Chapter 4: Equity Derivatives in C++** 51
  - Basic example – European Call 51
  - Advanced example – equity basket 56
  - Summary 60
- **Chapter 5: Foreign Exchange Derivatives with C++** 61
  - Basic example – European FX Call (FX1) 61
  - Advanced example – FX barrier option (FX2) 68
  - Summary 73
- **Chapter 6: Interest Rate Derivatives with C++** 75
  - Basic example – plain vanilla IRS (IR1) 76
  - Advanced example – IRS with Cap (IR2) 82
  - Summary 88
- **Chapter 7: Credit Derivatives with C++** 89
  - Basic example – bankruptcy (CR1) 89
  - Advanced example – CDS (CR2) 94
  - Summary 100
- **Appendix A: C++ Numerical Libraries for Option Pricing** 101
  - Numerical recipes 101
  - Financial numerical recipes 102
  - The QuantLib project 102
  - The Boost library 102
  - The GSL library 103
- **Appendix B: References** 105
- Index 107
