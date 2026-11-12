
## Preface

Any command-line input or output is written as follows:

```
roo> logging_setup --level DEBUG --package ROOT
Updated SRC_MAIN_RESOURCES\log4j.properties
```

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus, or dialog boxes for example, appear in the text like this: "Create a new Flight Description by selecting the **Create new Flight Description** option from the menu and entering values for **Origin**, **Destination**, and **Price** fields".

> Warnings or important notes appear in a box like this.

> Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book—what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an e-mail to <feedback@packtpub.com>, and mention the book title via the subject of your message.

If there is a book that you need and would like to see us publish, please send us a note in the **SUGGEST A TITLE** form on `www.packtpub.com` or e-mail to <suggest@packtpub.com>.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on `www.packtpub.com/authors`.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at `http://www.PacktPub.com`. If you purchased this book elsewhere, you can visit `http://www.PacktPub.com/support` and register to have the files e-mailed directly to you.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the errata submission form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded on our website, or added to any list of existing errata, under the Errata section of that title. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## Getting Started with Spring Roo

In this chapter, we will cover the following topics:

- Setting up Roo
- Getting help and hints from Roo
- Creating a Roo project
- Importing a Roo project into Eclipse or IntelliJ IDEA IDE
- Configuring logging
- Viewing properties defined in a properties file
- Managing properties defined in a properties file
- Creating a Java class
- Adding attributes to a Java class
- Creating a Java interface
- Referring to a type from the Roo shell
- Creating application artifacts from Roo script

## Introduction

Java provides an excellent platform for enterprise application development but has often been weighed down by productivity issues. For instance, if you want to develop a web application, then you need to learn a web framework, create JSPs or views, implement service layer, integrate services with a persistence framework, implement persistence logic, create configuration files for different application layers, write unit and integration tests, write build scripts, and so on. Phew! It seems like a lot of work to even create a simple web application that uses a standard set of Java frameworks. This is the reason why many organizations moved to platforms, which offer quick-start to developing simple or medium complexity applications. With the arrival of open source frameworks like Spring and Hibernate, Java platform received a big boost in terms of developer productivity and simplicity; it was still off the mark when it came to productivity levels compared to platforms like Ruby on Rails, until Spring Roo arrived.

Spring Roo is the next generation rapid application development tool for Java programming language. It is an open source tool, which comes under the umbrella of SpringSource (http://www.springsource.org/) projects. Applications developed using Spring Roo make use of the Spring programming model, which already has a proven track record of delivering portable, testable, and maintainable enterprise applications.

Spring Roo is an easy-to-use tool for rapidly developing Java enterprise applications using well-recognized frameworks such as Spring, Hibernate, AspectJ, Spring Web Flow, Spring Security, GWT, Flex, and so on. Spring Roo takes care of creating project structure for your enterprise application, adding support to use Maven for building and deploying the application, creating application architecture based on your choice of technologies, creating unit and integration tests based on your choice of testing framework, and so on. Spring Roo provides an interactive, intuitive, text-based interface through which you enter the details of your application in a step-by-step fashion to create a working application in minutes.

> > When using Spring Roo, it's up to the enterprise application developer to choose the technology or framework to use in developing the application. For instance, you can choose Hibernate or OpenJPA for persistence and Spring Web Flow or GWT for the web layer.

So, what do you need to learn to develop applications using Spring Roo? As we will see shortly, you need to learn hardly anything to develop enterprise applications using Spring Roo. Spring Roo's key goal has been to utilize existing knowledge of enterprise developers and automate most of the tasks that a developer had to perform in a typical enterprise application development project. This results in increased developer productivity with nearly no learning curve. Isn't it exciting that you can rapidly develop Java enterprise applications using standard Java technologies without learning anything new?

Before we jump into using Spring Roo tool, let's have a look at the key benefits of using Spring Roo:

- Improved enterprise developer's productivity: Spring Roo improves enterprise developer's productivity by auto-generating code based on instructions provided by developer or inferred from the code already generated by Spring Roo.
- Productivity improvement throughout project lifecycle: Spring Roo not only gets you quickly started with the project but it also gives you productivity improvements over the lifetime of the project.
- No extra layers of abstraction: Spring Roo doesn't attempt to hide implementation details from enterprise developers, making it easy for developers to understand the code and modify it as per their need.
- No runtime dependency: If you develop applications using Spring Roo, at runtime your application code is only dependent on frameworks that you used for developing the application, nothing more, nothing less.
- No vendor lock-in: At any time you decide to move away from using Spring Roo, you can use Eclipse IDE or STS (Spring Tool Suite—SpringSource's Eclipse flavor) to remove Spring Roo specific details from your application.

The following figures shows what you can do with Spring Roo and the benefits that you get:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_21_0.png)

The given figure shows that you can also use Spring Roo for creating proof of concepts, for creating prototypes, and for learning new technologies. With Spring Roo you can quickly create a working application in minutes, which makes it an ideal candidate for developing prototypes and creating proof of concepts. A developer can use Spring Roo to create a simple application using the technology that (s)he wants to learn and play around with it or go through the Roo-generated source code to quickly learn about the technology.
