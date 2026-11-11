
Next, we prepare to display the playlist. Before we do that, we extract some information about the playlist. The XSPF object's getTitle method returns the XSPF file's title element. getCreator returns the creator element of the file.

```
<?php foreach ($tracks as $track) { ?>
    <p>
        Title: <?= $track->getTitle() ?><br />
        Artist: <?= $track->getCreator() ?><br />
    </p>
<?php } ?>
</body>
</html>
```

Finally, we loop through the tracks array. We assign the array's elements, which are XSPF_Track objects, into the $track variable. XSPF_Track also has getTitle and getCreator methods. Unlike XSPF's methods of the same names, getTitle returns the title of the track, and getCreator returns the track's artist.

Running this file in your web browser will return a list populated with data from Last.fm.

![](img/c637937d44400f1da21fb7cca101de80_162_0.png)

## Services_YouTube

Services_YouTube works in a manner very similar to File_XSPF. Like File_XSPF, it is an object-oriented abstraction layer on top of a more complicated system. In this case, the system is the YouTube API.

Using Services_YouTube is a lot like using File_XSPF. Include the package in your code, instantiate a Services_YouTube object, and use this object's methods to interact with the service. The official documentation for the latest release of Services_YouTube is located at http://pear.php.net/package/Services_YouTube/docs/latest/. The package also contains online working examples at http://pear.php.net/manual/en/package.webservices.services-youtube.php.

Many of the methods deal with getting members' information like their profile and videos they've uploaded. A smaller, but very important subset is used to query YouTube for videos. We will use this subset in our mashup. To get a list of videos that have been tagged with a specific tag, use the object's listByTag method.

listByTag will query the YouTube service and store the XML response in memory. It does not return an array of video objects we can directly manage, but with one additional function call, we can achieve this. From there, we can loop through an array of videos similar to what we did for XSPF tracks.

The example file YouTubePearTest.php illustrates this process.

```
<?php
    require_once 'Services/YouTube.php';
    $dev_id = 'Your YouTube DeveloperID';
    $tag = 'Social Distortion';
    $youtube = new Services_YouTube($dev_id, array('usesCache' => true));
    $videos = $youtube->listByTag($tag);
?>
```

First, we load the Services_YouTube file into our script. As YouTube's web service requires a Developer ID, we store that information into a local variable. After that, we place the tag we want to search for in another local variable named $tag. In this example, we are going to check out which videos YouTube has for one of the greatest bands of all time, Social Distortion. Services_YouTube's constructor takes this Developer ID and uses it whenever it queries the YouTube web service. The constructor can take an array of options as a parameter. One of the options is to use a local cache of the queries. It is considered good practice to use a cache, as to not slam the YouTube server and run up your requests quota.

Another option is to specify either REST or XML-RPC as the protocol via the driver key in the options array. By default, Services_YouTube uses REST. Unless you have a burning requirement to use XML-RPC, you can leave it as is.

Once instantiated, you can call listByTag to get the response from YouTube. listByTag takes only one parameter—the tag of our desire.

Services_YouTube now has the results from YouTube. We can begin the display of the results.

```
<html>
<head>
<title>Social Distortion Videos</title>
</head>
<body>
<h1>YouTube Query Results for Social Distortion</h1>
```

Next, we will loop through the videos. In order to get an array of video objects, we first need to parse the XML response. We do that using Services_YouTube's xpath method, which will use the powerful XPATH query language to go through the XML and convert it into PHP objects. We pass the XPATH query into the method, which will give us an array of useful objects. We will take a closer look at XPATH and XPATH queries later in another project. For now, trust that the query //video will return an array of video objects that we can examine.

Within the loop, we display each video's title, a thumbnail image of the video, and a hyperlink to the video itself.

```
<?php foreach ($videos->xpath('//video') as $i => $video) { ?>
<p>
    Title: <?= $video->title ?><br />
    <img src='<?= $video->thumbnail_url ?>' alt='<?= $video->title ?>' />
<br />
    <a href='<?= $video->url ?>'>URL</a>
</p>
<?php } ?>
</body>
</html>
```

Running this query in our web browser will give us a results page of videos that match the search term we submitted.

![](img/c637937d44400f1da21fb7cca101de80_164_0.png)

> Note that before running YouTubePearTest.php file, you will have to install CURL.

### YouTube Query Results for Social Distortion

Title: social distortion - story of my life

![](img/c637937d44400f1da21fb7cca101de80_165_0.png)

URL

Title: Social Distortion Reach for the Sky

![](img/c637937d44400f1da21fb7cca101de80_165_1.png)

URL

Title: Social Distortion Don't Take Me for Granted

![](img/c637937d44400f1da21fb7cca101de80_165_2.png)

URL

## XML_RSS

Like the other PEAR extensions, XML_RSS changes something very complex, RSS, into something very simple and easy to use, PHP objects. The complete documentation for this package is at http://pear.php.net/package/XML_RSS/docs/XML_RSS.

There is a small difference to the basic philosophy of XML_RSS compared to Services_YouTube and File_XSPF. The latter two packages take information from whatever we're interested in, and place them into PHP object properties.

For example, File_XSPF takes track names into a Track object, and you use a getTitle() getter method to get the title of the track. In Services_YouTube, it's the same principle, but the properties are public, and so there are no getter methods. You access the video's properties directly in the video object.

In XML_RSS, the values we're interested in are stored in associative arrays. The available methods in this package get the arrays, then you manipulate them directly. It's a small difference, but you should be aware of it in case you want to look at the code. It also means that you will have to check the documentation of the package to see which array keys are available to you.

Let's take a look at how this works in an example. The file is named RSSPEARTest.php in the example code. One of Audioscrobbler's feeds gives us an RSS file of songs that a user recently played. The feed isn't always populated because after a few hours, songs that are played aren't considered recent. In other words, songs will eventually drop off the feed simply because they are too old. Therefore, it's best to use this feed on a heavy user of Last.fm.

RJ is a good example to use. He seems to always be listening to something. We'll grab his feed from Audioscrobbler:

```
<?php
    include ("XML/RSS.php");
    $rss =& new XML_RSS("http://ws.audioscrobbler.com/1.0/user/RJ/
                        recenttracks.rss");
    $rss->parse();
```

We start off by including the module and creating an XML_RSS object. XML_RSS is where all of the array get methods reside, and is the heart of this package. It's constructor method takes one variable—the path to the RSS file. At instantiation, the package loads the RSS file into memory.

parse() is the method that actually does the RSS parsing. After this, the get methods will return data about the feed. Needless to say, parse() must be called before you do anything constructive with the file.

```
$channelInfo = $rss->getChannelInfo();
?>
```

The package's getChannelInfo() method returns an array that holds information about the metadata, the channel, of the file. This array holds the title, description, and link elements of the RSS file. Each of these elements is stored in the array with the same key name as the element.

```
<?= "<?xml version=\"1.0\" encoding=\"UTF-8\" ?>" ?>
```
