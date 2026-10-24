
## Using Selenium to Scrape the Web

So far, we have learned about some Python libraries, web and API-based technologies, data-finding and locating elements, extraction techniques, and plenty of data-related services in Chapters 1 to 7.

Selenium automates browsers – a quote from https://www.selenium.dev/, and it is primarily a collection of tools also known as a testing framework. Selenium is used to automate the web (applications, website forms, and much more) for testing purposes. Along with testing using automation, there are many potential service cum task-based scenarios that can be performed and handled using Selenium. The Selenium framework consists of various modules or components. We will be using Selenium WebDriver.

In general, we will install and learn about Selenium WebDriver, use WebDriver to automate websites, and use Selenium to scrape data from the web.

In this chapter, we will cover the following topics:
- Introduction to Selenium
- Using Selenium WebDriver
- Scraping using Selenium

### Technical requirements

You will require a web browser (Google Chrome or Mozilla Firefox) and we will be using JupyterLab for Python code.

Please refer to the Setting things up and Creating a virtual environment sections of Chapter 2 to continue with setting up and using the environment created. We will be using Google Chrome with Selenium WebDriver v4.10.0.

To install the `selenium` Python library, the following links will be very helpful:
- `https://selenium-python.readthedocs.io/installation.html`
- `https://www.selenium.dev/documentation/webdriver/getting_started/install_library/`
- `https://pypi.org/project/selenium/`

We will require the following Python libraries for this chapter:
- `requests`
- `selenium`
- `re`
- `csv`
- `json`

The code files for this chapter are available online in this book’s GitHub repository: `https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter08`.

### Introduction to Selenium

The testing of web-based applications or systems is compulsory according to the **system development life cycle (SDLC)**, and this step is done prior to the launch of applications on the web. Selenium, an open source project, uses a web browser as an interface for automation and can be used for web-related or web-based activities.

Dynamic and secure web applications using JavaScript (JS), cookies/sessions, other JS scripts, and many more web components can be handled, processed, automated, and crawled with the use of Selenium. *Selenium is an umbrella project for a range of tools and libraries that enable and support the automation of web browsers.* (`https://www.selenium.dev/documentation/overview/`). Though primarily used for browser-based automation, different cases that can be managed or used with a browser can be tackled using Selenium. This makes Selenium the most popular and appreciated automation-related browser-based tool.

Selenium is mostly used for web testing. In the following sections, we will explore the benefits and usages of Selenium, and how we can use it for web scraping.

### Advantages and disadvantages of Selenium

Selenium supports different web browsers through one of its components, called WebDriver. There are many benefits that make Selenium popular, and a few are as follows:
- Easy to implement
- Cross-browser support
- Open source and free
- Supports parallel testing
- Supports multiple **operating systems (OSs)**
- Supports multiple languages (Java, Python, Ruby, PHP, and others)
- A huge collection of docs and resources is available
- Supports remote servers and cloud devices

Using and implementing Selenium in application testing has many advantages, but there also exist some limitations or disadvantages:
- Working with multiple tabs and frames
- Low execution speed (depending on the machine)

Despite the large number of advantages and extendibility that Selenium offers, it is slow in terms of processing compared to some of its competitors (**Playwright**, **Puppeteer**), and its large memory consumption is still a debatable issue, which is discussed in web-based groups and communities.

> **Important note**
> For more details on a few selected automation libraries and frameworks, please refer to Selenium https://www.selenium.dev/about/, Puppeteer https://pptr.dev/, and Playwright https://playwright.dev/.

### Use cases of Selenium

There are plenty of cases for the use of Selenium in projects. From a web-scraping perspective, Selenium can be used in both normal and complex cases. For normal scraping cases, we can definitely use other libraries and techniques that we learned about earlier, in *Chapters 2 to 7*.

Selenium is used for automation and testing on the web. The use of Selenium is preferred (many times, even as a last option) in specific cases or when scraping is not possible with other libraries and techniques. Some of the cases in which the use or involvement of Selenium for scraping tasks may be required are as follows:
- Handling alerts, iframes, and popups (time-bound)
- Collecting and using cookies and sessions
- Addressing scrolling and clicking activity (ensuring anti-bot measures on websites)
- Working on JavaScript-based websites (websites with dynamic values or elements)
- Taking screenshots
- Using headless mode (less consumption of resources)
- Bypassing basic authentication (hidden or dynamic values)
- Dealing with HTML forms
- Executing and injecting JavaScript code
- Impersonating human action on a page

Selenium is a framework or collection of various task-based modules (libraries). We will be introducing these modules in the next section.

### Components of Selenium

Application (web-based) testing is done at various stages of the development cycle and even multiple times. This type of testing ensures that the development is requirement-specific or is progressing as planned. It also helps to find any possible bugs or errors and to overcome or document them.

Generally, testing is done manually (by users) and/or using automated tools such as Selenium. Automating testing and verification tasks is one of the core components of the application life cycle. Selenium consists of the following three major components or projects (https://www.selenium.dev/projects/):
- **Selenium WebDriver**: This is an Object-Oriented (OO) API and one of the main components that is used to automate the browser. Browser automation is done by providing commands with the help of in-built APIs and other languages (JAVA, PHP, and Python) to third-party browser drivers such as Google Chrome, Mozilla, and Opera. **WebDriver** allows a program or script to access the browser in a native way, just like a user would. For scraping-related tasks, we normally deal with WebDriver, and we will explore its features in the *Using Selenium WebDriver* section. For more details on WebDriver, please visit this link: https://www.selenium.dev/documentation/webdriver/.
- **The Selenium IDE:** This is a ready-to-use browser extension, also known as a **User Interface (UI)** of Selenium that records user actions in the browser. The IDE also provides a feature that plays back a recorded action, along with the commands deployed and the defined parameters with values. It also has debugging features such as setting breakpoints, addressing exceptions through the IDE, creating scripts that can run commands, and supporting the control-flow structure. This extension is available for Google Chrome and Mozilla Firefox. For more details on the Selenium IDE, please visit this link: https://www.selenium.dev/documentation/ide/.
- **Selenium Grid:** This allows running tests or doing automation across multiple machines and browsers and cross-platform testing. Grid also allows component-based configuration that is deployed across machines or platforms. Basically, Selenium Grid supports distributable testing. This helps reduce the testing time and identify performance issues in parallel for different systems. For more details on Selenium Grid, please visit this link: https://www.selenium.dev/documentation/grid/.

With this brief overview of and introduction to Selenium, we will now install and explore Selenium WebDriver using code and examples in the next section.

### Using Selenium WebDriver

Selenium is used for browser automation, and one of its major components, WebDriver, is the core tool to access browsers. WebDriver implements code logic for selected browsers that is required during automation. It's also the core system that binds the Selenium framework with the browser and often gets called or referred to as **Selenium driver** or only **driver**. For more detailed information, visit this link: https://www.selenium.dev/documentation/webdriver/getting_started/.

Before going deep into the automation or using the framework, let's install the required libraries in the next section.

### Setting things up

To explore browser automation using Python and Selenium WebDriver, first, we need to install the `selenium` library (a Python library), and browser-related drivers.

> **Important note**
> Selenium is a framework that contains various components such as WebDriver and others, whereas `selenium` is a Python library (https://www.selenium.dev/documentation/webdriver/getting_started/install_library/) that we use to code and maintain logic that binds WebDriver and the selected browser.
