# Basic exploring

In the following code example, we will get an understanding of a few basic activities using Selenium that are normally priority tasks:

```
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
chromedriver_path="C:\HOWScraping2E\driver\chromedriver.exe"
service = Service(service=chromedriver_path)
driver = webdriver.Chrome(service=service)
# chromedriver
```

Here, an empty, new Chrome window gets loaded similar to the browser window shown in Figure 8.3.

We will now supply three different URLs and iterate them by obtaining some information:

```
urls = {
    'google':'https://www.google.com',
    'python':'https://www.python.org',
    'selenium':'https://www.selenium.dev'
}
```

Now, let's iterate items() in the urls dictionary:

```
for key,url in urls.items():    # iterate the urls items
    driver.get(url)
    driver.implicitly_wait(1)      # Wait 1 sec
    print(driver.title)           # HTML <title>
    print(driver.current_url)     # Current URL
```

driver.get(url) loads the URL in the browser. The implicitly_wait(1) method has been used as the sleep time, waiting for the allotted 1 second before any further action is taken. The driver.title code returns the <title> HTML value. driver.current_url reveals the redirect URL or the latest URL that is being loaded in the browser.

Selenium also supports features related to screenshots. get_screenshot_as_file() is one of the common methods used for this purpose:

```
print(driver.get_cookies())
driver.get_screenshot_as_file(key+".png")  # png files
print(driver.page_source)      # HTML page source
driver.implicitly_wait(3)      # wait 3 sec before page Refresh
driver.refresh()               # refresh the page
```