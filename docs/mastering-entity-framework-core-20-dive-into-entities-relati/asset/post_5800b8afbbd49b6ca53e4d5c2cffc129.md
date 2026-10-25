
You require the following tools to successfully execute the code in this book:

- Visual Studio 2017 Express
- SQL Server 2017 Express
- SQL Server 2017 Developer

## Who this book is for

This book is for .NET Core developers who would like to integrate EF Core in their application. Prior knowledge of .NET Core and C# is assumed.

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning.

Code words in a text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, and user input are shown as follows: "The new column should be included in the Edit method of PostController."

A block of code is set as follows:

```
public interface IPostDetailQueryHandler
{
    Task<Post> Handle(PostDetailQuery query);
}
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
public async Task<IActionResult> Index()
{
    return View(await _context.Blogs.FromSql("Select *
    from dbo.Blog").ToListAsync());
}
```

You can download the code files by following these steps:

- 1. Log in or register to our website using your e-mail address and password.
- 2. Hover the mouse pointer on the **SUPPORT** tab at the top.
- 3. Click on **Code Downloads & Errata**.
- 4. Enter the name of the book in the **Search** box.
- 5. Select the book for which you're looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this book from.
- 7. Click on **Code Download**.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/Mastering-Entity-Framework-Core. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Downloading the color images of this book

We also provide you with a PDF file that has color images of the screenshots/diagrams used in this book. The color images will help you better understand the changes in the output. You can download this file from https://www.packtpub.com/sites/default/files/downloads/MasteringEntityFrameworkCore20_ColorImages.pdf.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the Errata Submission Form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title.

To view the previously submitted errata, go to https://www.packtpub.com/books/content/support and enter the name of the book in the search field. The required information will appear under the Errata section.

## Piracy

Piracy of copyrighted material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors and our ability to bring you valuable content.

## Questions

If you have a problem with any aspect of this book, you can contact us at questions@packtpub.com, and we will do our best to address the problem.

## Kickstart - Introduction to Entity Framework Core

I still remember the days when we were spending quite a lot of time on working with relational databases rather than just focusing on solving business problems; those days are definitely gone. To elaborate, let's jot down the issues we had before ORM:

- Data access layers were not portable, which made it hard to change from one platform to another.
- There were no abstractions, which forced us to write manual mapping between object-oriented objects and data entities.
- Vendor-specific SQL statements, which requires knowledge to port between different RDBMS systems.
- Relied heavily on triggers and stored procedures.

The entire product development process shifted towards tools and open source platforms, and even Microsoft took that path from .NET Core onward. If we keep spending time on writing code which could be achieved through tools, we might end up looking like cavemen.

The Entity Framework was created to address this concern; it was not introduced with the initial .NET framework but rather was introduced in .NET Framework 3.5 SP1.

If we look closely, it was obvious that the .NET team built it for the following reasons:

- To minimize the time spent by the developers/architects on stuff like abstractions and the portable data access layer
- So that the developers do not require vendor specific SQL knowledge
- So that we can build object-oriented business logic by eradicating triggers and SPs

![](img/17e01f90db603e72c0f0c6a7dce8920c_22_0.png)

This book uses Visual Studio 2017 (the latest at the time of writing) and ASP.NET Core 2.0 MVC with Entity Framework 2.0. Even though Entity Framework 2.0 is the latest version, it is still an evolving one, so it would take time for the .NET team to develop all the existing features of Entity Framework 6.2 based on the full .NET Framework.

We will cover the following topics here:

- Prerequisites
- Creating a new project
- Installing Entity Framework 2.0
- Data models
- Database context
- Registering the context in services (.Net Core DI)
- Creating and seeding databases
- Performing CRUD operations

## Prerequisites

.NET Core, the open source platform, paved the way for multi-platform support in Visual Studio 2017. The editors came in different flavors, supporting both platform-specific and cross-platform IDEs:

- **Visual Studio:** An exclusive edition for Windows with Community, Professional and Enterprise editions:

Visual Studio for Mac can be downloaded from `https://www.visualstudio.com/vs/visual-studio-mac/`.

- **Visual Studio Code:** The cross-platform editor from Microsoft for Windows, Linux, and macOS:

![](img/17e01f90db603e72c0f0c6a7dce8920c_24_0.png)

Download the desired version/edition of Visual Studio Code from `https://www.visualstudio.com/downloads/`.

The Visual Studio 2017 installer is segregated into workloads, individual components, and language packs. We will be installing and using **Visual Studio Community 2017** with the workloads **ASP.NET and web development** and **.NET Core cross-platform development**. The workload is a combination of one or more individual components which can also be installed from the **Individual components** tab of the installer, as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_25_0.png)

We have looked at the different flavors/editions of Visual Studio available to us, and we will be using Visual Studio Community on our journey, which is free of charge for private and test purposes. It is up to the reader to pick an edition which suits their needs (the tools and scaffolding available in the IDE might differ).

## Creating a new project

Open Visual Studio and create a new project either from the **File** menu or from the **Start** page.

## The Start page

From the **New Project** section, create a new project using any one of the following approaches:

- 1. Select **Create new project...** On the left pane, select **Templates | Visual C# | .NET Core**. Select the **ASP.NET Core Web Application** template from the list.
