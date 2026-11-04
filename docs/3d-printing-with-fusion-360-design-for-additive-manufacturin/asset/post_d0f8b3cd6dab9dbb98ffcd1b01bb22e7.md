
![](img/baa027bed101c08b496f1f420cee8a5b_0_0.png)

## 3D Printing with Fusion 360

Design for additive manufacturing, and level up your simulation and print preparation skills

1ST EDITION

Sualp Ozel

## 3D Printing with Fusion 360

Design for additive manufacturing, and level up your simulation and print preparation skills

Sualp Ozel

![](img/baa027bed101c08b496f1f420cee8a5b_1_0.png)

## 3D Printing with Fusion 360

Copyright © 2023 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Group Product Manager: Rohit Rajkumar

Publishing Product Manager: Vaideeshwari Roshan

Senior Editor: Aamir Ahmed

Technical Reviewer: Chetan Gowda HS

Book Project Manager: Aishwarya Mohan

Technical Editor: Simran H. Udasi

Copy Editor: Safis Editing

Proofreader: Safis Editing

Indexer: Rekha Nair

Production Designer: Prafulla Nikalje

DevRel Marketing Coordinator: Anamika Singh

First published: November 2023

Production reference: 1101123

Published by Packt Publishing Ltd.
Grosvenor House
11 St Paul's Square
Birmingham
B3 1RB, UK

ISBN 978-1-80324-664-2

www.packtpub.com

I dedicate this book to my family. I love you with all my heart. To my wife, Marlene, you have been my champion and biggest supporter, not only while authoring this book, but throughout our joint life journey. To my sons, Kerem and Koray, thank you for listening to me during countless dinner conversations about 3D printing and for all your ideas about what to 3D print next. To my parents, Bijen and Suat. Thank you for your endless support, love, and inspiration.

## Foreword

I have known and worked with Sualp Ozel for close to seven years, as one of the leading experts in Design Simulation and Additive manufacturing, he is my go to whenever I need to understand new developments and applications in the space. In this book, Sualp takes you on a learning journey from Design for Additive manufacturing, structural simulation, and print preparation through to process simulation and workflow automation.

The first section will get you familiar with the basics of Fusion 360, from how to open new files or upload your existing models to the Fusion software for storage. Sualp demonstrates how to use Fusion to inspect and repair broken mesh files along with how to edit models so that they can be 3D printed effectively using various 3D-printing technologies. The section also covers how to use Fusion's Automated Modeling, Topology Optimization and Stress Simulation capabilities in tandem to generate functional parts that meet performance and manufacturing criteria.

The book also covers hollowing and latticing parts and the various ways you can create lightweight and material saving models along with how to export them to common slicers such as Cura, PreForm and Prusa Slicer.

In the third section you learn all about the *Manufacture* workspace of Fusion 360, how to utilize it as a native slicer to prepare your parts for 3D printing. From how to use Fusion effectively to select a printer and print settings, arrange and orient your parts within your build volume, create support structures based on the needs of your specific printer.

The final section is all about metal printing and automation, where Sualp draws from his years of experience and expertise to explain the unique requirements of metal powder bed fusion printing, how to create efficient support structures and simulate the printing process to detect potential build failures and compensate for thermal distortion.

Understanding these critical areas can save hours of engineering time and the expense of failed prints. Throughout this book, Sualp provides all you need to learn the entire workflow from design to 3D print using the powerful capabilities in Fusion 360 in clear and easy to read resource suitable for people just starting their 3D printing journey, through to those that are ready to adopt some of the most advanced design and simulation tools available.

Duann Scott

3MF Consortium Executive director

Teaching Assistant, MIT xPRO

Founder at Bits to Atoms and the CDFAM Computational Design Symposium Series

## Contributors

## About the author

**Sualp Ozel** is a professional engineer and a senior product manager at Autodesk Inc. Sualp has 15 years of experience in planning roadmaps based on market requirements to deliver timely enhancements to existing products and go to market with new offerings while meeting the market’s expectations and requirements for quality. Since 2016, he has managed Autodesk’s additive manufacturing software portfolio including Fusion 360, Netfabb, Netfabb Local Simulation, and Within Medical. Sualp has also been an adjunct instructor at Carnegie Mellon University since 2014, teaching the *Introduction to CAD and CAE tools* course within the mechanical engineering department.

*I want to thank everyone who supported me in this journey.*

*Thank you to Anthony Graves for making sure I had the best HP Z2 workstation so I could work with Autodesk Fusion seamlessly.*

*Thank you to Pavel Pelčák and the Prusa research organization for providing me with access to FFF and SLA 3D printers and materials to use when writing the book.*

*I also want to thank everyone who worked on Autodesk Fusion 360. Without their work and commitment, the design-and-make community would not be where it is today.*

## Table of Contents

Preface xiii

## Part 1: Design for Additive Manufacturing (DFAM) and Fusion 360

1

## Opening, Inspecting, and Repairing CAD and Mesh files 3

Technical requirements 4
Inspecting Mesh bodies and repairing them 18
Opening and Uploading workflows for CAD models and Mesh files 5
Summary 30
Inserting Mesh workflows for STL, OBJ, and 3MF files 13

2

## Editing CAD/Mesh Files with DFAM Principles in Mind 33

Technical requirements 33
Editing inserted Mesh bodies as mesh bodies 65
Introduction to design history with Fusion 360 34
Editing inserted Mesh bodies after converting them into solid bodies 67
Parametric Modeling 36
Editing inserted Mesh bodies by recreating them as solid bodies 70
Direct modeling 44
Summary 73
Recognizing CAD features and editing them 53
Working with Mesh files natively in Fusion 360 64

## 3

## Creating Lightweight Parts, and Identifying and Fixing Potential Failures with Simulation
75

- Technical requirements 76
- Getting Started with Automated modeling 76
- Utilizing Shape Optimization 88
- Structural simulation to detect and fix common failures 98
- Summary 107

## 4

## Hollowing and Latticing Parts to Reduce Material and Energy Usage
109

- Technical requirements 109
- Hollowing parts 110
- Creating drainage holes 116
- Creating internal lattice structures 124
- Summary 140

## Part 2: Print Preparation – Creating an Additive Setup

## 5

## Tessellating Models and Exporting Mesh Files to Third-Party Slicers
143

- Technical requirements 144
- Common mesh file formats and their differences 145
  - STL format 145
  - OBJ format 146
  - 3MF format 146
- Tessellation, a critical step for 3D printing with non-Fusion 360 slicers 146
  - Tessellating a solid body 146
  - Tessellating a body with a volumetric lattice 148
- Exporting mesh files or sending models to your slicer 151
  - Exporting models directly to third-party slicers 151
  - Exporting models directly to non-Fusion 360 slicers 155
  - Exporting models out of Fusion 360 158
- Summary 159
