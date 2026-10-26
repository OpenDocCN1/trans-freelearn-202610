# The Structure of an RSS Feed

An RSS feed is an XML document that conforms to a certain specification. We'll be using RSS version 2.0, as described at `http://blogs.law.harvard.edu/tech/rss`. The following table outlines the elements that an RSS feed contains.

| Parent | Element | Description |
|---|---|---|
| *(this is the root element)* | `rss` | The top level element of the RSS feed. |
| `rss` | `channel` | |
| `channel` | `link` | The URL to the website. |
| `channel` | `title` | The title of the RSS Feed, such as Tropical Green. |
| `channel` | `copyright` | Copyright information pertaining to the RSS feed. |
| `channel` | `description` | The description of the RSS feed. |
| `channel` | `managingEditor` | The e-mail address for the person responsible for the content. |
| `channel` | `webMaster` | The e-mail address of the Webmaster. |
| `channel` | `generator` | The name of the application used to generate the feed. |
| `channel` | `item` | A repeating element for each article to be aggregated. |
| `item` | `title` | The title of the article. In this case we are using the `DisplayName` property of the posting. |
| `item` | `link` | The link to the HTML version of the article (or more information). |
| `item` | `pubDate` | The date the item was published. |
| `item` | `author` | The author's name or e-mail address for the item. |
| `item` | `guid` | A Globally Unique ID for the item. This element maps perfectly to MCMS as postings already have GUIDs. |
| `item` | `description` | The content that will be included for this item. |

Remember that as the RSS feed is XML, all element names are case sensitive. Below is a sample feed:

```
```xml
<rss version="2.0">
  <channel>
    <link>http://localhost</link>
    <title>Tropical Green's website</title>
    <copyright>© Tropical Green 2005</copyright>
    <description>The latest changes from the Tropical Green
      website.</description>
    <managingEditor>content@tropicalgreen.net</managingEditor>
    <webMaster>webmaster@tropicalgreen.net</webMaster>
    <generator>MCMS 2002 RSS Feed Generator</generator>
    <item>
      <title>A Walk Through Jenny's Garden</title>
      <link>http://localhost/www.tropicalgreen.net/Columns
        /AWalkThroughJennysGarden.htm</link>
      <pubDate>Thu, 17 Mar 2005 13:07:31 GMT</pubDate>
      <author>WinNT://DEVELOPERXP/Developer</author>
```
```