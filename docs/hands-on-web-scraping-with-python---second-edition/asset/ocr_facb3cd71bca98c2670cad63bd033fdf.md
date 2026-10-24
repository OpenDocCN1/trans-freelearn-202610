Let’s first verify the setup of the selenium library:

```
import selenium
selenium.__version__    # 4.10.0
selenium.__spec__
ModuleSpec(name='selenium',..... submodule_search_locations=['C:\\HOWScraping2E\\secondEd\\Lib\\site-packages\\selenium'])

```

This code block shows that the Python selenium library with version 4.10.0 has been successfully installed in our target environment (please refer to the Setting things up and Creating a virtual environment sections of Chapter 2).

After verifying the Python library, we will install the drivers (browser drivers) in the next section.

# Installing drivers

Before installing the browser drivers, identifying system specifications is essential. For the purpose of this book, we will use Windows OS and choose Google Chrome as our browser driver.

> **Important note**
> Selenium supports multiple web browsers, such as Firefox, Internet Explorer, Safari, Chrome, and others. If you are using Firefox, please visit the https://firefox-source-docs.mozilla.org/testing/geckodriver/Support.html link for more details on setup.

Let’s follow these few steps to set up drivers for Google Chrome:

+   1. Visit https://www.selenium.dev/downloads/ and go to the Platforms supported by Selenium section.
+   2. Now go to the Browsers | Chrome option. The documentation link to https://chromedriver.chromium.org/ should be available.
+   3. Click the link with the Getting started with ChromeDriver on Desktop text, which is available under the ChromeDriver Documentation option.
+   
+   Clicking the link will land you on the https://chromedriver.chromium.org/getting-started page.
+   4. In the Setup section, there is a Downloads link. Clicking the Downloads text will route you to the https://chromedriver.chromium.org/downloads URL.
+   5. Go to the https://chromedriver.chromium.org/downloads URL, and click the link with the ChromeDriver 111.0.5563.64 text, as shown in Figure 8.1: