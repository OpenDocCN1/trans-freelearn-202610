
## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for this book from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files emailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register to our website using your email address and password.
- 2. Hover the mouse pointer on the SUPPORT tab at the top.
- 3. Click on Code Downloads & Errata.
- 4. Enter the name of the book in the Search box.
- 5. Select the book for which you're looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this book from.
- 7. Click on Code Download.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/Pentaho-8-Reporting-for-Java-Developers. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Downloading the color images of this book

We also provide you with a PDF file that has color images of the screenshots/diagrams used in this book. The color images will help you better understand the changes in the output. You can download this file from https://www.packtpub.com/sites/default/files/downloads/Pentaho8ReportingforJavaDevelopers_ColorImages.pdf.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books - maybe a mistake in the text or the code - we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting [http://www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata), selecting your book, clicking on the Errata Submission Form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title. To view the previously submitted errata, go to [https://www.packtpub.com/books/content/support](https://www.packtpub.com/books/content/support) and enter the name of the book in the search field. The required information will appear under the Errata section.

## Piracy

Piracy of copyrighted material on the internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the internet, please provide us with the location address or website name immediately so that we can pursue a remedy. Please contact us at `copyright@packtpub.com` with a link to the suspected pirated material. We appreciate your help in protecting our authors and our ability to bring you valuable content.

## Questions

If you have a problem with any aspect of this book, you can contact us at `questions@packtpub.com`, and we will do our best to address the problem.

## Introduction to Pentaho Reporting

**Pentaho Reporting** is an easy-to-use, open source, lightweight suite of Java projects built for one purpose: report generation. In this book, you will discover how easy it is to embed Pentaho Reporting into your Java projects, or use it as a standalone reporting platform. Pentaho Reporting's open source license—the GNU **Lesser General Public License (LGPL)**—gives developers the freedom to embed Pentaho Reporting into their open source and proprietary applications at no cost. An active community participates in the development and use of Pentaho Reporting, answering forum questions, fixing bugs, and implementing new features. While many proprietary reporting options are available, none can offer the openness and flexibility that Pentaho Reporting provides its users with.

As with most successful open source projects, Pentaho Reporting has a proven track record, along with a long list of features. Most of this history has been documented in open forums and in email threads, which are still available for folks to browse through and glean ideas from. Starting as a side hobby and turning into an enterprise reporting suite over the course of many years, the Pentaho Reporting Engine and its suite of tools, such as the Report Designer, Reporting Engine, and Reporting SDK, are used as critical components in countless corporate, educational, governmental, and community-based information technology solutions.

In most business software applications, a reporting component is necessary, be it for summarizing data, generating large numbers of customized documents, or simply for making it easier to print information that would be useful in various output formats. With a complete set of features, including PDF, Microsoft Excel, HTML, Text, XML, CSV, and RTF report generation, along with advanced reporting capabilities such as subreports and cross tabs, Pentaho Reporting can crack the simplest of problems quickly, along with solving the more advanced challenges when designing, generating, and deploying reports.

Read on in this chapter to learn more about Pentaho Reporting, its typical uses, history, and origins, along with a more detailed overview of the reporting functionality that Pentaho Reporting provides.

In this chapter, you will cover the following topics:

- Introduction to Pentaho Reporting
- Typical uses of Pentaho Reporting
- Pentaho Reporting history
- Pentaho Reporting architecture

## Introducing Pentaho Reporting

As introduced, Pentaho Reporting is an easy-to-use, open source, lightweight suite of Java projects, built to let the report developer be able to create pixel-perfect reports of his/her data in PDF, Microsoft Excel, HTML, Text, RTF, XML, and CSV formats. These computer generated reports easily refine data from various sources into a human readable form, and can be accessed via an integrated web viewer, saved as files and sent by email to a predefined list of recipients. It is also possible to generate reports as part of a Pentaho Data Integration transformation, to distribute the generated files according to more complex business rules (http://community.pentaho.com/projects/reporting).

![](img/9337ac94537bf7e092e1ea6531dce8fd_48_0.png)

> *Pentaho Data Integration transformations are used to describe the data flows in a process, such as reading from a source, transforming data, and loading it into a target location. You will learn more in Chapter 16, Using Reports in Pentaho Data Integration.*

As it renders such high performance, consuming a considerably low amount of memory, the report processing can scale from small footprint embedded scenarios to large-scale enterprise reporting scenarios. Pentaho Reporting integrates perfectly with the Business Analytics Platform and allows you to share reports with coworkers and peers.

Pentaho Reporting styling is flexible and it permits the report designer to choose the granularity at which they desire to manipulate the entire user interface, right down to the minute details like the font styling on individual characters. In addition to this, you can also add your own images and logos to these reports and select from the various layouts, charts, and tables the one that you wish to apply.

## Introducing Pentaho Reporting tools

Pentaho Reporting can be defined as a suite of open source tools and can be described as being composed of the following:

- **Report Designer**: This is a desktop application providing a design, develop, and test environment to enable the knowledge worker to easily create simple as well as complex reports. The tool is designed to be used by experienced and power users, who are familiar with a few technical concepts.
- **Reporting Engine**: This engine is born as an evolution of the JFreeReport project, a free Java class library for creating reports. The library can be used in both client-side (Report Designer, for example) and server-side applications (web applications, for example).
- **Reporting SDK**: This is a packaging of several resources (the engine, some documentation, and all the libraries required to embed the Pentaho Reporting Engine into your application), to be a valid support for all the developers, particularly, for all the Java developers.

In this book, we will look at the Report Designer and the Reporting SDK in detail, assuming they are the right tools for developers and report designers, to use the Reporting Engine in real life use cases.
