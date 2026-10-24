The Python libraries that are required for this chapter are as follows:

+   - `lxml`
+   - `urllib`

The code files for this chapter are available online on GitHub: `https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter03`.

> **Important note**
> 
> Web- or website-based content refers to the responses or page sources that are received after processing requests to a URL. Content can be of various types, such as PDF, CSV, TXT, XML, HTML, and JSON. In general, in this chapter, we are talking about HTML, page source, or markup documents as our primary content, unless stated otherwise.

# Introducing XPath and CSS selectors to process markup documents

In the *Understanding the latest web technologies and Data-finding techniques used in web pages* sections in *Chapter 1*, we explored and discussed **HTML** and **XML** markup documents and their availability across the web.

Normally, markup is a kind of labeling or tagging of parts, sections, or any entities in documents, which helps to identify the content and even process it using a third-party application. We call them **tags** in HTML (`https://www.w3.org/html/`) and **nodes** in XML (`https://www.w3.org/standards/xml/`). Hence, markup documents are a tree-like structure, containing tags or nodes (nested or individual), also known as an **element tree**.

> **Important note**
> 
> XML documents have been pretty popular and common across the web since the start of the growing internet era. Readability, encoding support, interoperability, and data exchangeability are a few core powers of XML. XML is still supported by the latest web technologies and is the utmost backbone of markup documents.

With the brief overview of markup and XML we have received here, the next sections will introduce the **Document Object Model (DOM)**, XPath, and CSS selectors and using them for web scraping purposes with the help of a Python library.