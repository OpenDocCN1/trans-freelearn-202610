
That looks like a solid result. One thing you may notice is that there is no encapsulation character surrounding the text in the third column. This could prove to be a problem since we have used commas as a delimiter. What if we have commas in our third column? If this worries you, you can either add quotation marks around the third column, or you can use tabs to delimit the columns. To do this, change the first `outfile.write()` line to have \t (tab) as the join character instead of comma. You can also add some whitespace trimming to the message via `lstrip()` to remove any stray characters.

## The limitations of parsing HTML using regular expressions

This regular expressions method seems pretty straightforward at first, but it has some limitations. First, for new data cleaners, regular expressions can be kind of a pain in the neck to design and perfect. You should definitely plan on spending a lot of time debugging and writing yourself copious documentation. To assist in the generation of regular expressions, I would definitely recommend using a regular expression tester, such as http://Pythex.org, or just use your favorite search engine to find one. Make sure you specify that you want a Python regex tester if that is the language you are using.

Next, you should know in advance that regular expressions are completely dependent on the structure of the web page staying the same in the future. So, if you plan to collect data from a website on a schedule, the regular expressions you write today may not work tomorrow. They will only work if the layout of the page does not change. A single space added between two tags will cause the entire regex to fail and will be extremely difficult to troubleshoot. Keep in mind too that most of the time you have little or no control over website changes, since it is usually not your own website that you are collecting data from!

Finally, there are many, many cases where it is next-to-impossible to accurately write a regular expression to match a given HTML construct. Regex is powerful but not perfect or infallible. For an amusing take on this issue, I refer you to the famous Stack Overflow answer that has been upvoted over 4000 times: http://stackoverflow.com/questions/1732348/. In this answer, the author humorously expresses the frustration of so many programmers who try over and over to explain why regex is not a perfect solution to parsing irregular and ever-changing HTML found in web pages.

## Method two – Python and BeautifulSoup

Since regular expressions have some limitations, we will definitely need more tools in our data cleaning toolkit. Here, we describe how to extract data from HTML pages using a parse tree-based Python library called BeautifulSoup.

### Step one – find and save a file for experimenting

For this step, we will use the same file as we did for Method 1: the file from the Django IRC channel. We will search for the same three items. Doing this will make it easy to compare the two methods to each other.

### Step two – install BeautifulSoup

BeautifulSoup is currently in version 4. This version will work for both Python 2.7 and Python 3.

> If you are using the Enthought Canopy Python environment, simply run `pip install beautifulsoup4` in the Canopy Terminal.

### Step three – write a Python program to extract the data

The three items we are interested in are found within the set of `li` tags, specifically those with `class="le"`. There are not any other `li` tags in this particular file, but let's be specific just in case. Here are the items we want and where to find them in the parse tree:

- We can extract the username from the `li` tag underneath the `rel` attribute.
- We can get the `linenum` value from the `name` attribute in the `a` tag. The `a` tag is also the first item in the contents of the `li` tag.

> Remember that arrays are zero-based so we need to ask for item 0.

In BeautifulSoup, the **contents** of a tag are the items underneath that tag in the parse tree. Some other packages will call them **child** items.

- We can extract the message as the fourth content item in the li tag (referenced as array item [3]). We also notice that there is a leading space at the front of every message, so we want to strip that off before saving the data.

Here is the Python code that corresponds to the outline of what we want from the parse tree:

```
from bs4 import BeautifulSoup
import io

infile = io.open('django13-sept-2014.html', 'r', encoding='utf8')
outfile = io.open('django13-sept-2014.csv', 'a+', encoding='utf8')
soup = BeautifulSoup(infile)

row = []
allLines = soup.findAll("li","le")
for line in allLines:
    username = line['rel']
    linenum = line.contents[0]['name']
    message = line.contents[3].lstrip()
    row.append(linenum)
    row.append(username)
    row.append(message)
    outfile.write(', '.join(row))
    outfile.write(u'\n')
    row = []
infile.close()
```

### Step four – view the file and make sure it is clean

When we open the new CSV file in a text editor, we can see that the first few lines now look identical to the ones from Method 1:

```
1574618, petisnnake, i didn't know that
1574619, dshap, FunkyBob: ahh, hmm, i wonder if there's a way to do
it in my QuerySet subclass so i'm not creating a new manager subclass
*only* for get_queryset to do the initial filtering
1574620, petisnnake, haven't used Django since 1.5
```

Just like with the regular expressions method, if you are worried about commas embedded within the last column, you can encapsulate the column text in quotes or just use tabs to delimit the columns instead.

## Method three – Chrome Scraper

If you really do not want to write a program to parse out data, there are several browser-based tools that use a tree structure to allow you to identify and extract the data you are interested in. I think the easiest to use with minimum work is a Chrome extension called Scraper, created by a developer called mnmldave (real name: Dave Heaton).

### Step one – install the Scraper Chrome extension

Download the Chrome browser if you do not already have that running. Make sure that you get the correct Scraper extension; there are several extensions that have very similar names. I recommend using the developer's own GitHub site for this product, available at http://mnmldave.github.io/scraper/. This way you will be able to have the correct scraper tool, rather than trying to search using the Chrome store. From the http://mnmldave.github.io/scraper site, click the link to install the extension from the Google Store, and restart your browser.

### Step two – collect data from the website

Point your browser to the same web URL we have been using to get the data for the other two web data extraction experiments, one of the Django IRC logs. I have been using the September 13, 2014 log for the examples and screenshots here, so I will go to http://django-irc-logs.com/2014/sep/13/.

In my browser, at the time of writing, this page looks like this:

![](img/9aa003afabe05fdb07da46ad71325de6_139_0.png)

We have three items from this IRC log that we are interested in:

- The line number (we know from our previous two experiments that this is part of the link underneath the # sign)
- The username (located between the < and > symbols)
- The actual line message

Scraper allows us to highlight each of these three items in turn and export the values to a Google Spreadsheet, where we can then reassemble them into a single sheet and export as CSV (or do whatever else we want with them). Here is how to do it:

1. Use your mouse to highlight the item you want to scrape.
2. Right-click and choose Scrape similar... from the menu. In the following example, I have selected the username petisnnake as the one that I want Scraper to use:

![](img/9aa003afabe05fdb07da46ad71325de6_140_0.png)

3. After selecting Scrape similar, the tool will show you a new window with a collection of all the similar items from the page. The following screenshot shows the entire list of usernames that Scraper found:
