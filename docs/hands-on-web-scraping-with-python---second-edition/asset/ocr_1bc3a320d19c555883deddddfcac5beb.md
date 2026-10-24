+   8. Depending on the OS (Windows OS in our case), choose chromedriver as shown in Figure 8.2, and download it.

+   9. After successfully downloading the zipped file (chromedriver_win32.zip), it can be unzipped or the contents (chromedriver.exe) can be extracted to the project root folder or any appropriate location. The path for chromedriver.exe can be set in a variable and used to develop code.

> **Important note**
> There are multiple ways to install and access chromedriver using selenium. Please follow the links provided for more detailed information: https://selenium-python.readthedocs.io/installation.html and https://www.selenium.dev/documentation/webdriver/getting_started/install_drivers/. Similar steps can be followed if any different browser is to be tried or tested.

With the setup-related steps completed, in the next section, we will verify the setup with the help of a code example.

# Verifying the setup

During the setup stage, we have to visit and process a few links, download the files, and so on. To verify the setup and confirm that the driver is running fine, let us deploy the code that loads the https://www.python.org URL in the browser (Chrome) with the help of selenium and chromedriver:

```
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
chromedriver_path="C:\HOWScraping2E\driver\chromedriver.exe"    # path
service = Service(service=chromedriver_path)
driver = webdriver.Chrome(service=service)
# initiate an empty Chrome window
driver.get('https://www.python.org')  # loads URL in browser
driver.quit() # closes browser & terminates the session
```

In this code block, we first imported webdriver and then Service. selenium.webdriver consists of various tools and classes. Service is one of the classes from webdriver.chrome.service. Service is mainly associated with browser-based features and functions. The path (location) to chromedriver.exe is important and has to be provided to the service.