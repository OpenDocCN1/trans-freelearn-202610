
# Expert Data Modeling with Power BI

Enrich and optimize your data models to get the best out of Power BI for reporting and business needs

Foreword by:  
Christian Wade  
Principal Program Manager, Microsoft

Second Edition

<packt>

Soheil Bakhshi

![](img/5d2bce74fd6fceebed14fd1488bf7088_1_0.png)

![](img/5d2bce74fd6fceebed14fd1488bf7088_1_1.png)

# DirectQuery Connections to Power BI Datasets and Analysis Services in Composite Models

# New Options, Features, and DAX Functions

# Other Books You May Enjoy

# Index

# What this book covers

*Chapter 1, Introduction to Data Modeling in Power BI*, briefly describes different functionalities of Power BI and why data modeling is important. This chapter also reveals some important notes to be considered around Power BI licensing, which could potentially affect your data model. This chapter introduces an iterative data modeling approach, which guarantees an agile Power BI implementation.

*Chapter 2, Data Analysis eXpressions and Data Modeling*, does not discuss DAX in detail, as in Part 3 and 4 of this book DAX is heavily used to solve different data modeling challenges. Therefore, we’ll only focus on the DAX functionalities that are harder to understand and are very relevant to data modeling. This chapter starts with a quick introduction to DAX, then we jump straight into virtual tables and time intelligence functionalities and their applications in real-world scenarios.

*Chapter 3, Data Preparation in Power Query Editor*, quickly explains the components of Power Query and their application. It expresses the importance of creating query parameters and user-defined functions along with real-world use cases and scenarios to demonstrate how powerful they are in building much more flexible and maintainable models.

*Chapter 4, Getting Data from Various Sources*, explains how to get data from different data sources that are more commonly used in Power BI. Then, the importance of data source certification is explained, which helps you set your expectations on the type of data you’re going to deal with. This is especially helpful in estimating data modeling efforts. Different connection modes are also explained in this chapter.

*Chapter 5, Common Data Preparation Steps*, explains common data preparation steps along with real-world hands-on scenarios. A combination of what you have learned so far in this book and the steps discussed in this chapter gives you a strong foundation to go on to the next chapters and build your data models more efficiently.

modeler is one who is always open to new challenges. You may face some of the advanced business requirements discussed in this chapter or you may face something different but similar. The message we want to send in this chapter is to think freely when dealing with new business challenges and try to be innovative to get the best results.

*Chapter 11, Row-Level and Object-Level Security*, explains how to implement Row-Level Security (RLS) and Object-Level Security (OLS) in a Power BI data model. Dealing with RLS and OLS can be complex, and knowing how to deal with different situations requires deep knowledge of data modeling and filter propagation concepts. Our aim in this chapter is to transfer that knowledge to you so you can design and implement high-performing and low-maintenance data models.

*Chapter 12, Dealing with More Advanced Data Warehousing Concepts in Power BI*, explains two concepts coming from data warehousing, **Slowly Changing Dimensions (SCDs)** and degenerate dimensions. This chapter also demonstrates when and how we can implement these concepts in a Power BI data model.

*Chapter 13, Introduction to Dataflows*, briefly introduces Dataflows, another available feature in Power BI. This chapter is designed to cover the basics and help you to build robust building blocks for your learning journey. This chapter also explains how to export/import Dataflows, and how the no-code/low-code experience and query plan work in Dataflows.

*Chapter 14, DirectQuery Connections to Power BI Datasets and Analysis Services in Composite Models*, introduces new terminologies related to composite models and discusses how to resolve more complex scenarios with fewer issues. This chapter also covers RLS challenges in composite models with DirectQuery connections to either a Power BI dataset or Azure Analysis Services and explains the challenges around setting dataset permissions for contributors after RLS is created.

*Chapter 15, New Options, Features, and DAX Functions*, introduces field parameters, Power BI Datamarts, and some new DAX functions. One of the main features is Field Parameters, which allow developers to create dynamic reports where users can switch between different fields using a

For optimal performance, having 8 GB of RAM and a 64-bit processor is recommended.

- Software: Install the latest version of Power BI Desktop from the Microsoft Store or the Power BI website on a Windows machine with Windows 8.1 or Windows Server 2012 R2 or later. You need to have .NET Framework 4.6.2 or higher and the Microsoft Edge browser (Internet Explorer is no longer supported). Power BI Desktop automatically installs WebView2. It is recommended to use a client version of Windows, such as Windows 10, instead of Windows Server. Power BI Desktop doesn’t support Internet Explorer Enhanced Security Configuration.
- Security: You need to have a valid Power BI account to sign in to Power BI Desktop and publish reports. You also need to have permission to access the data sources and services that you want to use in your reports.

The following are download links for Power BI Desktop:

- Power BI Desktop from the Microsoft Store:  
  https://aka.ms/pbidesktopstore
- Power BI Desktop from the web: https://www.microsoft.com/en-us/download/details.aspx?id=58494

You can sign up for a Power BI Service as an individual. Read more here: https://learn.microsoft.com/en-us/power-bi/fundamentals/service-self-service-signup-for-power-bi?WT.mc_id=DP-MVP-5003466.

> Note
>
> Since January 31, 2021, Power BI Desktop is no longer supported on Windows 7.

# Tabular Editor

DAX Debug Output is a free community tool that allows developers to view the results of DAX expressions run in Power BI Desktop. This can help troubleshoot and optimize complex DAX formulas and measures. This tool is currently in public preview and the developer has not released much information about its system requirements yet. It currently works with the Power BI Desktop August 2022 release or later. DAX Debug Output is available to download from here:  
https://github.com/pbidax/DAXDebugOutput/releases/tag/v0.1.0-beta.

If you are using the digital version of this book, we advise that you type the code yourself or access the code via the GitHub repository (link available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

# Download the example code files

The code bundle for the book is hosted on GitHub at  
https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here:  
https://packt.link/XAVMa.

# Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input,
