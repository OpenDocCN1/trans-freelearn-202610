
You should have some experience of working with a framework for web application development, and you should be looking to deploy applications that will perform well on live production environments. These could be virtual machines or hosted by a cloud service provider such as AWS or Azure.

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning. Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "The update command is different to the upgrade command, but they are often used together."

A block of code is set as follows:

```
#import packages into the project
from bs4 import BeautifulSoup
from urllib.request import urlopen
import pandas as pd
```

Any command-line input or output is written as follows:

```
sudo apt-get install docker-ce
```

New terms and important words are shown in bold. Words that you see on the screen, for example, in menus or dialog boxes, appear in the text like this: "Right-click on your web application project in Solution Explorer and select Manage NuGet Packages... to open the graphical package manager window."

![](img/5e416ac29f1bccaad147c6829853fd77_22_0.png)

Warnings or important notes appear like this.

![](img/5e416ac29f1bccaad147c6829853fd77_22_1.png)

Tips and tricks appear like this.

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/ASPdotNET-High-Performance. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books-maybe a mistake in the text or the code-we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the Errata Submission Form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title. To view the previously submitted errata, go to https://www.packtpub.com/books/content/support and enter the name of the book in the search field. The required information will appear under the Errata section.

## Piracy

Piracy of copyrighted material on the internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the internet, please provide us with the location address or website name immediately so that we can pursue a remedy. Please contact us at copyright@packtpub.com with a link to the suspected pirated material. We appreciate your help in protecting our authors and our ability to bring you valuable content.

## Questions

If you have a problem with any aspect of this book, you can contact us at questions@packtpub.com, and we will do our best to address the problem.

## 1
What's New in ASP.NET Core 2?

There are many things that have changed in version 2 of the ASP.NET Core framework. There have been a lot of improvements in some of its supporting technologies as well. Now is a great time to give it a try, as its code has been stabilized and the pace of change has settled down a bit.

There were significant differences between the original release candidate and version 1 of ASP.NET Core and further alterations between version 1 and version 2. Some of these changes have been controversial, particularly ones related to tooling; however, the scope of .NET Core has grown massively, and this is a good thing.

One of the high-profile differences between version 1 and version 2 is the change (some would say regression) from the new JavaScript Object Notation (JSON)-based project format back to the Extensible Markup Language (XML)-based csproj format. However, it is a simplified and stripped-down version, compared to the format used in the original .NET Framework.

There has been a move toward standardization between the different .NET Frameworks, and .NET Core 2 has a much larger API surface as a result. The interface specification, known as .NET Standard 2, covers the intersection between .NET Core, the .NET Framework, and Xamarin. There is also an effort to standardize Extensible Application Markup Language (XAML) into the XAML standard, which will work across Universal Windows Platform (UWP) and Xamarin.Forms apps.

C# and .NET can be used on a huge range of diverse platforms and in a large number of different use cases, from server-side web applications to mobile apps and even games (using game engines such as Unity 3D). In this book, we'll focus on web application programming and, in particular, on general ways to make web apps perform well. This means that we will also cover client-side web browser scripting with JavaScript and the performance implications involved.

This book is not just about C# and ASP.NET. It takes a holistic approach to performance and aims to educate you about a wide range of relevant topics. We don't have the space to take a deep dive into everything, so the idea here is to help you discover some useful tools, technologies, and techniques.

In this chapter, we will go through the changes between version 1 and version 2 of both .NET Core and ASP.NET Core. We will also look at some new features of the C# language. There have been many useful additions and a plethora of performance improvements too.

In this chapter, we will cover the following topics:

- What's new in .NET Core 2.0
- What's new in ASP.NET Core 2.0
- Performance improvements
- .NET Standard 2.0
- New C# 6.0 features
- New C# 7.0 features
- JavaScript considerations

## What's new in Core 2

There are two main products in the Core family. The first is .NET Core, which is a low-level framework that provides basic libraries. It can be used to write console applications, and it is also the foundation for higher level application frameworks.

The second is ASP.NET Core, which is a framework for building web applications that run on a server and service clients (usually web browsers). This was originally the only workload for .NET Core until it grew in scope to handle a more diverse range of scenarios.

We'll cover the differences in the newer versions separately for each of these frameworks. The changes in .NET Core will also apply to ASP.NET Core, unless you are running it on top of the .NET Framework, version 4.

## What's new in .NET Core 2

The main focus of .NET Core 2 is the huge increase in scope. There are more than double the number of APIs included, and it supports .NET Standard 2 (covered later in this chapter). You can also reference .NET Framework assemblies with no recompilation required. This should just work as long as the assemblies only use APIs that have been implemented in .NET Core.

This means that more NuGet packages will work with .NET Core. Finding whether your favorite library was supported or not was always a challenge in the previous version. The author set up a repository listing package compatibility to help with this. You can find the ASP.NET Core Library and Framework Support (ANCLAFS) list at https://anclafs.com/ and https://github.com/jpsingleton/ANCLAFS. If you want to make a change, then please send a pull request. Hopefully, in future, all the packages will support Core, and this list will no longer be required.

There is now support in .NET Core for Visual Basic, and more Linux distributions. You can also perform live unit testing with Visual Studio 2017 (Enterprise Edition only), much like the old NCrunch extension. We'll talk more about tooling in Chapter 3, Setting Up Your Environment, where we will also cover containerization.

## Performance improvements

Some of the more interesting changes in .NET Core 2.0 are performance improvements over the original .NET Framework. There have been tweaks to the implementations of many framework data structures. Some of the classes and methods that have seen speedy improvements or memory reduction include:

- `List<T>`
- `Queue<T>`
- `SortedSet<T>`
- `ConcurrentQueue<T>`
- `Lazy<T>`
- `Enumerable.Concat()`
- `Enumerable.OrderBy()`
- `Enumerable.ToList()`
- `Enumerable.ToArray()`
- `DeflateStream`
