```
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(RssUrl);

// uncomment if you have a proxy
// WebProxy proxy = null;
// proxy = new WebProxy("http://proxyserver:port");
// request.Proxy = proxy;

// Get the Response from the web request
HttpWebResponse response = (HttpWebResponse)request.GetResponse();
```

The response is then loaded into an XmlTextReader. We read the contents of the XmlTextReader and generate the HTML for the different articles in the feed, such as the author's name for instance:

```
// load the stream into an XmlTextReader
XmlTextReader rsReader = new XmlTextReader(response.GetResponseStream());

int actItem = 0;

// Loop through the reader
while (!rsReader.EOF)
{
```

A feed can serve many articles. We need to add a check to ensure that only the configured number of articles is displayed:

```
// How many items are we going to display?
if (actItem >= this.ItemsToDisplay)
    break;
```

Now let's iterate through all XML tags of the current feed item and collect all the data for the current item. As different feeds use different standards (like RSS 1.0, 2.0, or Atom), the content we are looking for may be encoded in different tags:

```
// What is the current element of the RSS Feed
switch (rsReader.Name.ToLower())
{
```

The feed or channel element encloses the information about the current RSS feed:

```
case "feed":
case "channel":
{
    CurrentState = ennState.Channel;
    rsReader.Read();
    break;
}
case "title":
{
    if (rsReader.NodeType == XmlNodeType.Element)
        Title = HttpUtility.HtmlDecode(rsReader.ReadInnerXml());
    else
        rsReader.Read();
    break;
}
case "link":
{
    if (rsReader.NodeType == XmlNodeType.Element)
    {
        string Link1 = rsReader.GetAttribute("href");
        string Link2 = rsReader.ReadInnerXml();
        if (Link1 != null)
            Link = Link1;
        else
            Link = Link2;
    }
```