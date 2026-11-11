
| Playlist Child Element | Required? | Description |
|---|---|---|
| identifier | No | The unique ID for the playlist. Must be a legal Uniform Resource Name (URN). |
| image | No | A URL to an image representing the playlist. |
| date | No | The creation (not the last modified!) date of the playlist. Must be in XML schema dateTime format. For example, "2004-02-27T03:30:00". |
| license | No | If the playlist is under a license, the license is specified with this element. |
| attribution | No | If the playlist is modified from another source, the attribution element gives credit back to the original source, if necessary. |
| link | No | Allows non-XSPF resources to be included in the playlist. |
| meta | No | Allows non-XSPF metadata to be included in the playlist. |
| extension | No | Allows non-XSPF XML extensions to be included in the playlist. |

A `trackList` element has an unlimited number of `track` elements to represent each track. `track` is the only allowed child of `trackList`. `track`'s child elements give us information about each track. The following table summarizes the children of `track`:

| Track Child Element | Required? | Description |
|---|---|---|
| location | No | The URL to the audio file of the track. |
| identifier | No | The canonical ID for the playlist. Must be a legal URN. |
| title | No | A human readable title of the track. Usually, the song's name. |
| creator | No | The name of the track creator. Usually, the song's artist. |
| annotation | No | Comments on the track. |
| info | No | A URL to a page containing more information about the track. |
| image | No | A URL to an image representing the track. |
| album | No | The name of the album that the track belongs to. |
| trackNum | No | The ordinal number position of the track in the album. |
| duration | No | The time to play the track in milliseconds. |
| link | No | Allows non-XSPF resources to be included in the track. |
| meta | No | Allows non-XSPF metadata to be included in the track. |
| extension | No | Allows non-XSPF XML extensions to be included in the track. |

Note that XSPF is very simple and track oriented. It was not designed to be a repository or database for songs. There are not a lot of options to manipulate the list. XSPF is merely a shareable playlist format, and nothing more.

## RSS

The simplest answer to, "What is RSS?", is that it's an XML file used to publish frequently updated information, like news items, blogs entries, or links to podcast episodes. News sites like Slashdot.org and the New York Times provide their news items in RSS format. As new news items are published, they are added to the RSS feed. Being XML-based, third-party aggregator software makes reading news items easy. With one piece of software, I can tell it to grab feeds from various sources and read the news items in one location. Web applications can also read and parse RSS files. By offering an RSS feed for my blog, another site can grab the feed and keep track of my daily life. This is one way by which a small site can provide rudimentary web services with minimal investment.

The more honest answer is that it is a group of XML standards (used to publish frequently updated information like news items or blogs) that may have little compatibility with each other. Each version release also has a tale of conflict and strife behind it. We won't dwell on the politicking of RSS. We'll just look at the outcomes. The RSS world now has three main flavors:

- The RSS 1.0 branch includes versions 0.90, 1.0, and 1.1. Its goal is to be extensible and flexible. The downside to the goals is that it is a complex standard.
- The RSS 2.0 branch includes versions 0.91, 0.92, and 2.0.x. Its goal is to be simple and easy to use. The drawback to this branch is that it may not be powerful enough for complex sites and feeds.

There are some basic skeletal similarities between the two formats. After the XML root element, metadata about the feed itself is provided in a top section. After the metadata, one or more items follow. These items can be news stories, blog entries, or podcasts episodes. These items are the meat of an RSS feed.

The following is an example RSS 1.1 file from XML.com:

```
<Channel xmlns="http://purl.org/net/rss1.1#"
    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    rdf:about="http://www.xml.com/xml/news.rss">

<title>XML.com</title>
<link>http://xml.com/pub/</link>
<description>
    XML.com features a rich mix of information and services for the
    XML community.
</description>
<image rdf:parseType="Resource">
    <title>XML.com</title>
    <url>http://xml.com/universal/images/xml_tiny.gif</url>
</image>

<items rdf:parseType="Collection">
    <item rdf:about=
        "http://www.xml.com/pub/a/2005/01/05/restful.html">
        <title>
            The Restful Web: Amazon's Simple Queue Service
        </title>
        <link>
            http://www.xml.com/pub/a/2005/01/05/restful.html
        </link>
        <description>
            In Joe Gregorio's latest Restful Web column, he explains
            that Amazon's Simple Queue Service, a web service offering a
            queue for reliable storage of transient
            messages, isn't as RESTful as it claims.
        </description>
    </item>

    <item rdf:about=
        "http://www.xml.com/pub/a/2005/01/05/tr-xml.html">
        <title>
            Transforming XML: Extending XSLT with EXSLT
        </title>
        <link>
            http://www.xml.com/pub/a/2005/01/05/tr-xml.html
        </link>
    <description>
        In this month's Transforming XML column, Bob DuCharme
        reports happily that the promise of XSLT extensibility via
        EXSLT has become a reality.
    </description>
    </item>
</items>
</Channel>
```

The root element of an RSS file is an element named Channel. Immediately, after the root element are elements that describe the publisher and the feed. The title, link, description, and image elements give us more information about the feed.

The actual content is nested in the items element. Even if there are no items in the feed, the items element is required, but will be empty. Usage of these elements can be summarized as follows:

| Channel Child Element | Required? | Description |
|---|---|---|
| title | Yes | A human readable title of the channel. |
| link | Yes | A URL to the feed. |
| description | Yes | A human readable description of the feed. |
| items | Yes | A parent element to wrap around item elements. |
| image | No | A section to house information about an official image for the feed. |
| others | No | Any other elements not in the RSS namespace can be optionally included here. The namespace must have been declared earlier, and the child elements must be prefixed. |

> If used, the image element needs its own child elements to hold information about the feed image. A title element is required and while optional, a link element to the actual URL of the image would be extremely useful.

Each news blog, or podcast entry is represented by an item element. In this RSS file, each item has a title, link, and a description, each, represented by the respective element. This file has two items in it before the items and Channel elements are closed off.

Note the use of the rdf namespace. The RSS 1.0 branch uses Rich Description Framework (RDF) extensively. RDF is an XML framework to make documents not only machine-friendly, but also human-friendly by organizing topics together. To consume RSS 1.0, we do not need to know too much about RDF. However, we will be taking a little closer look at it on a future project. For now, the key concept to take away is that individual items are represented by the item element in the RSS 1.0 branch.

The item element's children are summarized as follows:

| Item Child Element | Required? | Description |
|---|---|---|
| title | Yes | A human readable title of the item. |
| link | Yes | A URL to the item. |
| description | Yes | A human readable description of the item. |
| others | No | Any other elements not in the RSS namespace can be optionally included here. The namespace must have been declared earlier, and the child elements must be prefixed. |

Looking at a RSS 2.0 feed from IBM DeveloperWorks, we can see a lot of similarities:

```
<rss version="2.0">
<channel>
<title>developerWorks : Linux : Technical library</title>
<link>http://www.ibm.com/developerworks/index.html</link>
<description>
The latest content from IBM developerWorks
</description>
<pubDate>Wed, 10 Jan 2007 01:03:11 EST</pubDate>
<language>en-us</language>
<copyright>Copyright 2004 IBM Corporation.</copyright>
<image>
<title>IBM developerWorks</title>
<url>
http://www-106.ibm.com/developerworks/i/dwlogo-small.gif
</url>
<link>
http://www.ibm.com/developerworks/index.html
</link>
</image>
<item>
<title>
```
