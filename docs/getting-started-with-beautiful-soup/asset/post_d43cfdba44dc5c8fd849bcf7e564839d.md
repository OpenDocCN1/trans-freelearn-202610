
We can pass different formatters as parameters to any of the output methods, such as `prettify()`, `encode()`, and `decode()`.

## The minimal formatter

In this formatting mode, strings will be processed enough to generate a valid HTML code. This is the default formatter and the HTML entities that are escaped are `&amp;`, `&gt;` and `&lt;`. The output will be similar to the one shown in the previous screenshot.

## The html formatter

In this formatting mode, Beautiful Soup will convert the Unicode characters to HTML entities.

```
print(soup.prettify(formatter="html"))
```

The following screenshot shows the difference between using the `minimal` and `html` formatters:

![](img/c437561ffd11fadb571896e0a4ca4d18_108_0.png)

From the previous screenshot we can identify that the formatter `html` changes whatever Unicode characters possible back to HTML entities.

## The None formatter

In this case, Beautiful Soup will not modify the strings. This can lead to the generation of an invalid HTML code.

The `print(soup.prettify(formatter=None))` code line produces an output similar to the following screenshot:

![](img/c437561ffd11fadb571896e0a4ca4d18_109_0.png)

## The function formatter

We can specify a Python function and what to do with each string and attribute value.

```
def remove_chara(markup):
    return markup.replace("a", "")
```

Here we define a function to remove the character a from the strings passed.

```
print(soup.prettify(formatter=remove_chara))
```

We use this function to strip out the a characters from the output. We should note that this will not escape the three special characters &amp;, &gt;, and &lt; in the output as shown in the following screenshot:

![](img/c437561ffd11fadb571896e0a4ca4d18_109_1.png)

## Using get_text()

Getting just text from websites is a common task. Beautiful Soup provides the method get_text() for this purpose.

If we want to get only the text of a Beautiful Soup or a Tag object, we can use the get_text() method. For example:

```
html_markup = """<p class="ecopyramid">
<ul id="producers">
    <li class="producerlist">
        <div class="name">plants</div>
        <div class="number">100000</div>
    </li>
    <li class="producerlist">
        <div class="name">algae</div>
        <div class="number">100000</div>
    </li>
</ul>"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.get_text())

#output
plants
100000

algae
100000
```

The get_text() method returns the text inside the Beautiful Soup or Tag object as a single Unicode string. But get_text() has issues when dealing with web pages. Web pages often have JavaScript code, and the get_text() method returns the JavaScript code as well. For example, in Chapter 3, Search Using Beautiful Soup, we saw the example of scraping book details from packtpub.com.

```
import urllib2
from bs4 import BeautifulSoup
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page,"lxml")
```

We can print the text inside the page using get_text(); this is shown in the following code snippet:

```
print(soup_packtpage.get_text())
```

With the previous code line, we will also get JavaScript code in the output as shown:

```
$(window).load(function() {
  $("img[data-original]").addClass("lazy");
  $("img.lazy").lazyload();
  setTimeout(function() {
    addthis_config = Drupal.settings.addthis.config_default;
    addthis_share = Drupal.settings.addthis.share_default;
    if (typeof pageTracker != "undefined")
    {addthis_config.data_ga_tracker = pageTracker;}
    var at = document.createElement("script"); at.type =
      "text/javascript"; at.async = true;
    at.src = "//dgdsbygo8mp3h.cloudfront.net/sites/default/
      files/addthis/addthis_widget.js";
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(at, s);
  }, 5);
});
```

Removing the JavaScript and printing only the text within a document can be achieved using the following code line:

```
[x.extract() for x in soup_packtpage.find_all('script')]
```

The previous code line will remove all the script elements from the document. After this, the print(soup_packtpage.get_text()) code line will print only the text stored within the page.

## Quick reference

You can take a look at the following references to get an overview:

- Formatted printing: The following method gives the formatted output:
  - soup.prettify(): This method is used to output with indentation and formatting
- Unformatted printing: These methods give output without any format or indentation:
  - str(soup)
  - soup.encode()
  - soup.decode()
- Formatters: These methods have the ability to control the behavior of the output:
  - `soup.prettify(formatter="minimal")`
  - `soup.prettify(formatter="html")`
  - `soup.prettify(formatter="None")`
  - `soup.prettify(formatter=already_defined_func)`
- get_text(): This method gives you all the content of an object or a tag:
  - `soup.get_text()`: This method is used to get all the text content in BeautifulSoup or Tag objects

## Summary

This chapter dealt with the different output methods in Beautiful Soup. We saw the formatted and unformatted printing in the BeautifulSoup object and also the different output formatters to control the output formatting. We also saw a method of getting only the text from a web page.

In the next chapter, we will create a web scraper using searching, navigation, and other techniques we have studied in this book so far.

## 8

## Creating a Web Scraper

In this chapter, we will create a website scraper using the different searching and navigation techniques we studied in the previous chapters. The scraper will visit three websites to find the selling price of books based on the ISBN. We will first find out the book list and selling price from packtpub.com. We will also find the ISBN of the books from packtpub.com and search other websites such as www.amazon.com and www.barnesandnoble.com based on this ISBN. By doing this, we will automate the process of finding the selling price of the books on three websites and will also get a hands-on experience in implementing scrapers for these websites. Since the website structure may change later, the code examples and images used in this chapter may also become invalid. So, it is better to take the examples as a reference and change the code accordingly. It is good to visit these websites for a better understanding.

## Getting book details from PacktPub.com

Getting book details from www.packtpub.com is the first step in the creation of the scraper. We need to find the following details from PacktPub.com:

- Book title name
- Selling price
- ISBN

We have seen how to scrape the book title and the selling price from packtpub.com in Chapter 3, Search Using Beautiful Soup. The example we discussed in that chapter considered only the first page and didn't include the other pages that also had the list of books. So in the next topic, we will find different pages containing a list of books.

## Finding pages with a list of books

The page at www.packtpub.com/books has the next and previous navigation links to go back and forth between the pages containing a list of books, as shown in the following screenshot:

![](img/c437561ffd11fadb571896e0a4ca4d18_114_0.png)

So, we need to find out a method for getting multiple pages that contain the list of books. Logically, it seems to look at the page being pointed at by the next element in the current page. Taking a look at the next element, for page 49, using the Google Chrome developer tools, we can see that it actually points to the next page link, that is, /books?page=49. If we observe different pages using the Google Chrome developer tools, we can see that the link to the next page's actually has a pattern of /books?page=n for the n+1 page, that is, n=49 for the 50th page, as shown in the following screenshot:

![](img/c437561ffd11fadb571896e0a4ca4d18_114_1.png)

From the preceding screenshot, we can further understand that the next element is within the <li> tag with class="pager-next last". Inside the <li> tag, there is an <a> tag that holds the link to the next page. In this case, the corresponding value is /books?page=49, which points to the 50th page. We have to add www.packtpub.com to this value to make a valid URL, as www.packtpub.com/books?page=49.
