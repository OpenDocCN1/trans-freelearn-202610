
## Embedding and displaying video in your Joomla! web pages

Besides all of the wonderful features that Joomla! provides out-of-the-box, one very powerful feature is the ability to include your own custom HTML into Joomla! Articles. This means we have the ability to completely customize the output of content in our articles and modules, which is what is required to insert a video into a Joomla! web page, without the use of extensions.

### The source

When preparing your video for the Web, one of the most significant choices is which video format to use.

Unfortunately, no single video format is 100 percent guaranteed to suit all of your site visitors. They will all be using different computer operating systems, screen resolutions, and web browsers, which means that consideration is required to allow your video to be seen by the majority of users, without the requirements for additional downloads or plugins.

We have already listed video formats earlier in this chapter. The three main players that make themselves noticed when providing support for these video formats are as follows:

| Adobe Flash | SWF movies |
| Apple QuickTime | MOV, MPEG-1/2/4, AVI (depending on the codec used) |
| Windows Media | WMV, MPEG-1, AVI (depending on the codec used) |

Adobe Flash claims they have a 98 percent presence in the market, and it is widely used as a format choice for video distribution sites such as Google, YouTube, and Yahoo!. If a high percentage of users can view Flash content, then this is one great reason to consider using Flash as a video format for your site.

There may be situations where installing Flash is restricted, such as on company computer networks. To cater for audiences such as this, making your content also available in an MPEG-4 video format may be a suitable option.

### The encoding

An essential step for web video is preparing it for web delivery. Your source material may often be in a different format than the one you decide to use, and if it has been recorded by a mobile phone, a DVD camcorder, among others, it will probably be in a very unsuitable format for use on the Web.

There are a number of video exporting and conversion software applications available now, and although it would be great to go into detail, whole books have been written about editing and optimizing videos. The following is some basic information that can be taken as a guide when exporting your movie from video editing software. These are ideal for users viewing your sites with a broadband connection:

| Adobe Flash format | Frame size and aspect ratio: (4:3) 384 x 480, (16:9) 270 x 480. |
|  | Frame rate: 12-15 frames per second. |
|  | De-interlace: Should be set to yes. |
|  | Audio bit rate: 64 Kbps (speech) to 128 Kbps (music). |
| Apple QuickTime format | Frame size and aspect ratio: (4:3) 640 x 480 or 320 x 240, (16:9) 640 x 352. |
|  | Frame rate: 15 frames per second. |
|  | De-interlace: Should be set to yes. |
|  | Audio bit depth: 16 bit. |
| Windows Media format | Frame size and aspect ratio: (4:3) 640 x 480 or 320 x 240, (16:9) 640 x 352. |
|  | Frame rate: 15 frames per second. |
|  | De-interlace: Should be set to yes. |
|  | Audio bit depth: 16 bit. |

Raw video will need to be encoded into a streaming format to produce an appropriate QuickTime, Windows Media, Flash, RealMedia, or MPEG-4 file. The length and time required to encode video is dependent on the export settings, software, and computer processing power. Many free and commercial software applications are now available to perform video editing, exporting, and file conversions.

An open source encoding tool worth mentioning is the MediaCoder from http://www.mediacoderhq.com/download.htm. MediaCoder is a free universal media transcoder that works with almost all popular codecs. MediaCoder can also transcode audio, providing you with a Swiss army knife for media encoding. A search on the Internet will provide you with editing tools to suit your computer platform, and there are lots available.

### The upload

Whether you choose to use FTP or the built-in Media Manager, it is an easy process to upload video to your web server.

#### Via FTP

Open up your FTP program and log into your web server. In most graphical FTP programs, you will have two views, one for your local files and one for the remote files:

![](img/0151410972a5c5ea0aab26416e44b1da_179_0.png)

In your remote file set, navigate so you can see the Joomla! root directory structure. At this level, create a new folder named videos. Browse your local computer for the video file you want to upload and send it on up to the new video directory.

> It is good practice to make a blank .html file and upload it to your new video directory. This will help stop anyone from being able to directly view all of the video files inside your new video directory. If you cannot create a new .html file, copy an existing Joomla! .html file that resides in the images directory.

#### Via the Media Manager

In order to upload a file via the Media Manager, log into the administration and visit the **Content | Media Manager** feature. Use the left-hand side directory structure to navigate to existing directories, or use the **Create Folder** feature to create a new video directory:

![](img/0151410972a5c5ea0aab26416e44b1da_180_0.png)

> The Media Manager location defaults to the setting in the Global Configuration settings. Usually this is within the /images directory. If you wish to create a new root directory at the same level as the /images one (as we did using the FTP method), then you will need to change the default Media Manager path in Global Configuration and then come back to the Media Manager area to create your new directory.

![](img/0151410972a5c5ea0aab26416e44b1da_181_0.png)

The Global Configuration also contains other media settings such as Max File upload size, which may need to be taken into consideration when uploading videos.

Once your videos directory has been created, simply use the file upload feature of the Media Manager to upload your file into the selected directory.

### Editors

One of the most frustrating things with WYSIWYG editors is that they strip out or adjust your nicely formatted code. Some are worse than others at performing this role, but it will be in your interest to turn off your WYSIWYG editor when entering anything but simple HTML custom code into your Joomla! Articles.

Although, I have just recommended that you turn your editors off, I know they can be useful at times, and some users can't live without them. One simple trick is to create a new super administrator user and call it something like "noeditor". Then go and set the Editor choice for this user as Editor - No Editor. Using this method, you can log in as the "noeditor" user whenever you wish. This saves having to switch the editor on and off frequently.

User editor settings can be adjusted on a "User basis" by going to the Site | User Manager page. Click on the user you wish to edit and there will be a parameter setting labeled Editor:

![](img/0151410972a5c5ea0aab26416e44b1da_182_0.png)

Select the option you require and click on Save on the User Edit page. Editors can also be set on a higher level in Global Administration. Again, visit the Site | Global Configuration menu to adjust this setting. Once on the Global Configuration page, you will see a setting labeled Default Editor. Select the option you require, and click on Save on the Global Configuration page to save your changes:

![](img/0151410972a5c5ea0aab26416e44b1da_182_1.png)

At Joomla.org, there are numerous editor extensions which can be easily installed and provide some fantastic functionality for editing your site content. All of these treat the code differently, but as a blanket rule: When entering complex HTML code into your Joomla! Articles, it is best to have all editors turned off. Once your custom HTML code has been included into the article page and saved, be cautious about turning on your WYSIWYG editor and revisiting the article. When your WYSIWYG editor loads the source code, it may strip out all of your hard work, and leave you with an article that will not display correctly.

To help make the Joomla! editor a bit more HTML friendly, you can adjust the security for your editor filtering. Here is how to do it:

- Within your Joomla! Administration, visit the Article Manager screen.
- At the top-right of the page, you will see an option to edit the Global Article "Parameters".
