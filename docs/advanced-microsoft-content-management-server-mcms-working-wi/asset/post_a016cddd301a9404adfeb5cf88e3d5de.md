
## 12 MCMS and RSS

The Web has been changing over the past few years. Gone are the days when you must browse to a website to see it has changed. You may browse to a website to get an overall view of the content but instead of taking time out of your busy day to check each of your favorite websites, why not aggregate all the changes automatically so you can quickly scan for relevant content? Really Simple Syndication (RSS), an XML format, has been around for several years and it provides the foundation of providing content and updates to users.

RSS already has wide adoption in the Weblog (blog) community and now corporate and community sites are catching on to this way of increasing content coverage through syndication. The effectiveness of a Content Management System can generally be gauged by the amount of time it takes to maintain content along with the time it takes users of the website to find the content changes. If users are notified of content changes without having to browse back to the website, we reduce the time it takes for users to view new information.

### Looking Ahead

In this chapter, we will create and configure an RSS feed that syndicates the latest content to users of the Tropical Green website. We will also create an RSS aggregator placeholder that can be configured by authors to consume RSS feeds and display their contents.

Overleaf is a screenshot of the structure of the RSS feed we will be generating in this chapter.

![](img/442e26bf28f2df81fbf8dff3478f88e3_406_0.png)

The screen below is the authoring interface for the RSS Aggregation control we will also be building. It allows the user to enter the URL of the RSS feed to display, and configure aspects of the presentation mode view.

![](img/442e26bf28f2df81fbf8dff3478f88e3_406_1.png)

In presentation mode, our RSS Aggregation control will display each article in the RSS feed and depending on configuration the Open Article link, description, date, and author, as in the following screenshot:

![](img/442e26bf28f2df81fbf8dff3478f88e3_406_2.png)

### The Structure of an RSS Feed

An RSS feed is an XML document that conforms to a certain specification. We'll be using RSS version 2.0, as described at `http://blogs.law.harvard.edu/tech/rss`. The following table outlines the elements that an RSS feed contains.

| Parent | Element | Description |
|---|---|---|
| *(this is the root element)* | `rss` | The top level element of the RSS feed. |
| `rss` | `channel` |  |
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

```xml
<rss version="2.0">
  <channel>
    <link>http://localhost</link>
    <title>Tropical Green's website</title>
    <copyright>© Tropical Green 2005</copyright>
    <description>The latest changes from the Tropical Green website.</description>
    <managingEditor>content@tropicalgreen.net</managingEditor>
    <webMaster>webmaster@tropicalgreen.net</webMaster>
    <generator>MCMS 2002 RSS Feed Generator</generator>
    <item>
      <title>A Walk Through Jenny's Garden</title>
      <link>http://localhost/www.tropicalgreen.net/Columns/AWalkThroughJennysGarden.htm</link>
      <pubDate>Thu, 17 Mar 2005 13:07:31 GMT</pubDate>
      <author>WinNT://DEVELOPERXP/Developer</author>
      <guid isPermaLink="false">{01C931CC-0AFC-4BF4-9CB7-FBA8F354D0C8}</guid>
      <description>You can publish pages directly from Word to MCMS using Authoring Connector</description>
    </item>
  </channel>
</rss>
```

### Providing Content as an RSS Feed

We will create a mechanism that will allow users to point their RSS aggregator to a page and retrieve the postings that have been updated within a set number of days. An RSS aggregator is a program that can retrieve items from one or more RSS feeds and display the items in a readable form.

### Creating the RSS Feed

For the purposes of this example, we will create a new web form in the Tropical Green sample site.

Let's jump right into the code:

- 1. Launch the Tropical Green solution in Visual Studio .NET.
- 2. Create a new folder to store our RSS web form by right-clicking on the TropicalGreen project and selecting Add | New Folder. Name the new folder UtilityPages.
- 3. Create a new web form in the UtilityPages folder by right-clicking on UtilityPages and selecting Add | Add New Web Form.
- 4. Name the item rss.aspx and click Open.
- 5. We should now have a blank web form created and opened in Design view.

The web form we are creating will return XML and not HTML, so we need to remove the generated elements from the rss.aspx file as they will not form part of our RSS XML document.

- 1. At the bottom left of the screen, Click HTML to switch to HTML mode.
- 2. Delete everything below the Page directive:

```aspx
<%@ Page language="c#" Codebehind="rss.aspx.cs" AutoEventWireup="false" Inherits="TropicalGreen.UtilityPages.rss" %>
```

- 3. As many different aggregators can hit your feed in an automated manner, the impact of the feed on the overall site performance could be huge. The frequency with which aggregators will poll the feed for changes cannot be controlled by us as the setting is usually configurable on the aggregator itself. To minimize this impact on site performance, we will implement output caching for the web form serving the feed. Add the following directive just below the Page directive to set output caching to 300 seconds, or five minutes:

```aspx
<%@ OutputCache Duration="300" VaryByParam="none" %>
```

- 4. Switch to code view for the web form, and we'll start adding the code that generates our RSS feed. Import the following namespaces:

```csharp
using System;
using System.Collections;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Web;
using System.Web.SessionState;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.HtmlControls;
using System.Configuration;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
using System.IO;
using System.Xml;
using System.Text;

namespace TropicalGreen.UtilityPages
{
    /// <summary>
    /// Summary description for rss.
    /// </summary>
    public class rss : System.Web.UI.Page
    {
        - - - code continues - - -
    }
}
```

Let's create the GenerateRssXml() method, which returns the latest updates from our MCMS website as a string in RSS format. First we set up the temporary memory stream and XmlTextWriter needed to create the XML stream for sending to the client:

```csharp
/// <summary>
/// Generate the RSS feed xml
/// </summary>
/// <returns>RSS XML</returns>
private string GenerateRssXml()
{
    // Create the Memory Stream
    MemoryStream _MemoryStream = new MemoryStream();

    // Create an XmlTextWriter to write to the memory stream
    XmlTextWriter XmlWriter = new XmlTextWriter(_MemoryStream, Encoding.UTF8);

    // Create the top element <rss>
    XmlWriter.WriteStartElement("rss");

    // Set the version attribute of the RSS Feed
    XmlWriter.WriteAttributeString("version", "2.0");

    // Create the Channel Element
    XmlWriter.WriteStartElement("channel");
```

The general information for the RSS feed, such as link, title, and copyright, will be read from the Web.config file, and we'll create these Web.config entries later. GenerateRssXml() relies on two helper methods that we will develop shortly, GetPostingSummary(), which will retrieve the summary of a posting (either an HtmlPlaceholder or the Description property), and GetAccountFriendlyName(), which is designed to return the author's full name.

```csharp
// Get the general info from the Web.config
string strFeed_Title = ConfigurationSettings.AppSettings.Get("RSS_Title");
string strWebsiteUrl = ConfigurationSettings.AppSettings.Get("RSS_WebsiteUrl");
string strFeed_Copyright = ConfigurationSettings.AppSettings.Get(
    "RSS_Copyright");
```
