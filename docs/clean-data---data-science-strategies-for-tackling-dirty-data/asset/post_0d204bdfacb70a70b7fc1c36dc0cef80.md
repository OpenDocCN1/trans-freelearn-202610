
![](img/9aa003afabe05fdb07da46ad71325de6_141_0.png)

Scraper finds all the similar items based on one sample username.

- 4. At the bottom of the window, there is a button labeled **Export to Google Docs...** Note that depending on your settings, you may have to click to agree to allow access to Google Docs from within Scraper.

## Step three – final cleaning on the data columns

Once we have all the data elements extracted from the page and housed in separate Google Docs, we will need to combine them into one file and do some final cleaning. Here is an example of what the line numbers look like once they have been extracted, but before we have cleaned them:

|  | A | B |
|---|---|---|
| 1 | Link | URL |
| 2 | # | #1574618 |
| 3 | # | #1574619 |
| 4 | # | #1574620 |
| 5 | # | #1574621 |
| 6 | # | #1574622 |

We are not interested in column A at all, nor are we interested in the leading # symbol. The username and line message data is similar — we want most of it, but we would like to remove some symbols and combine everything into a single Google Spreadsheet.

Using our find-and-replace techniques from Chapter 3, Workhorses of Clean Data – Spreadsheets and Text Editors (namely removing the #, <, and > symbols and pasting the rows into a single sheet), we end up with a single clean dataset that looks like this:

|  | A | B | C |
|---|---|---|---|
| 1 | 1574618 | petisnake | i didn't know that |
| 2 | 1574619 | dshap | FunkyBob: ahh, hmm, i wonder if there's a way to do it in my QuerySet |
| 3 | 1574620 | petisnake | haven't used Django since 1.5 |
| 4 | 1574621 | FunkyBob | petisnake: really? it's probably the biggest single new feature of 1.7 |
| 5 | 1574622 | FunkyBob | renlo: btw -- you can do a smarter query than that |

Scraper is a nice tool for extracting small amounts of data from web pages. It has a handy Google Spreadsheets interface, and can be a quick solution if you do not feel like writing a program to do this work. In the next section, we will tackle a larger project. It may be complicated enough that we will have to implement a few of the concepts from this chapter into a single solution.

## Example project – Extracting data from e-mail and web forums

The Django IRC logs project was pretty simple. It was designed to show you the differences between three solid techniques that are commonly used to extract clean data from within HTML pages. The data we extracted included the line number, the username, and the IRC chat message, all of which were easy to find and required almost no additional cleaning. In this new example project, we will consider a case that is conceptually similar, but that will require us to extend the idea of data extraction beyond HTML to two other types of semi-structured text found on the Web: e-mail messages hosted on the Web and web-based discussion forums.

## The background of the project

I was recently working on a research study about how social media can be used to provide software technical support. Specifically, I was trying to discover whether certain types of software development organizations that make APIs and frameworks should move their technical support for developers to Stack Overflow or whether they should continue to use older media, such as e-mail and web forums. To complete this study, I compared (among other things) how long it took developers to get an answer to their API question via Stack Overflow versus how long it took on older social media such as web forums and e-mail groups.

In this project, we will work on a small piece of this question. We will download two types of raw data representing the older social media: HTML files from a web forum and e-mail messages from Google Groups. We will write Python code to extract the dates and time of the messages sent to these two support forums. We will then figure out which messages were sent in reply to the others and calculate some basic summary statistics about how long it took each message to get a reply.

![](img/9aa003afabe05fdb07da46ad71325de6_143_0.png)

> If you are wondering why we aren't extracting data for the Stack Overflow portion of the question in this example project, just wait until Chapter 9, Stack Overflow Project. That entire chapter is devoted to creating and cleaning a Stack Overflow database.

This project will be divided into two parts. In Part one, we will extract data from the e-mail archive from a project hosted on Google Groups, and in Part two, we will extract our data from the HTML files of a different project.

## Part one – cleaning data from Google Groups e-mail

Many software companies have traditionally used e-mail mailing lists or hybrid e-mail-web forums to provide technical support for their products. Google Groups is a popular choice for this service. Users can either send e-mails to the group, or they can read and search the messages in a web browser. However, some companies have moved away from providing technical support to developers via Google Groups (including Google products themselves), and are instead using Stack Overflow. The database product Google BigQuery is one such group that now uses Stack Overflow.

## Step one – collect the Google Groups messages

To study the response times for questions on the BigQuery Google Group, I first created a list of the URLs for all the postings in that group. You can find my complete list of URLs on my GitHub site: https://github.com/megansquire/stackpaper2015/blob/master/BigQueryGGurls.txt.

Once we have a list of target URLs, we can write a Python program to download all the e-mails residing in those URLs, and save them to disk. In the following program, my list of URLs has been saved as the file called GGurls.txt. The time library is included, so we can take a short sleep() method in between requests to the Google Groups server:

```python
import urllib2
import time

with open('GGurls.txt', 'r') as f:
    urls = []
    for url in f:
        urls.append(url.strip())

currentFileNum = 1
for url in urls:
    print("Downloading: {0} Number: {1}".format(url, currentFileNum))
    time.sleep(2)
    htmlFile = urllib2.urlopen(url)
    urlFile = open("msg%d.txt" %currentFileNum,'wb')
    urlFile.write(htmlFile.read())
    urlFile.close()
    currentFileNum = currentFileNum +1
```

This program results in 667 files being written to disk.

## Step two – extract data from the Google Groups messages

We now have 667 e-mail messages in separate files. Our task is to write a program to read these in one at a time and use one of the techniques from this chapter to extract the pieces of information we need. If we peek inside one of the e-mail messages, we see lots of **headers**, which store information about the e-mail, or its **metadata**. We can quickly see the three headers that identify the metadata elements that we need:

```
In-Reply-To: <ab71b72a-ef9b-4484-b0cc-a72ecb2a3b85@r9g2000yqd.googlegroups.com>
Date: Mon, 30 Apr 2012 10:33:18 -0700
Message-ID: <CA+qSDkQ4JB+Cn7HNjmtLOqqkbJnyBu=Z1Ocs5-dTe5cN9UEPyA@mail.gmail.com>
```

All messages have Message-ID and Date, but the In-Reply-To header will only appear in a message that is a reply to another message. An In-Reply-To value must be the Message-ID value of another message.

The following code shows a regular expression-based solution to extract the Date, Message-ID, and In-Reply-To (if available) values and to create some lists of original and reply messages. Then, the code attempts to calculate the time differences between a message and its replies:

```python
import os
import re
import email.utils
import time
import datetime
import numpy

originals = {}
replies = {}
timelist = []

for filename in os.listdir(os.getcwd()):
    if filename.endswith(".txt"):
        f=open(filename, 'r')
        i=''
        m=''
        d=''
        for line in f:
            irt = re.search('(In\-Reply\-To: <)(.+?)@', line)
            mid = re.search('(Message\-ID: <)(.+?)@', line)
```
