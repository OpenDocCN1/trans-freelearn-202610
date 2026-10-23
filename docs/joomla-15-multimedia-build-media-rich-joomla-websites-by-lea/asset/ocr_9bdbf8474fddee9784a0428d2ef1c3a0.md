# # Good practice techniques

Here are some good practice techniques when using videos in your Joomla! pages.

# ## Text transcripts

It is good practice to provide a text transcript to everything you do on your web page. For example, the alt tag of an image provides a text transcript of alternative text, allowing the user to know there is an image about that subject loading into that area of the page.

If your video is about a speech or news, provide a text transcript of that content and make it available via a hyperlink to another area, or better yet, make it available on the same page somewhere. Text transcripts can be from a few words long to hundreds of them; your good judgment will be important to accurately describe your video content.

# ## Alternative content

Just like images, your HTML, which includes the video into your web page can also contain alternative content. The object element can be used to contain all types of information, much more than that of an image tag `<img />`.

An example would be, if you want to provide a Windows .wmv video on your webpage, then users who use Apple Macs wont be able to view this unless they have downloaded the Windows Media Player for MAC. Using the object element, you can specify that if the user can't play .wmv, then you should load a MPEG version (or alternative format) of that movie instead. If the MPEG movie isn't supported, you could specify that a text transcription be delivered instead.

Alternative content can not only help people with text-based browsers and screen readers, but it can be picked up by search engines.

# ## Captioning

Captions on video content can be useful to the hearing impaired, or people with no speakers on their computers. Captioning may not always be suitable. However, for video content that is news or training-based, captions generally only enhance the video content.