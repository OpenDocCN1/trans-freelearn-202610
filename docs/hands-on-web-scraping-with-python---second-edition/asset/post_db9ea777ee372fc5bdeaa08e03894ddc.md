
6. *Step 5* will load the deployed code or project as jobs that will be available to execute, run, schedule, and many more options under **Jobs Dashboard**, as seen in *Figure 5.10*:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_150_0.png)

Figure 5.10: Listing jobs and Spider links

7. *Step 6* shows that **1 spider** exists. Click on the spider, which will take you to **Spider Details**, and provide some information on the spider. We now can run the spider by clicking the **Run** option from **Jobs Dashboard**, and then on **Spider Details**.

8. You can verify that the run was successful by clicking on the spider name (**booklist**) in the **Completed, Deleted, Running**, and **Next** sections, as seen in *Figure 5.11*:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_150_1.png)

Figure 5.11: Spider jobs completed

9. As seen in *Figure 5.11*, details such as **Items, Requests, Runtime**, and **Errors** are also available and can be clicked on for a more detailed investigation. *Figure 5.12* shows us the information under **Items**:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_151_0.png)

Figure 5.12: Job Items (loading Item 0 or the first item)

10. There are multiple options, such as Download, Exporting to Files (CSV, JSON, XML), Filtering, Dataset Publishing, and Samples, that are available under the Jobs Items section, as seen in Figure 5.12.

With the steps mentioned, we successfully deployed our Scrapy project to app.zyte.com and extracted the details as per the code available in the spider (booklist).

Important note
It’s also advisable that you test the crawler on your local machine for any exceptions and errors that might occur, testing all features (such as running the crawler and exporting data) and using the latest libraries (if possible). Regarding deployment or any issues in Zyte, please visit https://support.zyte.com.

In this section, we focused on the deployment of our crawler in the server.

## Summary

In this chapter, we explored and learned about parsing and extracting data from the web using *Beautiful Soup* and *Scrapy*.

So far, in this book, we have identified many libraries and techniques that are effective and suitable for web scraping. Beautiful Soup equips developers with a handful of features to parse, traverse, and create a crawler. Scrapy provides the same features as Beautiful Soup and can be used for data extraction, but is more of a project-based framework that uses lots of libraries behind the scenes and enables you to focus only on your tasks. Because of Scrapy’s easy-to-implement and collaborative architecture, it’s quite popular among beginners, professionals, and even web-based service providers such as Zyte, *ScrapeOps*, and *Apify*.

In the next chapter, we will learn about and explore more scraping techniques and security-related issues.

## Further reading

- Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- lxml – XML and HTML with Python: https://lxml.de/
- Web crawlers:
  - https://www.cloudflare.com/en-gb/learning/bots/what-is-a-web-crawler/
  - https://research.aimultiple.com/web-crawler/
- Web scraping: https://www.zyte.com/learn/
- robots.txt: https://www.robotstxt.org/robotstxt.html
- Scrapy Cloud: https://www.zyte.com/scrapy-cloud/
- Scrapy: https://docs.scrapy.org/en/latest/index.html

## Part 3: Advanced Scraping Concepts

In this part, you will learn how to work with secure or security-enabled websites. You will learn to scrape data from web APIs and use browser automation tools such as Selenium to bypass certain imposed security. There are practical examples across the chapters in this part, which will help you to extract data from PDF files and even unstructured formats using regular expressions.

This part contains the following chapters:

- *Chapter 6, Working with the Secure Web*
- *Chapter 7, Data Extraction Using Web APIs*
- *Chapter 8, Using Selenium to Scrape the Web*
- *Chapter 9, Using Regular Expressions and PDFs*

## Working with the Secure Web

So far, we have learned about the web, web content, reverse-engineering techniques, data-finding techniques, a few Python libraries, and a framework that we can employ to access and scrape the desired web content.

Plenty of security-related concerns exist on web platforms today, along with measures to ensure security. Lots of web applications, extensions, and even web-based service providers exist to protect us and our web-based systems against unauthenticated usage and unauthorized access.

The growing use of internet applications and e-commerce activity demands a secure web (or web-based security-enabled features) as a high priority to deal with actions that are harmful or even illegal. We often receive irrelevant emails or spam, containing information that we did not ask for.

It’s quite challenging from a web scraping perspective to deal with such issues, but the concept of ethical hacking makes it more viable, even from an application and security perspective. We will cover a few basic concepts that deal with security on the web and proceed to look at web scraping.

In this chapter, we will learn about the following topics:

- Exploring secure web content
- HTML <form> processing using Python
- User authentication and cookies
- Using proxies

### Technical requirements

A web browser (*Google Chrome* or *Mozilla Firefox*) will be required, and we will also use *JupyterLab* for Python code.

Please refer to the *Setting things up* and *Creating a virtual environment* sections in *Chapter 2* to continue using the environment created.

The Python libraries that are required for this chapter are as follows:

- `requests`
- `pyquery`

The code files for this chapter are available online in this book's GitHub repository: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter06.

### Exploring secure web content

Today's web and internet technologies are quite vulnerable in terms of web security (content, authorization, illegal access, and so on). We want the web to be safe and the content that we browse, search, or view to be genuine, not violating any legal or ethical standards or affecting people's human rights.

The web must be accessible and available to everyone who seeks information, effectively and by following ethical practices. We often encounter web content that is not exactly what we were looking for, or hear of web content that has been tampered with or hacked into or that private and sensitive information has been leaked illegally, and so on. Although a lot of these cases are beyond our control, we can reduce vulnerabilities and make the web a much safer place to be.

In many new technologies, security-related concerns have been identified and solutions have been implemented. There are even applications and organizations concerned with network-based security, which are growing in demand and using the security enabled technologies. In government departments, developers use up-to-date technologies to make the web a safer place. With such improvements in technology, scraping and crawling are getting more challenging. Web scraping, or extracting the information available on the web, is not harmful when ethical concerns are taken into account.

In the upcoming sections, we will introduce some basic security-related concepts that can be implemented and are easily available on the web, and then explore, implement, and access them using Python.

### Form processing

Form or HTML `<form>` processing, also known as form handling, is used in most cases to pass or submit collected or user-provided data to a backend or server. There are plenty of cases of web-based forms (such as login, contact us, registration, and subscription forms) being submitted by someone impersonating someone else.

From a user perspective, a form is simply a data collector that has some `<form>` elements, such as `<select>`, `<option>`, `<textarea>`, and `<input>`, asking for some input or values. When data is provided, it gets validated at some level and is processed by the server or concerned system, and any necessary further action takes place (such as user login, user registration, resetting a forgotten password, QR processing, and image validation).
