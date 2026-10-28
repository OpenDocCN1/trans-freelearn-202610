## Preface

Facebook is big, by all meanings of the word. It's used by half a billion people—and countless businesses, bands, and public figures—for socializing and self-promotion. It's also a huge development platform, with tens of thousands of applications.

It's now common to see a Facebook "Like" button on blog posts, news articles, and many other websites. In the same way, Facebook integration is becoming more and more desirable for browser-based RIAs and games, with some, like FarmVille, even being based entirely around Facebook. That's where Flash comes in.

## What this book covers

*Chapter 1, Introduction,* gets you up to speed with Facebook and ready to learn to develop Flash applications that connect with the Facebook platform. You'll learn why it's worth putting more time into developing for Facebook than other social networks (and why it's likely to stay that way), and get yourself technically set up for coding.

*Chapter 2, Welcome to the Graph,* introduces you to Facebook's model for connecting all the information in its huge data stores—the **Graph API**. You'll discover how intuitive this model is, and will start to explore the publicly available data using AS3 through utility code, which you'll build from scratch.

*Chapter 3, Let Me In!,* breaks down Facebook's systems for security, permissions, and authentication. You'll learn how to access the private data of Facebook users (including their photos, biographical information, and lists of friends). You will also start using the official Adobe ActionScript 3 SDK for Facebook platform alongside your own utility code.

*Chapter 4, Digging Deeper into the Graph,* helps you understand the concepts of paging and filtering, so that you aren't restricted to using only the default dataset that Facebook presents you with. You'll find out how to obtain data from specified dates, and how to speed up your applications by retrieving information from multiple sources at once.

Chapter 5, Search Me, builds on the previous chapter by teaching you how to search for data based on criteria other than dates. You'll learn how to retrieve Wall Posts by specific users, pages with specific names, and places by specific geographical coordinates.

Chapter 6, Adding to the Graph, takes you beyond merely retrieving data and into publishing new data to Facebook. You'll find out how to create new Wall Posts (including rich posts including images and embedded hyperlinks); how to comment on other users' Wall Posts; how to create new events, notes, and albums; and how to upload photos from your hard drive.

Chapter 7, FQL Matters, takes a break from the Graph API to teach you how to learn a powerful search tool—Facebook Query Language. You'll trade the Graph API's intuitiveness and simplicity for FQL's depth and additional features, while also understanding the benefits that each approach offers over the other.

Chapter 8, Finishing Off, wraps up what you've learned throughout the book and gets you ready to release your application to the wild. You'll find out how to embed your application into the Facebook website itself; how to get it into the official Facebook Application Directory; and how to export it as a desktop or Android application, while still keeping its Facebook connectivity. Finally, you'll learn how to keep up-to-date with the ever-changing Facebook platform, and discover some useful resources for taking what you've learned even further.

Appendix, Pop Quiz Answers, contains answers to all the Pop Quizzes in the book

## What you need for this book

To develop and compile the example code in this book, you will need an AS3 compiler. Sample projects are provided for use with Flash Professional (CS3 and above), Flash Builder, and the free FlashDevelop IDE (with the Flex SDK); if you use a different workflow you will be able to convert these to fit your tools.

You'll also need previous experience with AS3 class-based coding and a Facebook account. The exact requirements here, along with what to do if you don't meet them, are detailed in Chapter 1.

## Who this book is for

If you are an AS3 developer who wants to create applications and games that integrate with Facebook—either on the Facebook website itself or off it, then this book is for you. Even if you have no previous experience with Facebook, databases, or server-side programming, you can follow this book.

## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text are shown as follows: "All we have to do is pass it an argument of type `graph.GraphObject`."

A block of code is set as follows:

```
for (var key:String in decodedJSON)
{
    graphObject[key] = decodedJSON[key];
}
```

When I wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
if (decodedJSON.data)
{
    //has a "data" property so we assume it is a Graph List
    var graphList:GraphList = new GraphList();
}
```

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "Compile and run your SWF, then expand the **Connections** box and click on **posts**".

Warnings or important notes appear in a box like this.

Tips and tricks appear like this.
