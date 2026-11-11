
```
<select name="query">
<?php foreach ($songsArray as $key => $value) { ?>
    <option value="<?= $value ?>"><?= $value ?></option>
<?php } ?>
</select>
<input type="submit" value="Go" />
</form>
</html>
```

Again, before we start with the actual HTML, we use a PHP echo to declare the encoding on this page. In the HTML, we create the form. The form will use a GET method to pass the selected option value to the content.php page targeted for the Content frame. We use $songsArray, which we previously populated to create the select menu.

Finally, an input button is added to trigger the load. The form and html tags are then closed.

## Content Page

Our content page, named content.php, is loaded when the form on the navigation page is submitted. The form sends the artist and song title to content.php via a query parameter named query. content.php will have to take this parameter, pass it to YouTube's Web API, and display the results.

```
<?php

require_once ('Services/YouTube.php');

//YouTube parameters
$devId = 'YOUR OWN YOUTUBE DEVELOPER ID';
$tag = $_GET['query'];

//YouTube Result Parameters
$videosArray = array();
$firstVideo = null;
```

We start off with some basic initialization. We pull in the Services_YouTube package, set up the $dev_id variable, which holds our YouTube Developer ID, and set up a variable named $tag which holds the GET parameter received when the page is called.

Next, an array is set up to hold the video return results. A variable named $firstVideo is declared. A query can, and often does, return multiple results, with the most relevant result returned at the top. $firstVideo will hold this most relevant hit.

```
$youtube = new Services_YouTube($devId, array('usesCache' => true));
$videos = $youtube->listByTag($tag);

$videosArray = $videos->xpath('//video');
$firstVideo = $videosArray[0];
```

The first three lines of this block operate just like the Services_YouTube example: a Services_YouTube object is declared, the service is called, and the results are queried through XPATH.

We now have an array of all results. As we're only interested in the most relevant, we capture the one at index position 0 and place it into the $firstVideo array. Our setup PHP code is now completed, and we can begin the HTML.

```
?> 
<?= '<?xml version="1.0" encoding="UTF-8" ?>' ?> 
<html>
<head><title>Content</title></head>
<body>
<h1>YouTube Query Results for <?= $_GET['query'] ?></h1>
<?php if ($firstVideo) { ?>
```

In the HTML, we encounter a PHP if statement. This if statement checks to see if the $firstVideo array actually has anything in it. We check to see if the object exists.

```
Title: <?= $firstVideo->title ?><br />
<object width="425" height="350">
<param name="movie" value="http://www.youtube.com/v/<?= $firstVideo->id ?>"></param>
<param name="wmode" value="transparent"></param>
<embed src="http://www.youtube.com/v/<?= $firstVideo->id ?>" type="application/x-shockwave-flash" wmode="transparent" width="425" height="350"></embed>
</object>
<p>
<a href="<?= $firstVideo->url ?>"><?= $firstVideo->title ?>'s YouTube Page</a>
</p>
```

If the object exists, we use it to create the display page. The video object has three properties that we use in this section:

- title is the YouTube title.
- id is the YouTube unique identifier.
- url is the URL to the YouTube page.

The id is particularly important. Each video's page on YouTube has sample code you can use to copy and paste into your own web page. This will embed the video into the page and play it within the page. The sample code involves an object, two param tags, and an embed tag. We have copied this code into our application. However, we substitute the hard-coded example ids with our object's id property.

```
<?php } else { ?>
    No Results Found
<?php } ?>
</body>
</html>
```

We add an else block to display a, "No Results Found", message if YouTube returns nothing from the query. Finally, we close the body and html tags.

Place these three files into the same, web-server accessible directory on a web server. You can start using the mashup by launching jukebox.php in your web browser.

## Using the Mashup

Using the mashup is quite simple. Load jukebox.html into your web browser.

![](img/c637937d44400f1da21fb7cca101de80_174_0.png)

When you load `jukebox.php`, you will see a page similar to the one shown on the previous page. `navigation.php` will load, and the select menu will be created from our Last.fm feeds.

![](img/c637937d44400f1da21fb7cca101de80_175_0.png)

## Your Own Video Jukebox

Click on the menu. You will see a list of the songs from the two feeds. Select one and press the Go button.

![](img/c637937d44400f1da21fb7cca101de80_176_0.png)

My first selection, a song by Tangerine, yielded no results. We see the error message in the content pane. This isn't surprising because they are a small, up and coming local band based in Pittsburgh, Pennsylvania. Once they become more popular, their legions of fans will grow and hopefully someone will put something up on YouTube.

Let's select a more well-known band and song.

![](img/c637937d44400f1da21fb7cca101de80_176_1.png)

Our next selection, Atmosphere by Joy Division, was more successful. In this case, someone uploaded Atmosphere's music video. Pressing on the Play button will start playing the video without leaving our mashup.

The great thing about this mashup is the user-driven nature of YouTube. You can't be sure of what will be returned. In Atmosphere's case, we saw the official music video that someone uploaded. In many other cases, we get to see rare live performances that someone recorded with a camcorder and uploaded. Sometimes we'll see some creative minds who made their own music video and set it to the music that we queried. Other times, the video may have nothing to do with the song at all, except that it plays in the background. For example, when I submitted The Clash's classic Death or Glory, the only video that was returned was a tribute slideshow of, of all people, Harry Potter star Daniel Radcliffe that someone created.

## Summary

In this mashup, we used two different web APIs – one from video repository site YouTube, and the XML feeds from social music site Last.fm. We took a look at three different XML-based file formats from those two sites: XSPF for song playlists, RSS for publishing frequently updated information, and YouTube's custom XML format. We created a mashup that took the songs in two Last.fm feeds and queried YouTube to retrieve videos based on the song.

If we were to create our own XML-based parsers to parse the three formats, this would have taken much more time than it actually did. We found that the PHP Extension and Application Repository, PEAR, already had parsers we could use; one for each of the three formats. Using these PEAR packages, we were able to create an object-oriented abstraction of these formats, which allowed us to easily finish our application.

## 5

## Traffic Incidents via SMS

## Project Overview

|  |  |
|---|---|
| **What** | Deliver content from the California Highway Patrol Live Incidents website to mobile users through Short Messaging Service (SMS) |
| **Protocols Used** | Not applicable |
| **Data Formats** | RSS |
| **Tools Featured** | PHP's DOM Extensions |
| **APIs Used** | 411Sync |

And, on this project, we will learn how to use the dark arts. No, I'm not talking about controlling mindless zombies through black magic. I'm talking about screen-scraping.

In this project, we will screen-scrape from the California Highway Patrol website. The CHP maintains a website of traffic incidents. This site auto-refreshes every minute, ensuring the user gets live data about accidents throughout the state of California. This is very valuable if you are in front of a computer. If you are out and about running errands, it would be useless unless you had an Internet-enabled personal digital assistant. Even then, the site uses JavaScript and frames, and may not work on many PDA web browsers. More widely available than Internet PDAs are cell phones with text messaging through Short Messaging Service (SMS) capabilities. Our mashup will make the CHP data available through SMS instead of a web browser.
