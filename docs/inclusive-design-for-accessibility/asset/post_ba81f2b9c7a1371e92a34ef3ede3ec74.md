
# <packt>

# Inclusive Design for
Accessibility

A practical guide to digital accessibility, UX, and inclusive web and app design

![](img/3bdc9e2d41de898f8a3bcc9634b0000e_0_0.png)

Dale Cruse & Denis Boudreau et al.

Foreword by Glenda Sims

Chief Information Accessibility Officer (CIAO) at Deque

# Inclusive Design for Accessibility

Copyright © 2025 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors nor Packt Publishing or its dealers and distributors will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Portfolio Director:** Ashwin Nair

**Relationship Lead:** Nitin Nainani

**Project Manager:** Aparna Nair

**Content Engineer:** Hayden Edwards

**Technical Editor:** Sweety Pagaria

**Copy Editor:** Safis Editing

**Indexer:** Rekha Nair

**Proofreader:** Hayden Edwards

**Production Designer:** Deepak Chavan

**Growth Lead:** Sohini Ghosh

First published: August 2025

Production reference: 1140725

Published by Packt Publishing Ltd.
Grosvenor House
11 St Paul's Square
Birmingham
B3 1RB, UK.

ISBN 978-1-83588-822-3
www.packtpub.com

To my parents, Paul and Geraldine, who taught me – loudly and without apology – that people with disabilities are just that: people. Your unwavering clarity became my compass. And to my wife, Heather Humble – my fiercest believer, my truest partner. This entire book carries your fingerprints, whether visible or not. You’re my favorite.

- Dale Cruse

To those living with invisible disabilities or neurodivergent minds. To those who feel they must hide their truth to stay safe, fit in, or be seen as “enough.” To those who’ve mastered the art of masking, not because they want to, but because they have to. This book is for you. May the world soon be one where we no longer have to mask to belong.

- Denis Boudreau

## Contributors

## About the authors

**Dale Cruse** is a highly sensitive empath driven by a lifelong commitment to equal opportunity, inspired by his parents with disabilities. He created accessibility programs at Twitch and McGraw-Hill Education and led inclusive design initiatives at Microsoft and Deque, where he also helped launch the industry-standard axe DevTools accessibility testing plugin. Dale shared his expertise in the book *Accessibility for Everyone*, and *The Web Ahead* podcast, and is the author of *HTML5 Multimedia Development Cookbook* for Packt Publishing.

**Denis Boudreau** is an international expert in digital accessibility and inclusive leadership with 25 years of experience in the field, working with leaders and executives who are no longer willing to overlook disability inclusion and want to transform their leadership approach from “inclusive-ish” to truly inclusive. He specializes in supporting organizations wishing to adopt more inclusive leadership and communication practices, particularly for their workforce and clients who live with disabilities, are aging, or are marginalized by technology. Offering thought leaders and communicators the means to extend the reach of their message to a much broader audience, he is also the author of the Amazon bestseller *The Inclusive Speaker*, published in 2023.

## About the contributing authors

**Dr Angela Young** (they/them) is a bold advocate for digital accessibility. They drive change among queer, deaf, disabled, and neurodiverse professionals in tech through education, strategy, and storytelling. A seasoned speaker and 18-year educator, Angela has led accessibility initiatives at scale, empowering thousands to build inclusive digital experiences. They bring humor, heart, and deep expertise to every conversation. Angela holds a doctorate in education and certifications in accessibility and leadership.

**Maya Sellon** is an inclusive design and accessibility leader with over 20 years of experience across North America, Asia, and Europe. She’s currently completing her postgraduate degree in assistive technology and human services at CSUN. Maya has developed and implemented accessibility practices for global organizations, embedding inclusivity at the heart of design and technology. With a career spanning civil engineering, user experience, and digital accessibility, she brings a diverse and well-rounded perspective. A tech enthusiast, Maya believes accessibility is about people – not just rules or technology. In her free time, she enjoys drumming and is training to become a certified solo skydiver.

**Julianna Rowsell** (she/her) is a chronically ill, neurodivergent, and disabled product leader with ADHD, ankylosing spondylitis, and anemia. They bring 20+ years of expertise in accessibility, disability justice, and equity, challenging extractive systems to co-create inclusive just futures. They hold a master’s of design in inclusive design from OCAD University. Julianna has helped organizations such as the Government of Canada and leading tech companies make moves toward big bets that focus on accessible outcomes and equitable futures. In 2022, they founded, with their partner, an accessible, sustainable, regenerative flower farm.

**Nandita Gupta** is a senior product manager of accessibility at Cisco, where she works on creating accessible and user-friendly products for all. She was named ADCOLOR in Tech 2024, and featured in NASDAQ, Forbes, Times of India, and other media outlets. As a TEDx speaker, she has spoken internationally at numerous conferences and works within the intersection of product and accessibility. She is a board member on different boards within the community including Lime Connect and the Accessibility Leadership Council for Disability: IN. She has been actively involved in various mentoring organizations over the past years and was inducted into the Susan M. Daniels Disability Mentoring Hall of Fame.

**Jennifer Chadwick** is an award-winning senior digital accessibility strategist and coach, who works in Canada, the UK, Europe, and the US. She teaches organizations to understand the needs of users with disabilities and develops action plans for product design, customer service, marketing, HR, and procurement. Jennifer has spoken at CSUN, AccessU, the University of Toronto, and the United Nations COSP. She co-chairs the W3C Accessibility Roles and Responsibilities Mapping (ARR M) community group and contributed to the standards WCAG Silver 3.0 and the Accessibility Conformance Testing Rules (ACT 1.0).

**Crystal Scott** is a Christian wife, mother of three extraordinary children, and a Certified Professional in Web Accessibility (CPWA). With a decade of frontend development experience and nearly six years specializing in accessibility, she has dedicated time to mastering Webflow and building accessible websites, with plans to launch Graceful Web Studio in 2025. She is also a design annotation ninja and the founder of the Accessibility Book Club, a thriving LinkedIn community. Crystal is pursuing a B.A. in graphic design and media arts at SNHU and is passionate about lifelong learning, technology, and user-centered design. She lives in Yakima, Washington, with her family and two blue nose pit bulls.

**Chris McMeeking** is an accessibility-focused engineer and the founder/CTO of MobA11y, where he leads teams in reimagining how AI can support people with disabilities. With over a decade of experience, Chris has shaped mobile accessibility at companies such as CVS Health and Deque Systems, building tools that help developers create more inclusive apps. His early work on Switch Control – now a core feature on iOS and Android – originated from a University of Michigan project, where he later served as a longtime mentor. Chris brings deep technical insight and human-centered leadership to every product he touches.

**Dr. Keith Newton** is a senior technology specialist and leader in digital accessibility, AI, and user-centered design. An invited expert in W3C Accessibility Platform Architecture and Maturity working groups, Dr. Keith has led enterprise-wide transformations by integrating AI and machine learning to improve accessibility testing, organizational maturity, and governance. He’s collaborated with brands such as eBay, Warner Bros. Discovery, Salesforce, and American Express, developing scalable UX/UI solutions, building training programs, and promoting inclusive design. His deep expertise in WCAG compliance, assistive tech testing, and digital transformation has established him as a sought-after authority whose innovative contributions continue to shape the future of accessible technology.
