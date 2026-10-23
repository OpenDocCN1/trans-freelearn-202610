The page generated after the search in Amazon will have a URL structure as follows:

```
http://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=1783289554
```

If we search based on another ISBN, that is, http://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=1847195164, we will see that it gives us back the details based on the 1847195164 ISBN.

From this, we can conclude that if we substitute field-keywords of the URL with the corresponding ISBN, we will be getting the details for that ISBN.

From the http://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=1783289554 page, we have to find the selling price for the book. We can follow the same method to use the Google Chrome developer tools to see which tag holds the selling price.

![](bbox=[0.212, 0.406, 0.788, 0.649])

From the preceding screenshot, we can see that the price is stored inside the div tag with the newPrice class. We can find the selling price from Amazon using the following code:

```
def get_sellingprice_amazon(isbn):
    url_foramazon = "http://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords="
    url_for_isbn_inamazon = url_foramazon+isbn
    page = urllib2.urlopen(url_for_isbn_inamazon)
    soup_amazon = BeautifulSoup(page, "lxml")
```