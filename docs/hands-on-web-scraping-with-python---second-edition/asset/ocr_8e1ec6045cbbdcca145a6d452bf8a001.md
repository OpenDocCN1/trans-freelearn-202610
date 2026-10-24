The server identifies the cookies’ values and communicates with the browser based on the information that is stored in the cookies. Data in cookies helps a website access and transfer values such as session ID, expiration date, and the time the cookie information was stored.

A session or web session is normally identified as information stored in the server (temporarily), and it persists throughout the user’s interaction with the site.

As shown in Figure 2.24, a session is often related to a user’s credentials on a website:

```
session_obj = requests.Session()
```

```
response_obj = session_obj.get('https://httpbin.org/cookies', cookies={'name': 'anishchapagain','date':'30-dec-2022'})
response_obj.text
```

'{\n  "cookies": {\n    "date": "30-dec-2022", \n    "name": "anishchapagain"\n  }\n}\n'

```
response_obj.headers
```

{'Date': 'Fri, 30 Dec 2022 09:24:12 GMT', 'Content-Type': 'application/json', 'Content-Length': '88', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}

```
response_test = session_obj.get('https://httpbin.org/cookies')
response_test.text
```

'{\n  "cookies": {}\n}\n'

Figure 2.24: HTTP requests with requests.Session() objects

# Important note

During an HTTP request, it is important that header, cookies, and session values obtained from DevTools or code output are monitored and updated or replaced with values such as proxy and header/cookie keys frequently. While running the scraping script, using these updated values and keys will help you bypass server exceptions and detect bots. Visit https://requests.readthedocs.io/en/latest/user/advanced/#advanced for more information on sessions and proxies.

You now have a basic idea of HTTP request attributes, such as cookies, headers, sessions, and methods. In the next section, we will focus on the contents of URLs or, more appropriately, HTTP responses.

# Response content

We can receive pages or responses, or the page source of the requested URL, using the content attribute in bytes, whereas the text attribute returns a string object, as displayed in Figure 2.25: