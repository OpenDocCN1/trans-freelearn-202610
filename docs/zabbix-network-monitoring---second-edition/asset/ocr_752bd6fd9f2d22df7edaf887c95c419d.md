+   - **Type of information:** Text
+   - **New application:** OSM

> We are creating an agent item for our Zabbix server. This means that this web item will be checked by the local agent.

When done, click on the Add button at the bottom. In this item, we specified / as the second parameter, but that is optional — by default, root on the web server is requested. If a custom port has to be used, it can be specified as the third parameter, like this:

```
web.page.get[www.site.lan,/,8080]
```

Instead of checking the results of each of the items we are creating individually, let's create all three items first and then verify the results.

# Checking page performance

Another web page-related agent item is web.page.perf. It returns the loading time of the page in seconds. While still in the item list, click on Create item, and fill in the following:

+   - **Name:** OpenStreetMap main page load time
+   - **Key:** web.page.perf[www.openstreetmap.org,/]
+   - **Type of information:** Numeric (float)
+   - **Units:** s
+   - **Applications:** OSM

When done, click on the Add button at the bottom. We changed Type of information as this item key returns the time it took to load the page in seconds, and that value usually will have a decimal part.

# Extracting content from web pages

When creating the web monitoring scenario earlier, we used the ability to extract content from a page and reuse it later. With the more simple agent monitoring, it is still possible to extract some content from a page. As a test, we could try to extract the text after OpenStreetMap is and up to the first comma. Click on Create item again, and fill in the following:

+   - **Name:** OpenStreetMap is
+   - **Key:** web.page.regexp[www.openstreetmap.org,,,OpenStreetMap is ([a-z ]*),,,\1]