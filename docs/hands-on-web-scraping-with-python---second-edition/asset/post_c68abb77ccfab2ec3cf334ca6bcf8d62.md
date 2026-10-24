
With the required data being collected, driver.back() loads the listing page again (it provides immediate history, similar to the back button available in the browser window) and iterates over the next <li> list element available. Selenium WebDriver supports browser history related controls with the back() and forward() methods:

```
# Go back to history (From individual book detail page to listings page)
driver.back()
```

As there are multiple pages in the Childrens category, the code iterates for the pages with a pagination(True) value and searches for the link with the next text. Python exception handling is used in this example to verify whether the element exists or not with the next text:

- If the element exists, the page count is incremented and another listing page is loaded
- If the element does not exist, it is caught with the NoSuchElementException exception, which disables the page loop by updating the pagination Boolean value to False:

```
try:
    # Check for Pagination with text 'next' in the
    Listing page
    driver.find_element(By.LINK_TEXT,'next').click()
    page = page+1
except NoSuchElementException:
    pagination = False
    print(f"Further Pagination is not possible, currently at {page}")
```

> **Important note**
> For more information on exception handling, please visit the following links: https://docs.python.org/3/tutorial/errors.html and https://selenium-python.readthedocs.io/api.html.

Upon the completion of loops for pages and listings, the final data collected in dataSet is given planned column names listed in a list, ['Upc', 'Title', 'Price', 'Rating', 'Stock', 'Stock_Qty', 'Url', 'Image'], and used to create a CSV and JSON file.

## Example 2 – forms and searching

In Example 2, we will use Selenium to log in, log out, and go through available pages for collecting various quotes-based data from http://quotes.toscrape.com/. Similar to Example 1, we will locate and process the link that contains the Login text from http://toscrape.com.

The following code searches for an anchor element with the Login text and collects the href attribute. The click() action is applied to the Login element. This loads the http://quotes.toscrape.com/login page. WebDriver has a few attributes, such as current_url. This attribute returns the URL of the page loaded in the browser:

```
mainUrl=https://toscrape.com/
driver.get(mainUrl)  # loads toscrape.com
loginPage = driver.find_element(By.LINK_TEXT,
    "Login").get_attribute('href')
print(loginPage)
http://quotes.toscrape.com/login

driver.find_element(By.LINK_TEXT, "Login").click()

print(f"Before Login : {driver.current_url}")
http://quotes.toscrape.com/login
```

The element with the Login text is near the footer section of the page loaded, and we need to scroll down to view the element. If the browser window has a vertical scrollbar (depending on the dimensions of the window, if configured), the driver.find_element(By.LINK_TEXT, "Login").click() code will scroll down the page vertically till the element with the Login text is visible, and then click on it. This is a reduction of the window scrolling related code that had to be coded in versions before 4.1.0.

The http://quotes.toscrape.com/login page contains the HTML <form> element with a few inputs and a submit button. The form accepts any credentials, such as username=test and password=test. Since <input> types have to be filled with credentials, they are identified first, cleaned using clear(), and finally, the text value is entered using send_keys(). These actions automate scrolling or moving up to the element, clicking the element, and typing characters:

```
username = driver.find_element(By.ID, "username")
username.clear()  # cleans if there exist any characters
username.send_keys("test") # value entered
password = driver.find_element(By.ID, "password")
password.clear()
password.send_keys("test")
```

With username and password values provided to the respective input elements, form submission has to be done. Submit-related methods were available in versions before 4.0, but we can achieve the same thing by locating the button that submits form values and clicking it (using the click() method):

```
driver.find_element(By.CLASS_NAME,'btn').click()
# click submit button
quotesUrl = driver.current_url
print(f"After Login : {quotesUrl}") http://quotes.toscrape.com/
logoutUrl = driver.find_element(By.LINK_TEXT,
    "Logout").get_attribute('href')
print(f"Logout : {logoutUrl}") http://quotes.toscrape.com/logout
```

Upon submitting the login form, you will notice that there is a link with the Logout text and the http://quotes.toscrape.com/logout URL. This validates that the login-related step has been successfully completed.

**Important note**
Readers and developers will find the processing of the HTML form quite easy. The HTTP POST method is submitted with payload data (username, password, and csrf_token) if traced using **DevTools.**

There are multiple quotes <div> blocks on the page with the pagination element (driver.find_element(By.CSS_SELECTOR,'li.next a').click()). quotes identifies each individual block of the quote located using find_elements(). Pagination is managed using Boolean values and the required or planned data is extracted from each quote block. Finally, the data is added to the main dataSet collector:

```
quotes = driver.find_elements(By.CSS_SELECTOR,
    "div.row .quote")
for quote in quotes: # Iterate quotes available
    content =
        quote.find_element(By.CSS_SELECTOR,'.text').text
    author =
        quote.find_element(By.CLASS_NAME,"author").text
    authorLink =
        quote.find_element(By.PARTIAL_LINK_TEXT,"about")
        .get_attribute('href')
    authorGoodread =
        quote.find_element(By.PARTIAL_LINK_TEXT,"Goodread")
        .get_attribute('href')
    tags =
        quote.find_element(By.TAG_NAME,'meta')
        .get_attribute('content')
```

```
.....:
# Add values to dataSet
dataSet.append([author, content, tags, tag_count,
    authorLink, authorGoodread])
```

With the completion of the pagination loop and quotes from each page, finally, the logout action is performed. After logout, the browser loads the http://quotes.toscrape.com page:

```
driver.get(logoutUrl)
print(f"Current URL: {driver.current_url}")
http://quotes.toscrape.com/
```

With data in hand or in the main dataSet collector, this data can be written as a CSV file (using the 'author', 'quote', 'tags', 'tag_count', 'author_url', 'goodread_url' header) or a JSON file. Finally, the browser loaded by WebDriver is closed using driver.quit().

The web-scraping examples in this section used various identifiers, locators, attributes, and methods from the selenium library. WebDriver is effective for automating tasks, either repetitive or non-repetitive. Web application-related testing is commonplace where Selenium is used, but we have explored it for scraping purposes. To explore Selenium further, please explore the content and links shared in this chapter.

## Summary

The Selenium framework has many features and is widely used for application testing and browser automation. Exploring its features, we learned how to use WebDriver for scraping-related tasks. Python programming does have independent libraries to deal with HTML or web content, browser properties, networking, parsing, and more. Selenium can be used to process such features independently, and it is a major advantage that Selenium holds over various other Python libraries. The framework is also updated continuously, enriching the platform with testing, automation, and developer-friendly features.

In the next chapter, we will learn about regular expressions and using them to extract or scrape data.

## Further reading

- Selenium:
  - https://selenium-python.readthedocs.io/
  - https://developer.mozilla.org/en-US/docs/Web/HTML
  - https://www.lambdatest.com/blog/selenium-webdriver-tutorial-with-examples/
