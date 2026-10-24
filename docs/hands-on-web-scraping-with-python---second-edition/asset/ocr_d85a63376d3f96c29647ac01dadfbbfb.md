With the required data being collected, driver.back() loads the listing page again (it provides immediate history, similar to the back button available in the browser window) and iterates over the next <li> list element available. Selenium WebDriver supports browser history related controls with the back() and forward() methods:

```
# Go back to history (From individual book detail page to listings page)
driver.back()
```

As there are multiple pages in the Childrens category, the code iterates for the pages with a pagination(True) value and searches for the link with the next text. Python exception handling is used in this example to verify whether the element exists or not with the next text:

+   - If the element exists, the page count is incremented and another listing page is loaded
+   - If the element does not exist, it is caught with the NoSuchElementException exception, which disables the page loop by updating the pagination Boolean value to False:

```
try:
    # Check for Pagination with text 'next' in the
    Listing page
    driver.find_element(By.LINK_TEXT,'next').click()
    page = page+1
except NoSuchElementException:
    pagination = False
    print(f "Further Pagination is not possible,
        currently at {page}")
```

> **Important note**
> For more information on exception handling, please visit the following links: https://docs.python.org/3/tutorial/errors.html and https://selenium-python.readthedocs.io/api.html.

Upon the completion of loops for pages and listings, the final data collected in dataSet is given planned column names listed in a list, ['Upc', 'Title', 'Price', 'Rating', 'Stock', 'Stock_Qty', 'Url', 'Image'], and used to create a CSV and JSON file.