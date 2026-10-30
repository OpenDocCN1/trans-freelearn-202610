```
try:
    http_rsp = urlopen(link)
    print(http_rsp)
    html = http_rsp.read()
    print(html)
    html_decoded = html.decode()
    print(html_decoded)
except Exception as ex:
    print('*** Failed to get Html! ***\n\n' + str(ex))
else:
    return html_decoded
```

You can read more about Python exception handling at https://docs.python.org/3.6/library/exceptions.html.

By calling urlopen on the official Python website, we get the entire data as one long string. The first print statement prints this long string out to the console.

We then call decode on the result, and this time we get a little over 1,000 lines of web data, including some whitespace.

We also print out the type for calling urlopen, which is an http.client.HTTPResponse object. Actually, we print it out first:

![](bbox=[0.1, 0.6, 0.95, 0.75])