
## Preface

Social networking has become a driving force on the Internet. Many people are part of at least one social network, while more often people are members of many different communities. For this reason many business people are trying to capitalize on this movement and are in a rush to put up their own social network. As the growth of social networks continues, we have started to see more and more niche communities popping up all over in favor of the larger, all-encompassing networks in an attempt to capture a sliver of the market.

In this book, we will discuss the many aspects and features of what makes up the majority of today's social networks or online communities. Not only will we discuss the features, their purpose, and how to go about building them, but we will also take a look at the construction of these features from a large scale enterprise perspective. The goal is to discuss the creation of a community in a scalable fashion.

## What this book covers

- *Chapter 1, Social Networking* gives you an overall structure of this book, that is, what a reader can expect from this book.
- *Chapter 2, An Enterprise Approach to our Community Framework* helps you create an enterprise framework to handle the needs of most web applications. It discusses design patterns, best practices, and certain tools to make things easier. It also covers error handling and logging.
- *Chapter 3, User Accounts* covers registration and account creation process by means of an email verification system and a permission system to ensure security. It also touches upon password encryption/decryption techniques.
- *Chapter 13, Scaling* discusses some concepts to help you support a large number of users on your social network. It starts by looking at some key concepts of tiered architecture and web farming. It also discusses ways to create and search indexed data, methods to optimize data retrieval and content creation, and some mail queuing concepts.
- *Appendix* covers a discussion on the Microsoft ASP.NET MVP and MVC patterns and explains why we continued to use the MVP pattern for this book.

## What you need for this book

This book describes how to build a Social Network using Microsoft ASP.NET 4, Microsoft C# 4, and Microsoft SQL Server 2008. To use this book effectively, you will need access to Microsoft Visual Studio 2010 and Microsoft SQL Server 2008. Most of this book can be used with the various Express editions of Microsoft Visual Studio 2010 and SQL Express, but you will find that having the Professional edition of Microsoft Visual Studio 2010 will make your work flow more efficient. As we are not just discussing Microsoft ASP.NET, but are instead more interested in what is needed for the features of social networking, there may be times when we turn to an open source solution. All the examples in this book will clearly point out where to get the needed software, and how to configure that software as we work through our examples.

## Who is this book for

This book is written for Microsoft ASP.NET and C# developers who want to build an enterprise-grade Social Network, either for their own business purposes or as a contract job for another company. The book assumes you have a prior experience of developing web applications using Microsoft ASP.NET 4, Microsoft C# 4, Microsoft SQL Server 2008, and Microsoft Visual Studio 2010; it focuses on topics that will be of interest to existing developers—not on providing step-by-step examples for each detail.

## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text are shown as follows: "Create a new class in the `Fisheroo/BusinessLogic` directory called `ProfileService`."

A block of code will be set as follows:

```
if (profile != null && profile.ProfileID > 0)
{
    attributes = _profileAttributeService.
        GetProfileAttributesByProfileID
        (profile.ProfileID);
    levelOfExperienceType =
        _levelOfExperienceTypeRepository.
        GetLevelOfExperienceTypeByID
        (profile.LevelOfExperienceTypeID);

    profile.Attributes = attributes;
    profile.LevelOfExperienceType = levelOfExperienceType;
}
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items will be made bold:

```
Account account = _accountService.GetAccountByID(AccountID);
Profile profile = _profileService.LoadProfileByAccountID(AccountID);
if(profile != null)
{
    account.Profile = profile;
}
```

New terms and important words are introduced in a bold-type font. Words that you see on the screen, in menus or dialog boxes for example, appear in our text like this:

> "Click the Test Connection button to see if your settings are acceptable."

![](img/15a62e134135dde036b0ac3cbb01dab6_28_0.png)

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book, what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply drop an email to feedback@packtpub.com, making sure to mention the book title in the subject of your message.

If there is a book that you need and would like to see us publish, please send us a note in the **SUGGEST A TITLE** form on www.packtpub.com or email suggest@packtpub.com.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on www.packtpub.com/authors.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

### Downloading the example code for the book

You can download the example code files for all Packt books you have purchased from your account at **http://www.PacktPub.com**. If you have purchased this book elsewhere, you can visit **http://www.PacktPub.com/support** and register to have the files e-mailed directly to you.

The downloadable files contain instructions on how to use them.

### Errata

Although we have taken every care to ensure the accuracy of our contents, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in text or code—we would be grateful if you would report this to us. By doing this you can save other readers from frustration, and help to improve subsequent versions of this book. If you find any errata, report them by visiting **http://www.packtpub.com/support**, selecting your book, clicking on the **errata submission form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata are added to the list of existing errata. The existing errata can be viewed by selecting your title from **http://www.packtpub.com/support**.

### Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide the location address or website name immediately so we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

### Questions

You can contact us at questions@packtpub.com if you are having a problem with some aspect of the book, and we will do our best to address it.

## 1 Social Networking

Given the boom in the Internet and mobile devices industry, people across the world are more connected than ever before. Social networking is the in thing. This book is all about how to build a social networking site or a community site. In this book, we will take a look at a few well-known social networks and some not-so-well-known networks to get an idea of what features are popular. Then we will discuss the community that we will be building in this book. Once we have an idea of what others are doing and what our community will look like, we will dive right in to start building our own demo community.

## What makes this topic so important

**Social networking** is all about developing connections or ties between friends and associates. While people have always networked with one another, the Internet has allowed us to do this in a global manner. Some great examples of popular social networks are Digg, LinkedIn, Facebook, and Twitter. Most people have heard of these services and many use them on a daily basis. These communities are able to generate income from advertising and additional paid services.

## Communities Galore

`Digg.com` is an aggregator of information from other sites. They allow people to post links to interesting videos, blogs, news feeds, and other forms of media and content. This posting is then pushed to the top of their site based on how many others on the site also enjoy that post. Eventually, the posting will fall below the fold and fade into oblivion. The key with their site is that you are not actually viewing the body of the content on their site. This service is essentially a dynamic link farm times ten.
