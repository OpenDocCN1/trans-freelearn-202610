| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| OSM (3 items) | | |
| OpenStreetMap is | 2016-05-13 13:41:21 | a map of the world |
| OpenStreetMap main page | 2016-05-13 13:41:20 | HTTP/1.1 200 OK Date: Fri, 13 May 2016 10:41:20 GMT ... |
| OpenStreetMap main page load time | 2016-05-13 13:41:21 | 604.8ms |

> Each item requests the page separately.

The items should be returning full page contents, the time it took to load the page, and the result of our regular expression. The web.page.get item always includes headers, too. If you see empty values appearing every now and then in the web.page.get and web.page.regexp items, it probably happens because of the request timing out. While web scenarios had their own timeout setting, the agent items obey the agent timeout of 3 seconds by default. The web.page.perf item returns 0 upon a timeout.

> The Zabbix web.page.get item currently does not work properly with chunked transfer encoding, which is widely used. Extra data is inserted in the page contents. This was expected to be fixed in Zabbix 3.0 by using libcurl for these agent items as well, but that development was not finished. At the time of writing this, it is not known when this could be fixed.

Using these items, we could trigger when a page takes long to load or does not work at all or when a specific string cannot be found on the page — using str() and similar trigger expressions either on the whole page item or on the content extraction item.

> Web scenarios are executed on the Zabbix server, agent items on the agent. We will discuss the ability of running web scenarios on remote systems in Chapter 19, Using Proxies to Monitor Remote Locations.

The items we created all went to the same Zabbix agent. We can also create a host with multiple interfaces and assign items to each interface. This allows us to check a web page from multiple locations but keep the results in a single host. We still have to make the item keys unique — if needed, either use the trick with empty key parameters, extra commas in key parameters, or key aliasing, discussed in Chapter 22, Zabbix Maintenance. Note that templates can't be used in such a setup.