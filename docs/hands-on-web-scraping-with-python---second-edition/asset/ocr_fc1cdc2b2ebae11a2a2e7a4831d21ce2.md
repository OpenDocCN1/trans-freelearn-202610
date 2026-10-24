```
```python
nextPage=True
else:
    nextPage=False
```
```

While browsing through the pages, we did find the URL pattern, but there is no mention of how many total quotes are available. This restricts us to calculating values such as the final page number or the total number of pages to maintain the loop. To do this, we have used the nextPage Boolean variable, which is always True if the browsed page contains the text Next. If the browsed page does not have the text Next, then we assume that the browsed page is the final page and maintain the loop accordingly by updating nextPage to False. Maintaining the loop is always a priority and needs to be checked while browsing each page.

The following code iterates on the class attribute value quote and collects the required data with some basic data-cleaning activities:

```
```python
for quotes in source.find('.quote').items():
    quote = quotes.find('[itemprop="text"]').text().strip()
    author = quotes.find('[itemprop="author"]').text()
        .strip()
    tags = quotes.find('[itemprop="keywords"]')
        .attr('content').strip()
    authorUrl = quotes.find('a[href*="/author/"]')
        .attr('href')
```
```

In each iteration over quote, authorUrl is collected. During the preliminary study, we found that there are a few different quotes from the same author on the page.

As we have to browse each authorUrl encountered, some system resources are wasted when loading duplicate URLs, and even in the final output, duplicate values will be available. To deal with this situation, we must verify whether authorUrl has been accessed or not.

The following code inserts the formatted value of author (author name) or authorKey in authorSet:

```
```python
if authorUrl:
    authorKey = author.replace('.', '_').replace(' ', '_')
        .strip()

if authorUrl and authorKey not in authorSet.keys():
    authorUrl = "http://quotes.toscrape.com"+authorUrl
    source_author = pq(requests.get(authorUrl).content)
    bornDate = source_author.find('.author-born-date')
        .text()
    bornLocation = source_author
        .find('.author-born-location').text()
        .replace('in ','').strip()
```
```