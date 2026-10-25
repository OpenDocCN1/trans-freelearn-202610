
## Free access for Packt account holders

If you have an account with Packt at [www.PacktPub.com](http://www.PacktPub.com), you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

## Preface

This book is a quick and useful guide to learning the TypeScript language. The language features that TypeScript provides on top of JavaScript are covered thoroughly in this book with hands-on examples. TypeScript is a fairly new development language that can ease the pain of normal JavaScript development. Starting from downloading the compiler, covering language features, and implementing a dynamic application, this book will leave you ready to create new, large-scale JavaScript-based applications.

## What this book covers

[Chapter 1](#), *Getting Started with TypeScript*, covers setting up an environment for developing TypeScript applications and creating a simple application.

[Chapter 2](#), *TypeScript Basics*, covers the primary language features that TypeScript creates on top of JavaScript and how each of these features compiles into plain JavaScript.

[Chapter 3](#), *The TypeScript Compiler*, examines the TypeScript compiler and the different parameters that it accepts. The results of the different parameters will be discussed as well as how they affect the final JavaScript output.

[Chapter 4](#), *Object-oriented Programming with TypeScript*, is all about the basics of object-oriented programming. You will be presented with the benefits that TypeScript adds to make ECMA Script a more full-fledged object-oriented language.

[Chapter 5](#), *Creating a Simple Drawing Application*, walks you through creating a simple drawing application using the concepts already covered in the book. By the end of the chapter, you will have created a web-based drawing application that will give you a good understanding of writing complex applications using TypeScript.

[Chapter 6](#), *Declaration Files and Library Integrations*, discusses declaration files and how they help us integrate with other JavaScript libraries. Libraries such as jQuery, KnockoutJS, and RequireJS will be covered.

[Chapter 7](#), *Enhancing the Drawing Application*, re-examines the drawing application and shows you how to create a more reusable set of objects. Module definitions will be created and the process of minifying code is covered.

[Chapter 8](#), *Debugging TypeScript*, discusses the different options available to debug TypeScript once it is deployed and running. We also cover unit testing and test-driven development, which will allow us to test functionality with code.

## What you need for this book

TypeScript has a standalone compiler install, so any text editor can be used to develop TypeScript applications. The examples provided with this book will use Microsoft’s Visual Studio, and it is recommended that you use it too to help you follow the examples; however, this is not a requirement. A basic understanding of JavaScript and web development is required as not every language construct will be covered; just the ones that TypeScript provides on top of it.

## Who this book is for

This book is intended to introduce the TypeScript language and its features to anyone looking to develop rich web applications. Whether you are new to web development or are an experienced engineer with strong JavaScript skills, this book will get you writing code quickly. A basic understanding of JavaScript and its language features is necessary for this book.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book—what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an e-mail to <feedback@packtpub.com>, and mention the book title via the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on www.packtpub.com/authors.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the **errata submission form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded on our website, or added to any list of existing errata, under the Errata section of that title. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at <copyright@packtpub.com> with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at <questions@packtpub.com> if you are having a problem with any aspect of the book, and we will do our best to address it.

## Chapter 1. Getting Started with TypeScript

There are many languages available that can be used to create cross-platform applications and these types of code applications are being created daily. Some of the more common languages that these applications use are Java, C, and JavaScript and each has its advantages and disadvantages. While JavaScript is easy to get started with, it is also easy to lose control of. C requires a lot of overhead to create complex applications such as memory management, and Java requires the Java runtime to be installed. As applications grow and become more complex, so does the need to produce maintainable code.

TypeScript is a new open source language created to make web development easier and more reliable. In this chapter we will:

- Understand what TypeScript is
- Learn how TypeScript improves on the foundations of JavaScript
- Explore the ways TypeScript makes code more maintainable
- Learn how to get the TypeScript compiler
- Create our first application in TypeScript

## The advantages of TypeScript

As hardware technology has advanced, so too has the need for more advanced software applications to run not just on one device but on an array of devices. JavaScript is the natural solution to this cross-platform model since almost all modern browsers are capable of running it. Unfortunately, the development paradigm of JavaScript is still dependent upon complex pattern knowledge, and debugging must be done at runtime. Creating large applications in JavaScript can be difficult and structuring the code in a maintainable form is just as difficult. TypeScript, an open source language project started by Microsoft, aims to take JavaScript development to the next level.

JavaScript originally ran as an interpreted language and still does in some places, while modern browsers convert it to machine code during execution. What this means is that the code you write is the code being used at runtime. The advantage of using interpreted languages is that they provide a rapid development model with very little overhead for the developer. They work in cross-platform environments because the code does not need to be compiled for each new device it is deployed to. Interpreted languages aren’t without their share of disadvantages either though. In most modern implementations, JavaScript code must be run through a just-in-time compiler first, but even with the help of this compilation, the dynamic typing of the language makes this a difficult task and performance can suffer. On top of the performance hit, JavaScript code must still be deployed before testing can be done. A single syntax error will force you to run the application, track down the source of the problem, fix it, and then run the application again, which wastes a lot of time and can be quite frustrating. Developers like to refactor code as better ways to solve problems arise or new functionality is needed. This refactoring could cause other parts of the application to fail and without performing complete regression testing the problem could be overlooked. To interact with an object you must have intimate knowledge of the type that the object represents. TypeScript aims to solve these problems through a variety of additions to the JavaScript language including a compilation step with a detailed list of errors.

TypeScript is a statically typed compiled language that generates JavaScript code that can be used in cross-platform scenarios. You may be thinking to yourself at this point: why would I want to rewrite all of the applications I already have in this new language? The simple answer is you don’t have to. TypeScript is just a superset of JavaScript that gets compiled into plain JavaScript. Although almost all of your existing JavaScript code is valid TypeScript code, certain pieces will need to be adapted to ensure safe compilation; however, they will generally improve the quality of your code. You are free to type your code as strong or as weak as you wish but I highly recommend embracing the typing system completely.

JavaScript was originally created as a scripting language, but as the need for larger applications has grown so has the need for those applications to contain more reusable components and libraries. JavaScript is dynamic enough to act as both a functional language and an object-oriented one through certain design patterns. Object-oriented code
