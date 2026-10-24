

The following code shows the use of `next_element`. While traversing along a DOM (normally from top to bottom), various elements can be found with `next_element`:

```
soup.find('p','story').next_element
# 'Once upon a time there were three little sisters; and
#  their names were\n '

soup.find('p','story').next_element.next_element
# <a class="sister" href="http://example.com/elsie"
#  id="link1">Elsie</a>
```

Let us understand the details of the preceding code:

+   - `find('p','story').next_element`: Searches for a `<p>` element for the story value in class, and points to the next element. Here, the next element after `<p class="story">` is the text itself, as seen in the preceding code output.
+   - `find('p','story').next_element.next_element`: Points to the `<a>` child element with the text `Elsie`.

`previous_element` is similar to `next_element`, but the direction of traversing is reversed.

The following code shows the use of `previous_element`, which can be extended or chained:

```
soup.find('b').previous_element
# <p class="title"><b>The Dormouse's story</b></p>

soup.find('li',{'data-id':'45732'}).previous_element
    .previous_element
# 'Alex Trevelyan, 006: Agent turned terrorist leader;
#  James\'... in "Goldeneye".'
```

Regarding the preceding code, let us understand a few details:

+   - `find('b').previous_element`: There’s only one `<b>` element in the HTML content inside `<p>`, so it returns the element at `previous_element` or `<p class="title">`
+   - `find('li',{'data-id':'45732'}).previous_element.previous_element`: With `<li data-id="45732">`, we find that the previous element is `<li data-id="97865">`, hence we get the text

The `next_element` and `previous_element` attributes can be chained, as seen in the preceding code.

We have explored a lot regarding searching and traversing so far in this section. The next section demonstrates performing iteration.

## Iteration

We have already come across the concept of iteration in previous sections. For example, find_all(), next_elements, and previous_elements result in one or more elements, where a loop can be used.

The following code iterates on the child elements of the <ul> elements, which are <li> using find_all(), and uses the name attribute to show the element name:

```
for li in soup.ul.find_all('li'):
    text = li.get_text().strip()
    print(f"{li.name} : {li.get('data-id')} :
        ({len(text)}) - {text}")
```

Similarly, the get_text() function returns the text string of an element, and the get('data-id') function returns the values of the data-id attribute. Printing all those return values with their text lengths, the output looks as follows:

```
li : 10784 : (53) - Jason Walters, 003: Found dead in "A
    View to a Kill".
li : 97865 : (82) - Alex Trevelyan, 006: ...; James' nemesis
    in "Goldeneye".
li : 45732 : (54) - James Bond, 007: The main man; shaken
    but not stirred.
```

Various alternate mechanisms exist to perform iteration and the extraction process.

In this and previous sections, we explored the main concepts related to BeautifulSoup. We will scrape content using BeautifulSoup in the upcoming section.

> **Important note**
> BeautifulSoup has various other methods related to the find activity, such as find_all_next, find_next, and find_previous_siblings, to deal with parent-child and traversing-related instances. There have been updates to the names and supporting arguments of methods in bs4. For the complete and updated documentation, please visit https://www.crummy.com/software/BeautifulSoup/bs4/doc/.

## Web scraping using Beautiful Soup

In this section, we will build and execute a web crawler using Beautiful Soup. To set things up, we have chosen to scrape quotes from http://quotes.toscrape.com. Specifically, we will be scraping from the page http://quotes.toscrape.com/tag/inspirational, as seen in Figure 5.3:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_140_0.png)

Figure 5.3: Category “inspirational”

The example we are dealing with is similar to *Example 3 – scraping quotes with author details* in Chapter 4. Only the links and compositions have changed, by carrying out a few additional logical steps. The code for the example can be found on GitHub: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter05/bs4_scraping.ipynb.

The following code declares the paginated link as url, and columns contains a column header for the CSV file to be generated:

```
```python
url = "http://quotes.toscrape.com/tag/inspirational/page/"
columns=['id','author','quote','tags','quote_length',
         'born_date','born_location','author_url']
```
```

Inside the paginated loop, a response from url has been collected and provided to BSoup to be parsed with the default parser:

```
```python
response = requests.get(url+str(page))
source = BSoup(response.content)
    if source.find('ul','pager').find('li','next'):
        txtNext = source.find('ul','pager')
                  .find('li','next').find('a').get_text()
```
```

txtNext holds the value for the next page.

As seen in the following code, the nextPage pagination variable is controlled based on the "Next" string if it exists for txtNext:

```
if txtNext and
    re.findall(r".*(Next).*",txtNext)[0]=="Next":
        nextPage=True
```

Multiple variables, such as quote, author, and tags, are created with the content available from iterating <div> with the quote class value:

