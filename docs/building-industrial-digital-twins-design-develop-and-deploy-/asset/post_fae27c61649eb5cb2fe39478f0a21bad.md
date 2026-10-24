

Chapter 2, Planning Your Digital Twin, identifies the key criteria to assess the use of a Digital Twin for your specific industrial challenge or opportunity. The chapter addresses prerequisites for Digital Twins and organizational factors to consider.

Chapter 3, Identifying the First Digital Twin, covers the selection of Digital Twin candidates for a prototype, each from the perspective of the type of organization that is evaluating Digital Twins for their use cases.

Chapter 4, Getting Started with Our First Digital Twin, explains the planning framework, how to validate the problem statement and expected outcomes, and the proposed business process for developing your Digital Twin. It identifies the different types of technology platforms that can be used to create the first Digital Twin prototype of a wind turbine.

Chapter 5, Setting Up a Digital Twin Prototype, provides practical step-by-step guidance for selecting and setting up the infrastructure on a cloud platform in preparation for building the Digital Twin prototype in the next chapter.

Chapter 6, Building the Digital Twin Prototype, describes the end-to-end process of building a real-world Digital Twin prototype on the Microsoft Azure Digital Twins platform. The chapter covers the complete development process, including testing, technical evaluation, and business validation steps.

Chapter 7, Deployment and Value Tracking, guides you through the functional testing of the Digital Twin prototype and evaluates different deployment models for scaling the prototype to an operational production solution. The chapter further suggests value tracking approaches that demonstrate the value of the Digital Twin solution to the different stakeholders in an industrial enterprise.

Chapter 8, Enhancing the Digital Twin, describes the possibilities beyond the first Digital Twin prototype. The chapter looks at the "art of possible" with Digital Twins and some of the emerging opportunities for Digital Twins to provide safer, greener, more responsible industrial operations with positive returns on the value at stake for all stakeholders. This chapter goes in depth into the application of Digital Twins for renewable energy generation.

## To get the most out of this book

You will get the most out of this book if you have basic technical proficiency in programming concepts such as JSON and Azure Functions. These skills are not prerequisites.

The examples in the book provide guidance on the potential application of Digital Twins in your organization, but you will get additional value from the book if potential applications are identified as you read the book and create your first prototype.

| Software/hardware covered in the book | Operating system requirements |
|---|---|
| Azure Digital Twins on the Azure cloud platform – free trial subscription available | None – solutions are browser-based |
| JSON | |

If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book's GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

The readers can stay in touch with the authors and post their questions and discussion topics in the Slack Group for this book:
industrialdigitaltwin.slack.com

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Building-Industrial-Digital-Twin. If there's an update to the code, it will be updated in the GitHub repository.
We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781839219078_ColorImages.pdf.

## Conventions used

There are a number of text conventions used throughout this book.
Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Mount the downloaded WebStorm-10*.dmg disk image file as another disk in your system."

A block of code is set as follows:

```
html, body, #map {
    height: 100%;
    margin: 0;
    padding: 0
}
```

When we wish to draw your attention to a particular part of a code block, the relevant
lines or items are set in bold:

```
[default]
exten => s,1,Dial(Zap/1|30)
exten => s,2,Voicemail(u100)
exten => s,102,Voicemail(b100)
exten => i,1,Voicemail(s0)
```

Any command-line input or output is written as follows:

```
$ mkdir css
$ cd css
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For
instance, words in menus or dialog boxes appear in **bold**. Here is an example: "Select **System info** from the **Administration** panel."

Tips or Important Notes
Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, email us at
customercare@packtpub.com and mention the book title in the subject of
your message.

**Errata**: Although we have taken every care to ensure the accuracy of our content,
mistakes do happen. If you have found a mistake in this book, we would be grateful if
you would report this to us. Please visit www.packtpub.com/support/errata
and fill in the form.

**Piracy:** If you come across any illegal copies of our works in any form on the internet,
we would be grateful if you would provide us with the location address or website name.
Please contact us at <copyright@packt.com> with a link to the material.

**If you are interested in becoming an author:** If there is a topic that you have expertise
in and you are interested in either writing or contributing to a book, please visit
authors.packtpub.com.

## Share Your Thoughts

Once you've read _Building Industrial Digital Twins_, we'd love to hear your thoughts! Please
click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're
delivering excellent quality content.

## Section 1: Defining Digital Twins

This part of the book will introduce the concept of Digital Twins to a novice and why Digital Twins are needed. You will also understand the purpose and benefits of Digital Twins.

This section comprises the following chapters:

- Chapter 1, Introduction to Digital Twin
- Chapter 2, Planning Your Digital Twin

## 1 Introduction to Digital Twin

The Digital Twin is a concept that has gained a lot of popularity recently. Many analysts, vendors, and customers agree that it is poised for proliferation as its value is realized and recognized, specifically in industrial environments.

This book aims to enable you to build your first Digital Twin prototype or minimum viable Digital Twin. Before we start looking at the Digital Twin’s technical aspects, it is essential to understand what a Digital Twin is, how it came to be, and the business value of Digital Twins. Also, what are the ideal prospects for Digital Twins, especially if you are just starting out?

The generic concepts, characteristics, and principles around Digital Twins will be described in this chapter without us focusing on any specific technology for Digital Twins. In later chapters, we will focus on the technical development of a sample Digital Twin to demonstrate how to create your first Digital Twin prototype. This chapter aims to provide you with a shared understanding of Digital Twins, their characteristics, and how they can be applied. This understanding will be used later in this book when you configure your first Digital Twin.

