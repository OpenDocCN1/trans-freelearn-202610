
> **Important note**
>
> Using the **Copy as cURL (bash)** option is almost like copying *request headers* from the browser developer tools. Browsers extract, prettify, and present such information with some formatting. **Postman** also allows importing cURL commands and presenting them in an *HTML <form>*-based layout (editable text boxes and more), where developers can make their choices and proceed with web requests, as well as carry out testing in parallel.

The cURL command obtained through the browser, as seen in Figure 12.1, can be copied to https://curlconverter.com/python and converted into Python code ready for libraries such as `requests` and `http.client`. The following code is the converted/generated Python code for requests:

```
import requests
headers = {
    'authority': 'curl.se',
    'accept': 'text/html,application/xhtml+xml,
               application/xml;q=0.9, image/avif,
               image/webp, image/apng,*/*;q=0.8,
               application/signed-exchange;v=b3;q=0.7',
    'accept-language': 'en-US,en;q=0.9',
    'cache-control': 'max-age=0',
    'if-modified-since': 'Thu, 01 Jun 2023 10:05:02 GMT',
    'if-none-match': '"2153-5fd0e91e9a8fc-gzip"',
    'sec-ch-ua': '"Not.A/Brand";v="8", "Chromium";v="114",
                "Google Chrome";v="114"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'document',
    'sec-fetch-mode': 'navigate',
    'sec-fetch-site': 'none',
    'sec-fetch-user': '?1',
    'upgrade-insecure-requests': '1',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64;
                   x64) AppleWebKit/537.36 (KHTML, like Gecko)
                   Chrome/114.0.0.0 Safari/537.36',
}
response = requests.get('https://curl.se/',
                        headers=headers)
```

Using cURL commands, even using pycURL, will help us significantly in learning how to use core technologies related to web-based communication. As technical development-related processes are being routed to AI-driven and ML-based systems, understanding these core concepts will make a difference.

In the next section, we will learn about an add-on feature (middleware feature) that can be used in web requests.

## Proxies

A proxy or proxy web server (also known as middleware) is one of the most important and core technologies in network communication, especially web scraping. Plenty of web security-related challenges arise in network communication during scraping, and using a proxy helps us to overcome them.

Using a proxy when scraping usually hides the original machine’s IP. Be aware, however, that some providers will not always hide the original IP. A proxy communicates with the target site using some other secure IPs that are available through proxy service providers. A proxy works as middleware and helps to bypass web security on physical machines. One of the most used proxy types in web scraping is rotating **residential proxies**, which change the IP address on every new request.

The internet is open content. Web servers impose a large number of security measures to keep their content safe. Many of you will have encountered some content that was blocked to view or access, which can be overcome using proxies. In some way or another, either on public or on-premises networks, proxies provide great security and filtering capabilities.

When dealing with web scraping for long-form content, such as daily blogs, reviews, stock information, and ratings, there are various recurring scraping tasks for fixed sites, as well as scheduled scraping options, including hourly intervals. However, it is important to consider the possibility of encountering web security measures that could potentially lead to blocking, such as the following:

- IP blacklisting
- Facing CAPTCHAs on every page
- The inability to use filtering options
- Redirecting to pages that are not targeted (such as those hosted in a different country or in a different language from that of the user)
- Redirecting to the first page only
- Redirecting to the main page

These kinds of things can lead to wasted time, financial loss, loss of disk and web space, collecting irrelevant data, reduced quality of data, and much more. To resolve this, developers need to use and implement web-based penetration testing, such as using proxies; selecting rotating IPs for a selected country, a random combination of proxies with **User-Agent**, or randomized or standard *request headers*; or executing `curl` commands in the terminal to verify content.

Listed here are some of the situations in which proxies can be helpful:

- Overcoming territory (border-based) restrictions
- Preventing blacklisting
- Rotating the IP address
- Performance enhancement
- Bypassing CAPTCHAs, malware, spam, and more
- Overcoming blocked access
- Implementing privacy features

With numerous advantages of using proxies, there are two significant disadvantages too, as listed here:

- Extra financial requirements (buying proxies)
- Extra configuration (on top of the existing network and code)

Proxies are often found and prescribed for use with scraping or crawling tasks.

As seen in the following sample code template, the Python random library's shuffle method is being used to randomize values from user_agents and proxyList. The following code will not run until and unless we acquire a paid subscription from the website for the valid api-key, which results in multiple collections of IPs with ports for http and https:

```
import requests
import random
user_agents = ['Mozilla/5.0 (iPhone; CPU iPhone OS 16.5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/114.0.5735.99 Mobile/15E148 Safari/604.1', 'Mozilla/5.0 (iPod; CPU iPhone OS 16_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/114.0.5735.99 Mobile/15E148 Safari/604.1', 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36',]
# proxyList = dict(requests.get("https://api.demo-web.com?api-key=XXXX-XX-XXXX-XXX&zone=US&type=multiple&timeloc=12h&channel=chr_moz").content)
proxyList = {'http':['xxx.xxx.xxx.xxx:xx', 'xxx.xxx.xxx.xxx:xxx','xxx.xxx.xxx.xxx'],
'https':['xxx.xxx.xxx.xxx:xx','xxx.xxx.xxx.xxx:xxx', 'xxx.xxx.xxx.xxx']}
def rand_output(input):
    return random.shuffle(input)
response = requests.get('https://www....com',
headers={'User-Agent': rand_output(user_agents)},
proxies = rand_output(proxyList['https'])).text
```

proxyList contains a list of IPs that have visited the service provider pages. IPs can also be collected using API requests to proxy providers. The rand_output function receives a Python list() object and returns the `shuffle` list. `requests` supports the `headers` and `proxies` arguments, and more, where desired values can be provided.

Most cloud-based platforms and web hosting organizations provide an option if required to use their in-house proxy services or even allow using third-party proxy settings through some additional configuration. Many proxy providers use a web API and allow users to access proxies using some authentication system (an API key). Users can search `https://www.google.com/search?q=proxy+providers` on Google to find the best proxy solution and use or choose the appropriate solution.

> **Important note**
>
> There are plenty of browser-based extensions that provide **virtual private network (VPN)** access, rotating IPs, and more. Python libraries also exist for proxies, user agents, generating security-related tokens, and more. These extensions and libraries might cause system vulnerabilities, or work temporarily or permanently, but it’s the responsibility of the users to do the proper research before deciding whether to use them.

In the next section, we will introduce some new tools and applications that are useful in data processing.

## Data processing

Data processing, in the context of web scraping, refers to storing, handling, managing, and analyzing the data that is generated from scraping. In previous chapters of the book, we focused on the concept of effective and efficient scraping with code examples.

As the demand for data is growing, technologies are also evolving and adapting to new changes. Currently, as there has been a boom in AI/ML-based systems, there is competition to provide easy and quick solutions to problems without compromising on quality.

In the coming sections, we will introduce some technologies that help with data processing.

## PySpark

The Python library for **Apache Spark**, `pyspark` (`https://spark.apache.org/`), is used to process and analyze data, especially of a large volume. **Spark** is a framework that is used to handle big data (data with variety, volume, and velocity) and is more effective than **Hadoop** (`https://hadoop.apache.org/`), a framework for parallel processing, scheduling, and resource management.

Apache Spark is famous for its distributed processing system, data processing, effective clustering of the environment, CLI applications, and parallel processing for large volumes of data. It is based on Hadoop’s **MapReduce**, with additional optimization for fast computing, and is also available for various programming languages, such as C#, Java, and Python.
