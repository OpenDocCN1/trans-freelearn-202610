
Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: `SRTM.py` on GitHub is an example.

A block of code is set as follows:

```
IF SUM([Amount of Precipitation (inches)]) >= 0.2 THEN
"Yes"
ELSE
"No"
END
```

Any command-line input or output is written as follows:

```
hdfs dfs -put lots_o_data.csv /user/hadoop/datafolder/lots_o_data.csv
```

New terms and important words are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "In the **Parameters** section on the same page."

![](img/9f2a55db1236f995dcd9392acbc82910_21_0.png)

Warnings or important notes appear like this

![](img/9f2a55db1236f995dcd9392acbc82910_21_1.png)

Tips and tricks appear like this.

## Readers feedback

Feedback from our readers is always welcome. Let us know what you think about this book-what you liked or disliked. Reader feedback is important for us as it helps us develop titles that you will really get the most out of.

To send us general feedback, simply email `feedback@packtpub.com`, and mention the book's title in the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide at `www.packtpub.com/authors`.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

### Downloading the example code

You can download the example code files for this book from your account at `http://www.packtpub.com`. If you purchased this book elsewhere, you can visit `http://www.packtpub.com/support` and register to have the files emailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register to our website using your email address and password.
- 2. Hover the mouse pointer on the **SUPPORT** tab at the top.
- 3. Click on **Code Downloads & Errata**.
- 4. Enter the name of the book in the **Search** box.
- 5. Select the book for which you're looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this book from.
- 7. Click on **Code Download**.

You can also download the code files by clicking on the **Code Files** button on the book's webpage at the Packt Publishing website. This page can be accessed by entering the book's name in the **Search** box. Please note that you need to be logged in to your Packt account.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the book is also hosted on GitHub at `https://github.com/prachiss/Analytics-for-the-Internet-of-Things-IoT`. We also have other code bundles from our rich catalog of books and videos available at `https://github.com/PacktPublishing/`. Check them out!

## 1

## Defining IoT Analytics and Challenges

In this chapter, we will discuss some concepts and challenges associated with analytics on Internet of Things (IoT) data. We will cover the following topics:

- The analytics maturity model
- Defining the IoT
- What is different about IoT data?
  - Data volume
  - Problems with time
  - Problems with space
  - Data quality
  - Analytics challenges
- Concerns with finding business value

### The situation

The tense white-yellow of the fluorescent ceiling lights press down on you while you sit in your cubicle and stare at the monitors on your desk. You sense it is now night outside but can't see over the fabric walls to know for sure. You stare at the long list of filenames on one screen and the plain text rows of opaque sensor data on the other screen.

Your boss had just left to angrily brood somewhere in the office, and you are not sure where. He had been glowering over your shoulder.

"We spent $20 million in telecommunication and consulting fees last year just to get this data! The hardware costs $20 per unit. We've been getting data, and it has been piling up costing us $10,000 a month. There are 20 TB of files - that's big data, isn't it? And we can't seem to do anything with it?" he had said.

"This is ridiculous!", he continued, "It was supposed to generate $100 million in new revenue. Where is our first dollar? Why can't you do anything with it? I have five consultants a week calling me to tell me they can handle it- they'll even automate it. Maybe we should just pick one and hope they aren't selling us snake oil."

You know he does not really blame you. You were a whiz with Excel and knew how to query databases. A lot of analytics requests went to you. When the CEO decided the company needed a big data guy, they hired a VP out of Silicon Valley. But the new VP ended up taking a position with a different Silicon Valley company the day before he was supposed to start at your company.

You were hastily moved into the new analytics group. A group of one - you. It was to be a temporary shift until another VP was found. That was six months ago. The company is freezing funds for outside training and revenues are looking tight. So, no training for you.

Although many know the terms, no one in the company actually understands what Hadoop is or how to even start using this thing called machine learning. But others more and more seem to expect you to not only know it but already be doing it.

Executives have been reading articles in HBR and Forbes about the huge potential of the IoT combined with Artificial Intelligence or AI. They feel like the company will be left behind, and soon, if it does not have its own IoT big data solution incorporating AI. Your boss is feeling the pressure. Executives have several ideas for him where AI can be used. They seem to think that getting the idea is the hard part, implementation should be easy. Your boss is worried about his job and it rolls downhill to you.

Your screen on the left looks like this:

![](img/9f2a55db1236f995dcd9392acbc82910_26_0.png)

The list goes on and on for several pages. You have been able to combine several files and do some pivot tables and charting in Excel. But it takes a lot of your time, and you can only realistically handle a month or two worth of data. The questions are coming in faster than your ability to answer them. You and your boss have been talking about bringing in temps to do the work—they don't really need to understand it, just follow the steps that you outline for them.

Your screen on the right looks like this:

![](img/9f2a55db1236f995dcd9392acbc82910_27_0.png)

Your IT department has been consolidating lots of little files into several very large ones. The filesystem was being overloaded by the number of files, so the solution was to consolidate. Unfortunately, for you, many files are now too large to open in Excel, which limits what you can do with them. You end up doing more analytics on recent data simply because it is much easier (the files are still small).

Looking at the data rows, it is not obvious what you can do with it beyond sums and averages. The files are too big to do a VLOOKUP in Excel against something like your production records - which is stored in files often too big to even open in Excel.

At this point, you can't begin to think how you would apply Machine Learning to this data. You are not quite sure what it even means. You know the data is difficult to manipulate for anything beyond recent datasets. Surely, long periods of time would be needed to extract value out of it.

You hear a cough from behind you. Your boss is back.

He says quietly and stiffly, "I'm sorry. We're going to have to hire a consultant to take this over. I know how hard you've been working. You've done some amazing things considering the limitations, and nobody appreciates that enough. But I have to show results. It will probably take a month or two to fully bring someone on board. In the meantime, just keep at it—maybe we can make a breakthrough before then."

Your heart sinks. You are convinced there is huge value in the connected device data. You feel like you could make a career out of IoT analytics if you could just figure out how to get there. But you are not a quitter.

You decide you will not go down without a fight, you will find a way.

## Defining IoT analytics

In order to understand IoT analytics, it is helpful to separate it out and define both analytics and the IoT. This will help frame the discussion for the rest of the book.

### Defining analytics

If you ask a hundred people to define analytics, you are likely to get a hundred different answers. Each person tends to have his or her own definition in mind that can range from static reports to advanced deep learning expert systems. All tend to call efforts in the wide ranging territory analytics without much further explanation.

We will take a fairly broad definition in this book as we are covering quite a bit of territory. In their best selling book *Competing on Analytics*, Tom Davenport and Jeanne Harris created a scale, which they called **Analytics Maturity**. Companies progress to higher levels in the scale as their use of analytics matures, and they begin to compete with other companies by leveraging it.
