
## Sentiment Analysis

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_291_0.png)

Figure 11.9: Plotting of positive and negative values

With some data and analysis done in this section, it might be fair to suggest that most of the records found in quotes are neutral in nature.

> **Important note**
> 
> Sentiment analysis using `nltk` involves more steps in comparison to `textblob` and `vaderSentiment`. In addition, it has by far the largest collection of methods and attributes. Please visit `https://www.nltk.org/` for more details.

In this chapter, we learned about a few basics regarding ML and how ML, with its various techniques, can be helpful and play a significant role in various situations when quality data scraped from the web is provided.

## Summary

Python programming makes a huge contribution in AI- and ML-related domains. In this chapter, we have had only a glimpse of that. Quality data plays a very important role in ML. Whether collecting data via web scraping and storing it or providing scraped data on the fly to an ML model, prepared data is in demand. The better the quality of the data – and the more precise the data is – that we provide to ML algorithms, and for plotting charts, the more accurate results, visualizations, and descriptive plots we can expect.

We have now learned about ML concepts and various aspects of ML by exploring them. We have also learned how to implement ML models and collect the results, if required, from various processes. To summarize, we now have an overview of how to use scikit-learn and conduct sentiment analysis. ML is data-driven and quality data is a basic requirement for ML models to provide accuracy.

In the next chapter, we will learn about a few further steps and topics that are beneficial and applicable to web-scraped data and can be used in the web scraping process.

## Further reading

- **Machine learning:**
  - https://www.ibm.com/topics/machine-learning
  - https://www.mygreatlearning.com/blog/what-is-machine-learning/
  - https://www.run.ai/guides/machine-learning-engineering/machine-learning-automation
- **Machine learning – Python:**
  - https://scikit-learn.org/
  - https://www.w3schools.com/python/python_ml_getting_started.asp
  - https://realpython.com/tutorials/machine-learning/
  - https://machinelearningmastery.com/machine-learning-in-python-step-by-step/
- **Regression analysis:**
  - https://www.alchemer.com/resources/blog/regression-analysis/
  - https://www.investopedia.com/terms/r/regression.asp
- **Natural language processing:**
  - https://www.nltk.org/
  - https://www.ibm.com/topics/natural-language-processing
  - https://hbr.org/2022/04/the-power-of-natural-language-processing
- **Sentiment analysis:**
  - https://monkeylearn.com/sentiment-analysis/
  - https://huggingface.co/blog/sentiment-analysis-python
  - https://aws.amazon.com/what-is/sentiment-analysis/

## Part 5:
Conclusion

In this part, you will be provided with a few select advanced concepts that can be implemented alongside or on top of scraped data or datasets, for advanced processing and features. You will also learn about the jobs and career-related scopes related to web scraping and data science that are in high demand globally.

This part contains the following chapter:

- *Chapter 12, After Scraping – Next Steps and Data Analysis*

## 12

## After Scraping – Next Steps and Data Analysis

So far, we have learned how to scrape the web, analyze the data collected, and apply **machine learning (ML)** algorithms using Python programming.

This chapter will provide a basic introduction to some emerging concepts and technologies that are becoming crucial in the field. Being able to draw insights and knowledge from data is in high demand, and even supply-related scopes have been growing exponentially. From the fields of research to business, the importance of data has grown rapidly. **Artificial intelligence (AI)**-based systems, powered by ML logic, will continue to consume and generate more data.

In this chapter, we will learn about the following topics:

- What happens after scraping?
- Web requests
- Data processing
- Jobs and careers

## Technical requirements

A web browser (*Google Chrome* or *Mozilla Firefox*) is required to explore the provided resources.

## What happens after scraping?

Web scraping is a method of collecting and supplying quality data, and applying various techniques that depend on data. Any faculty or domain that involves data provides the opportunity to learn and grow as data collected from scraping is used for many other tasks such as analysis, reporting, dataset creation, and mining. Web-scraping-related techniques have always been dynamic and challenging, alongside the growth of web-based technologies.

Systems based on data, returning raw data, processed data, and visualization plots, in the form of images and videos, are growing by involving global audiences in multiple forms. **Information technology (IT)**-driven systems are core applications used in all industries, and Python programming has played a major role in the development and processing of data-related systems.

Earlier chapters of this book presented various steps that consolidated a few main concepts and actions, as listed here:

- Demand for data
- Collection of data
- Data analysis (information generation)
- Use of data (ML/AI)

A large number of frameworks and programming languages are being developed. Applications related to data, information, and knowledge management are most used in business- and research-based fields.

While we have discussed various features and techniques of web scraping and collecting data, as entities are constantly being created and evolving, being able to adapt our approaches and techniques is important. Some of the factors, such as the effectiveness of the system, consumption of time, content volume, and availability of applicable technology, affect the overall web scraping process, and even the steps carried out after data has been collected.

In the next section, we will introduce some advanced technologies and Python libraries that deal with web-related requests.

## Web requests

Throughout the chapters of this book, the `requests` Python library has been used to establish communication between the code and the web. Plenty of Python libraries can be found at https://pypi.org/ if we search for ones similar to `requests`.

The following subsections list some Python libraries and technologies and provide brief introductions to them.

## pycurl

The pycurl Python library (http://pycurl.io/) is a wrapper on top of the popular libcurl library. libcurl is one of the earliest Python libraries that was used to communicate with websites on the internet, based on the **curl** tool (also known as **cURL**).

`curl (https://curl.se/)` is a command-line tool that is used to connect and transfer data over the web. `curl` is the basis of network communication; it's a core implementation that is used with the help of a wrapper across different **operating systems (OSs)**, browsers, and machines that communicate

with the internet. The curl command is machine-independent. Programming languages such as Python, PHP, C, R, Java, and Rust convert the raw curl commands into their preferred format, which can be used in their code or by their libraries.

pycurl, developed in the C programming language, provides curl-based facilities to Python code. The requests library is often considered easy to use, in comparison to other libraries. pycurl provides almost all of the same features and functionality as curl, including its attributes and methods.

Listed here are some of the major advantages that pycurl possesses over requests:

- Supports a large number of communication protocols (HTTPS, POP3, FTP, LDAP, IMAP, and many more)
- Faster in comparison to requests and other similar Python libraries

Modern browsers even provide the raw curl command, which can be found using browser-based developer tools, as mentioned earlier in Chapter 2. These curl commands can be copied for use with the following:

- **Command-line interface (CLI)** applications (Bash shells or scripts) or terminals
- Web-based terminals and API tools such as **Postman** (https://www.postman.com/)
- Python libraries such as requests (https://curlconverter.com/python/) with headers
- Converting a command into other programming languages

Figure 12.1 shows the curl command to load https://curl.se/ copied from the browser-based developer tools using the Copy as cURL (bash) option.

```
curl 'https://curl.se/' \
  -H 'authority: curl.se' \
  -H 'accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7' \
  -H 'accept-language: en-US,en;q=0.9' \
  -H 'cache-control: max-age=0' \
  -H 'if-modified-since: Thu, 01 Jun 2023 10:05:02 GMT' \
  -H 'if-none-match: "2153-5fd0e9e9a8fc-gzip"' \
  -H 'sec-ch-ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Windows"' \
  -H 'sec-fetch-dest: document' \
  -H 'sec-fetch-mode: navigate' \
  -H 'sec-fetch-site: none' \
  -H 'sec-fetch-user: ?1' \
  -H 'upgrade-insecure-requests: 1' \
  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36' \
  --compressed
```

Figure 12.1: curl command