```
for quotes in source.find_all('div','quote'):
    quote = quotes.find(attrs={'itemprop':'text'})
        .get_text().strip()
    author = quotes.find(attrs={'class':'author'})
        .get_text().strip()
    tags = quotes.find(attrs={'itemprop':'keywords'})
        .get('content').strip()
    authorUrl = quotes.find(href=re.compile(r"/author/"))
        .get('href')
```

Similarly to the quote-based information, a unique authorUrl string has been identified and its content is parsed to collect author-related information, such as bornDate and bornLocation:

```
if authorUrl and authorKey not in authorSet.keys():
    authorUrl = "http://quotes.toscrape.com"+authorUrl
    source_author = BSoup(requests.get(authorUrl).content)
    bornDate = source_author.find(attrs=
        {'class':'author-born-date'}).get_text().strip()
    bornLocation = source_author.find(attrs=
        {'class':'author-born-location'}).get_text()
        .replace('in', '').strip()
```

Finally, author-related information is collected in authorSet, and dataSet holds quote and author details:

```
authorSet[authorKey]={
    'name':author,
    'url':authorUrl,
    'date':bornDate,
    'location':bornLocation
}
dataSet.append([
    uid,
    author,
    quote,
```

```
tags.replace(',', '|'),
len(quote),
authorSet[authorKey]['date'],
authorSet[authorKey]['location'],
authorSet[authorKey]['url']
)
```

The collected set of information (author, quote, tags, and a few more) is finally written into JSON (quotes_author_inspirational.json) and CSV (quotes_inspirational.csv) files, as seen in Figure 5.4:

| id | author | quote | tags | quote_len | born_date | born_location | author_url |
|---:|---|---|---|---:|---|---|---|
| 1 | Albert Einstein | "There are only two w..." | inspirationa... | 131 | March 14, 1879 | Ulm, Germany | http://quotes.toscrape.com/author/Alb... |
| 2 | Marilyn Monroe | "Imperfection is beaut..." | be-yourself/... | 111 | June 01, 1926 | The United States | http://quotes.toscrape.com/author/Ma... |
| 3 | Thomas A. Edison | "I have not failed. I've ..." | edison|fallu... | 65 | February 11, 1847 | Milan, Ohio, The Un... | http://quotes.toscrape.com/author/Th... |
| 4 | Marilyn Monroe | "This life is what you ..." | friends|hear... | 1084 | June 01, 1926 | The United States | http://quotes.toscrape.com/author/Ma... |
| 5 | Elie Wiesel | "The opposite of love ..." | activism|ap... | 224 | September 30, 1928 | Sighet, Romania | http://quotes.toscrape.com/author/Eli... |
| 6 | J.K. Rowling | "To the well-organized..." | death|inspir... | 68 | July 31, 1965 | Yate, South Glouc... | http://quotes.toscrape.com/author/J-K... |
| 7 | George Eliot | "It is never too late to ..." | inspirational | 54 | November 22, 1819 | South Farm, Arbury... | http://quotes.toscrape.com/author/Ge... |
| 8 | C.S. Lewis | "You can never get a ..." | books|inspir... | 79 | November 29, 1898 | Belfast, Ireland | http://quotes.toscrape.com/author/C... |
| 9 | Martin Luther King Jr. | "Only in the darkness ..." | hope|inspir... | 45 | January 15, 1929 | Atlanta, Georgia, T... | http://quotes.toscrape.com/author/Ma... |
| 10 | Helen Keller | "When one door of ha..." | inspirational | 153 | June 27, 1880 | Tuscumbia, Alabam... | http://quotes.toscrape.com/author/Hel... |
| 11 | George Bernard S... | "Life isn't about findin..." | inspirationa... | 69 | July 26, 1856 | Dublin, Ireland | http://quotes.toscrape.com/author/Ge... |
| 12 | John Lennon | "You may say I'm a dr..." | beatles|con... | 117 | October 09, 1940 | Liverpool, England... | http://quotes.toscrape.com/author/Jo... |
| 13 | Dr. Seuss | "A person's a person, ..." | inspirational | 43 | March 02, 1904 | Springfield, MA, The ... | http://quotes.toscrape.com/author/Dr... |

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_142_0.png)

> **Important note**
> 
> BeautifulSoup also supports CSS selectors, and there is a short, small crawler available as an example in the GitHub repository: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter05/bs4_scraping_css_select.ipynb.

In this section, we have developed Beautiful Soup-based crawlers and extracted data into files. In the next section, we will learn about the Python scrapy library and develop a crawler using it.

## Web scraping using Scrapy

We have learned about, explored, and used different Python libraries for web scraping in the current and previous chapters. Scrapy is one of the few open source web crawling frameworks written in Python that allows dynamic adaptation, a project-based scope, and modular extensibility for web scraping tasks.

