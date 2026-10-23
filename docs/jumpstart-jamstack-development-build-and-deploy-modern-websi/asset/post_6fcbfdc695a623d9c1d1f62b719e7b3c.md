
Chapter 12, APIs – Alexa Skills, teaches you how to build the Jumpstart Jamstack Alexa skill. As an example, you would be able to ask for the upcoming five events. Through this example, you will be able to extend the current skill by retrieving past events, or all events, and you will also be able to create a new skill for any other application you have.

Chapter 13, Tying It All Together, teaches you how to modify the CSS and the final elements of the website, and how to configure and release a GatsbyJS production build.

Chapter 14, Deployment Using Netlify and Azure, will introduce the Netlify serverless continuous deployment hosting service. You will learn how to connect your websites' repositories to Netlify and how Netlify prepares and combines the various files into a fast, efficiently performing, single static file.

Chapter 15, Conclusion, will summarize the concepts learned throughout the book, discuss how the pieces fit together, and explore how the Jamstack community is evolving. The chapter will discuss various external projects and how to contribute to them.

## To get the most out of this book

| Software/hardware covered in the book | OS requirements |
|---|---|
| The React JavaScript framework (Version 16.14.0) | Windows, macOS X, or Linux (any) |
| The Gatsby frontend framework (Version 2.9.2 or higher) | Windows, macOS X, or Linux (any) |
| The Sanity.io unified content platform (Version 2.9.1) | Windows, macOS X, or Linux (any) |
| Netlify (Version 3.18.3) | Windows, macOS X, or Linux (any) |
| Alexa skills | Windows, macOS X, or Linux (any) |

> Note:  
> At the time of writing, the latest versions of Gatsby, React, Netlify, and Sanity have been used. Any future updates for them will be pushed and updated to the GitHub repository.

If you are using the digital version of this book, we advise you to type the code yourself or access the code via the GitHub repository (link available in the next section). Doing so will help you avoid any potential errors related to copy/pasting of code.

## Download the example code files

You can download the example code files for this book from your account at www.packt.com. If you purchased this book elsewhere, you can visit www.packtpub.com/support and register to have the files emailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register at www.packt.com.
- 2. Select the **Support** tab.
- 3. Click on **Code Downloads**.
- 4. Enter the name of the book in the **Search** box and follow the onscreen instructions.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR/7-Zip for Windows
- Ziipeg/iZip/UnRarX for Mac
- 7-Zip/PeaZip for Linux

The code bundle for the book is also hosted on GitHub at `https://github.com/PacktPublishing/Jumpstart-Jamstack-Development`. In case there’s an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at `https://github.com/PacktPublishing/`. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: `https://static.packt-cdn.com/downloads/9781800203495_ColorImages.pdf`.

## Conventions used

There are a number of text conventions used throughout this book.

**Code in text:** Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Mount the downloaded `WebStorm-10*.dmg` disk image file as another disk in your system."

A block of code is set as follows:

```javascript
{
    resolve: "gatsby-source-sanity",
    options: {
      ...clientConfig.sanity,
      token: process.env.SANITY_READ_TOKEN,
      watchMode: !isProd,
      overlayDrafts: !isProd
    }
  }
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in **bold**:

```text
[default]
  exten => s,1,Dial (Zap/1|30)
  exten => s,2,Voice mail (u100)
**exten => s,102,Voice mail (b100)**
  exten => i,1,Voice mail (s0)
```

Any command-line input or output is written as follows:

```bash
npm install -g gatsby
```

**Bold:** Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Select **System info** from the **Administration** panel."

> Tips or important notes  
> Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback:** If you have questions about any aspect of this book, mention the book title in the subject of your message and email us at customer.care@packtpub.com.

**Errata:** Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit www.packtpub.com/support/errata, selecting your book, clicking on the Errata Submission Form link, and entering the details.

**Piracy:** If you come across any illegal copies of our works in any form on the Internet, we would be grateful if you would provide us with the location address or website name. Please contact us at `copyright@packt.com` with a link to the material.

**If you are interested in becoming an author:** If you have a topic that you have expertise in and are interested in either writing or contributing to a book, please visit authors.packtpub.com.

## Reviews

Please leave a review. Once you have read and used this book, why not leave a review on the site that you purchased it from? Potential readers can then see and use your unbiased opinion to make purchase decisions, we at Packt can understand what you think about our products, and our authors can see your feedback on their book. Thank you!

For more information about Packt, please visit packt.com.

# 1

# History of the Jamstack

Welcome to the **Jamstack**. This completely new web development paradigm has excited the information technology industry and is becoming steadily more popular, with new companies constantly forming around it. This book is one of the first of a few tutorials available focused on practical and hands-on experience with the Jamstack.

A technology *stack* represents a specific collection of languages, databases, and operating systems, such as the LAMP stack. The acronym **LAMP** stands for **Linux**, an operating system; **Apache**, a web server; **MySQL**, a database; and **PHP**, a programming language. The Jamstack is actually not a stack in this sense, but rather a new methodology and toolset to produce websites and web applications.

In this chapter, we're going to first look at the history of the web, introduce the Jamstack, and discuss its advantages. To understand how the Jamstack evolved into what it is today, we need to look back at the more-than-two-decade history of the World Wide Web. Web design and web development, the two main industries that evolved from the World Wide Web, developed into two very popular and lucrative occupations, but it wasn't always that way.

These are the main topics that we will cover in this chapter:

- The evolution of the Jamstack
- The rise of the Jamstack
- Our Jamstack
- Getting started with the Jamstack

# The evolution of the Jamstack

The evolution of the Jamstack can be easily explained by looking at how the World Wide Web evolved, starting with its most central component, **HyperText Markup Language (HTML)**.

# HTML

The very first web pages were simply comprised of text with HTML tags, providing markup instructions with the ability to link pages together. In fact, HTML is often mistaken by the average person as a programming language, but it was, at the most fundamental level, a series of symbols that represented formatting instructions. It still gets included in programming language lists, together with actual programming languages such as C and Java. It is merely a markup language, though, despite having evolved rapidly to now include accessibility and semantic features. This means that it is not much more than markup. In its earliest versions, however, it simply provided general formatting instructions.

For example, we could use an h1 tag to represent the header of a page, which would make the text appear larger, or a bold tag to make the text bold. Each page would consist of text, links, and HTML tags.

The following code snippet provides an example of this:

```html
<html>
<head>
<title>My Webpage</title>
</head>
<body>
<h1>Welcome to my webpage.</h1>
</body>
</html>
```
