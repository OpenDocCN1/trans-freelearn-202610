In the next section, we will learn about an add-on feature (middleware feature) that can be used in web requests.

# Proxies

A proxy or proxy web server (also known as middleware) is one of the most important and core technologies in network communication, especially web scraping. Plenty of web security-related challenges arise in network communication during scraping, and using a proxy helps us to overcome them.

Using a proxy when scraping usually hides the original machine’s IP. Be aware, however, that some providers will not always hide the original IP. A proxy communicates with the target site using some other secure IPs that are available through proxy service providers. A proxy works as middleware and helps to bypass web security on physical machines. One of the most used proxy types in web scraping is rotating **residential proxies**, which change the IP address on every new request.

The internet is open content. Web servers impose a large number of security measures to keep their content safe. Many of you will have encountered some content that was blocked to view or access, which can be overcome using proxies. In some way or another, either on public or on-premises networks, proxies provide great security and filtering capabilities.

When dealing with web scraping for long-form content, such as daily blogs, reviews, stock information, and ratings, there are various recurring scraping tasks for fixed sites, as well as scheduled scraping options, including hourly intervals. However, it is important to consider the possibility of encountering web security measures that could potentially lead to blocking, such as the following:

+   - IP blacklisting
+   - Facing CAPTCHAs on every page
+   - The inability to use filtering options
+   - Redirecting to pages that are not targeted (such as those hosted in a different country or in a different language from that of the user)
+   - Redirecting to the first page only
+   - Redirecting to the main page

These kinds of things can lead to wasted time, financial loss, loss of disk and web space, collecting irrelevant data, reduced quality of data, and much more. To resolve this, developers need to use and implement web-based penetration testing, such as using proxies; selecting rotating IPs for a selected country, a random combination of proxies with **User-Agent**, or randomized or standard *request headers*; or executing `curl` commands in the terminal to verify content.

Listed here are some of the situations in which proxies can be helpful:

+   - Overcoming territory (border-based) restrictions
+   - Preventing blacklisting