
# 文档中各元素

JSON 计数：4

```
```json
{
  "direction": "horizontal",
  "contents": [
    {
      "type": "paragraph",
      "markdown": "...",
      "bbox": [0.1, 0.1, 0.5, 0.2]
    }
  ]
}
```
```

The minimum valid code to embed audio in your Joomla! Article looks something like this:

```
<object width="350" height="50">
<param name="src" value="audio/sound.mp3">
<param name="autoplay" value="true">
<param name="controller" value="true">
<param name="bgcolor" value="#F4F4F4">
<embed src="audio/sound.mp3" autostart="true" loop="false" width="350" height="50" controller="true" bgcolor="#F4F4F4">
</embed>
</object>
```

Let's break down the elements of this code:

| object | Used to include multimedia in a web page. |
| height and width | Used to define the height and width of the object. |
| param | Controls the parameters of the object. |
| src | The source of the file. |
| autostart | Set to true or false in order to start playing the audio automatically. |
| controller | Set to true or false in order to show or hide the controller. |
| bgcolor | Used to set the background color of the player. |
| embed | Use the embed tag to place the player on the page. |
| pluginspage | Takes the user to a plugins download page where they can download the specific player. |

You will notice that the content within the <embed> tag is similar to some of that within the <object> tag. This is to cater for all browsers.

The previous code will embed the MP3 file into our Joomla! Article. This is assuming the audio track is called sound.mp3 and has been saved as an MP3 file in a directory named audio, which resides at the root level of our Joomla! file structure (at the same top level that the images directory does).

![](img/0151410972a5c5ea0aab26416e44b1da_143_0.png)

The previous code will use the user's default media player (that resides on their computers) to play the audio file. This is OK, but does not provide flexibility in player behaviors. Therefore, we can manipulate which player the audio file plays in by specifying a player in our code.

There are numerous Flash-based MP3 players now available; one I particularly like is from http://www.flash-mp3-player.net which is a powerful and free Flash MP3 Player containing some great features. There is even an easy code generator feature on the website which allows you to easily generate the correct code to embed the player into your web page. If you use the flash-mp3-player as an audio player, the HTML code looks similar to this:

```
<object type="application/x-shockwave-flash" data="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" width="200" height="20">
<param name="movie" value="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" />
<param name="bgcolor" value="#ffffff" />
<param name="FlashVars" value="mp3=audio/sound.mp3" />
</object>
```

> Audio players such as the Flash MP3 Player provide a tool for playing your external audio media. The path to your audio file, the filename, and its permissions will need to be set correctly for your audio files to play.

![](img/0151410972a5c5ea0aab26416e44b1da_144_0.png)

This code will produce a slick Flash-based audio player in your Joomla! Article, with minimal effort and code knowledge required. As mentioned, there are numerous players and online services around which contain their own features. The majority of these will be Flash-based players, because nearly 98 percent of web users' computers and browsers contain the Flash player.

If you are looking to manipulate your HTML audio code further, there are many parameters that can reside within the <object> tag. Unfortunately there are too many to cover in this book. For a list of more detailed parameters, visit the W3 Schools website at http://www.w3schools.com/TAGS/tag_object.asp.

## Including audio using a Joomla! Plugin

Now that we have done the hard work of including an audio file into our Joomla! Article using raw HTML code, let's look at the power of Joomla! Audio Extensions.

Joomla! Audio Extensions provide a diverse range of extended functionality to the core CMS. Once a plugin is installed and configured, adding audio can be a simple "few clicks" process. Specific Joomla! Audio Extensions will be highlighted later in this chapter, but an example below will show you how powerful they can be to administrators.

The extension chosen for this example is the JosDewplayer:

![](img/0151410972a5c5ea0aab26416e44b1da_145_0.png)

JosDewplayer is a GPL licensed MP3 audio player for Joomla!. The JosDewplayer utilizes a Flash-based player interface, which takes MP3 files and streams them in your HTML pages. The files can reside on your server, or remotely, and the latest version offers support for multiple file playback.

To get the JosDewplayer for Joomla!, visit http://extensions.joomla.org/ and search for "JosDewplayer".

> Before installing any third-party code into your Joomla! website, make sure you take a backup of your site database and file set before installing extensions.

## Using Audio in Your Joomla! Website

Log into your site Administration and click on **Extensions | Extension Manager** in the menu at the top:

![](img/0151410972a5c5ea0aab26416e44b1da_146_0.png)

Use the **Choose File** upload button to browse your local computer and find the `josdewplayer_plugin.zip` file. Once selected, use the **Upload File & Install** button to install this extension into your Joomla! website.

Once successfully installed, you should head over to **Extensions | Plugin Manager** to configure the new plugin. This particular plugin is classed as a **content** plugin, so you will be looking under that type for "JosDewplayer". Click on the text to load the Plugin Edit page:

![](img/0151410972a5c5ea0aab26416e44b1da_146_1.png)

The set of fields on the left-hand side allow you to rename the plugin if you wish to do so, enable or disable the plugin, and set access levels for your site users. Underneath these is the **Description** field, which contains basic instructions to use this plugin. On the right-hand side, a set of parameters allow you to customize the **Autoplay, Background color, and Autoreplay** features. **Plugin Parameters** usually only need to be set once, and for our example we will be publishing the plugin and leaving the other parameter settings as they are.

![](img/0151410972a5c5ea0aab26416e44b1da_146_2.png)

> Make sure your plugin is enabled for it to work.

![](img/0151410972a5c5ea0aab26416e44b1da_147_0.png)

Let's head back to our Joomla! Article that we created earlier, the one with the raw HTML audio code and MP3 player code inside it. Now that we have our extension doing all of the work, all we need to enter into the article is a special tag which the plugin will recognize — `{play}` —, and then a URL link to our audio file, and then close the special tag again — `{/play}`. Enter the following code into your Joomla! Article:

```
```text
{play}/audio/sound.mp3{/play}
```
```

In order to make an absolute link to the file, use the following code (replace the link with the full URL to your audio file):

```
```text
{play}http://localhost/Joomla1.6/audio/sound.mp3{/play}
```
```

You can use either a relative link or an absolute link, depending where your audio file resides. The absolute link is useful if you are linking to external audio content, and this is one nice feature of the JosDewplayer.

The Autoplay, Background color, and Autoreplay features, set at a global level in the Plugin Parameters, can be overwritten with each instance of your tag usage. For example, to change the default color, we could use the following code:

```
```text
{play}audio/sound.mp3|#000000 [BGCOLOR]{/play}
```
```

The audio player will now look like:

![](img/0151410972a5c5ea0aab26416e44b1da_147_1.png)

A list of the available parameters is located in the **Description** field of the plugin's parameters. As long as your path to the audio file is correct, we can place an audio player into our Joomla! Article in seconds. Now view the frontend of your website in order to see your audio playing through the JosDewplayer plugin. You can use the player in your site as many times as you want just by inserting the simple tags specific to this plugin. Adding audio to your site pages is that simple when using an audio plugin for Joomla!.
