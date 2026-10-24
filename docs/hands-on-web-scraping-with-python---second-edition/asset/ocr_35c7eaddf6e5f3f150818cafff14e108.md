with the internet. The curl command is machine-independent. Programming languages such as Python, PHP, C, R, Java, and Rust convert the raw curl commands into their preferred format, which can be used in their code or by their libraries.

pycurl, developed in the C programming language, provides curl-based facilities to Python code. The requests library is often considered easy to use, in comparison to other libraries. pycurl provides almost all of the same features and functionality as curl, including its attributes and methods.

Listed here are some of the major advantages that pycurl possesses over requests:

+   - Supports a large number of communication protocols (HTTPS, POP3, FTP, LDAP, IMAP, and many more)
+   - Faster in comparison to requests and other similar Python libraries

Modern browsers even provide the raw curl command, which can be found using browser-based developer tools, as mentioned earlier in Chapter 2. These curl commands can be copied for use with the following:

+   - **Command-line interface (CLI)** applications (Bash shells or scripts) or terminals
+   - Web-based terminals and API tools such as **Postman** (https://www.postman.com/)
+   - Python libraries such as requests (https://curlconverter.com/python/) with headers
+   - Converting a command into other programming languages

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