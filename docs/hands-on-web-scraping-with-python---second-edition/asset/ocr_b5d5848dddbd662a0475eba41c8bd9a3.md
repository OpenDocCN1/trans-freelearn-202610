In this section, we have explored various types of data that are found through APIs. Nested, unnested, key:value pairs, and combined output are generally expected from web APIs. Apart from this, API-related links are also difficult to find, until and unless the website provides them, otherwise pages such as robots.txt and sitemap.xml, as well as the DevTools, are there to be thoroughly investigated.

In the next section, we will use some freely available web APIs. We will use Python to scrape their contents.

# Web scraping using APIs

Technically, obtaining data from APIs is easy, and is also different from the web scraping scenarios. Most of the data is in JSON format, and there is no use of XPath, CSS Selector, or any other parsing libraries. Some data found using APIs might contain a lot of HTML code. To deal with this content, XPath and CSS Selector might be required.

Important note
In the examples in this section, we have tried to omit APIs that require an API key (walmartlabs.com, nasa.gov, nytimes.com, maps.googleapis.com) or user authentication tools. An API key is an authenticated value provided by service providers (after user registration) that identifies the user using the API. Freely available APIs are being used in these examples.

With the help of a few examples, we will collect data returned via some APIs. This collected data will then be exported as JSON and CSV files.

# Example 1 – holidays from the US calendar

https://date.nager.at/ contains information on worldwide public holidays. https://date.nager.at/PublicHoliday/Country/US displays a list of public holidays in the US. Tabular data is available for scraping, but we are going to use the https://date.nager.at/api/v3/PublicHolidays/2023/US API for the holidays in the year 2023.

The following code declares apiUrl with the content URL:

```
apiUrl= "https://date.nager.at/api/v3/PublicHolidays/2023/US"
headers= {          # HTTP Request Header
'accept':'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif, image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
'accept-language': 'en-US,en;q=0.9',
'cache-control':'max-age=0',
'upgrade-insecure-requests':'1',
'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36'
}
response = requests.get(apiUrl, headers=headers).json()
```