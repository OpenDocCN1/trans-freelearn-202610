Let's look at some of the actions we can take to improve performance once we have identified the issue. This should not be taken as an exhaustive list, but rather as pointers that can help you quickly improve the performance of your application. For example, multiple actions can be taken to improve Visualforce page performance, but I mention only a couple of them in the following sections.

# Browser caching

This is a simple setting from the UI that can significantly improve page performance by avoiding extra server calls by securely caching the most frequently used data on the client, that is, a browser. This setting is turned on by default and Salesforce discourages turning it off. Some exceptions to this include situations where the company’s security policy doesn’t allow the caching of secure data, and in sandboxes when you want to immediately see the effects of your configuration or code changes without emptying the cache.

# Using a Content Delivery Network

A **Content Delivery Network (CDN)** is a collection of server-based infrastructure that speeds up the performance of websites. When a website is loaded, the images and other content hosted with the website get loaded as well. If you are accessing the website from a different continent than where the website is hosted, you will see latency and a delayed response compared to a user located within the same city or region as the web hosting.

With a CDN, the content is copied to servers across the CDN, which then serves from the closest location to the customer visiting the website. This helps in reducing response times to the customer and provides a great customer experience. In Salesforce, static content from the Lightning Component framework and other applications is served from Akamai’s CDN servers located across the world. This results in improved application load times as static assets, including image files, **CSS (Cascading Style Sheets)**, JS, and font files, and other libraries are cached and served to the user from the most appropriate CDN location. Using a CDN doesn’t cache any metadata or data in your org.

# Network issues

Once you have run some basic commands such as `traceroute` or `ping` and notice something that requires further investigation, involve your networking team right away to perform a more in-depth investigation. Don’t delay involving your team if you have gone through all the things related to Salesforce and have ruled out the fact that a Salesforce-related issue is causing performance issues because that will delay the incident resolution process. The networking team will need time as they won’t have answers right away and will need to troubleshoot on their end, and only once they have found the root cause can they then start the remediation process.