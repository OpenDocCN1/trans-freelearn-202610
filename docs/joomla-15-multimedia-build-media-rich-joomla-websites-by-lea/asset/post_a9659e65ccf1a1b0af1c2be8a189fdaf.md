
## Web Safe Font List

- Arial, Helvetica - sans-serif
- Comic Sans MS - cursive
- Courier New, Courier 6 - monospace
- Georgia - serif
- Impact, Charcoal 6 - sans-serif
- Monaco 5 - monospace
- Lucida Sans Unicode, Lucida Grande - sans-serif
- Palatino Linotype, Book Antiqua 3, Palatino 6 - serif
- Tahoma, Geneva - sans-serif
- Times New Roman, Times - serif
- Trebuchet MS 1, Helvetica - sans-serif
- Verdana, Geneva - sans-serif
- Symbol, Σγμμβολ 2
- Webdings, ♈♉♊♋♌♍
- MS Sans Serif 4, Geneva - sans-serif

Microsoft started a "Core Fonts for the Web" initiative back in 1996. This set contained existing fonts, fonts which were commissioned by Microsoft, and some licensed from Apple. The "Core Fonts for the Web" selections are classified as members of the "web safe" font category.

"Core Fonts for the Web" were designed to:

- Support an international market
- Be legible on screen
- Offer a range of typographic styles

The "Core Fonts for the Web" initiative included the following fonts. Note that these fonts are all the same font size.

- Andale Mono
- Arial
- Comic Sans MS
- Courier New
- Georgia
- Impact
- Times New Roman
- Trebuchet MS
- Verdana
- ♈♉♊♋♌♍ (Webdings)

As mentioned earlier in this chapter, using a generic font family which is defined in your CSS will be the safest option. Remember our generic font families?

- Serif
- Sans-serif
- Cursive
- Fantasy
- Monospace

One of these font families should always be specified at the end of your font family CSS definition, like this:

```
body {
    font-family: Arial,Helvetica,sans-serif;
}
```

The previous example shows the sans-serif font family that would be loaded in if neither the Arial or Helvetica fonts were available on the user's computer system.

As web browsers continue to evolve and adopt font typefaces, the real-time downloading and displaying of specific fonts will evolve our current "web safe" font limitations. For now, it's best to play safe (by choosing a web safe font) and make sure your site visitors (whatever their browser and operating system) can easily read and obtain your important textual information.

## Summary

The use of text in our Joomla! web pages is usually the most prominent media element on the page. However, if a pre-built template has been used with an included font style, often little consideration has been given towards the font type and if it is suitable for the intended audience.

This chapter has highlighted the numerous fonts that are available to use and which ones are classified as "web safe", providing your users with the best opportunity to see the site text styling as you designed it to be seen.

Joomla! Template CSS files are an important and powerful styling mechanism that allows us to define and style our site text as well as other elements. In our CSS, we can use fixed text styling methods, or relative, allowing the user to resize their web page text as they require. Many Joomla! Templates contain enhanced "Typography" features including font-options and custom CSS styling. There are also some excellent third-party font extensions available at http://extensions.joomla.org/. Some of these include image replacement features which bring some exciting new font technologies to your Joomla! website.

Web Safe fonts are a safe choice to use for your main body-text. There is a great range of web safe fonts, which can provide different emotional responses from your site users. By choosing the correct font for your site, you can evoke numerous emotions such as a serious, humorous, or a classy feeling to your web text. You can also increase visitor "time on site", by providing correct font size and user-resizing controls for the text. Make sure you specify a font family in your CSS definitions, so if your specified fonts are not available, the browser will be able to choose a generic family-font to display your site text.

# 4. Adding and Managing Image Content

Next to text content, images are one of the most utilized multimedia resources in web pages. They can be used to inform and educate the reader, offer help with navigation, and to enhance the design and user's overall visual experience.

Whether you need to create multimedia image galleries in your Joomla! website or simply add images to your articles and modules, this chapter will provide the assistance to help utilize image content effectively within your Joomla! website. We look at:

- Why we use images in websites
- Image formats and which ones to use
- Including images in articles and modules
- Creating image galleries and slideshows
- Template images
- Joomla! Image Extensions
- Browser support and accessibility for images

## Images and why we use them in websites

Research suggests that images enhance learning by illustrating the concepts visually and by providing visual memory cues to the viewer. We have been using images to describe, tell stories, and record history since our human evolution.

I have been in a few situations where, if a language barrier between me and the other party exists, we have resorted to a pencil and napkin in order to communicate effectively.

Pictures can convey a message, which might take many thousands of words to describe. This non-textual communication and the visual emotions that the use of images can generate mean they are the perfect medium to complement or replace the text in our web pages:

![](img/0151410972a5c5ea0aab26416e44b1da_88_0.png)

The previous image could easily take a thousand words to describe, which would look uninteresting to the reader and take up valuable space on our web pages. Instead the picture only utilizes a fraction of the space that our description would use and tells a story in itself.

Not only do images describe a story, a moment in time, or a fantasy situation; they can provide a visual stimulus, which portrays a style or theme and sets a mood for the viewer. Many Joomla! Templates now contain a high percentage of graphical images in order to produce interesting designs and effects, such as this commercial example from http://www.yootheme.com:

![](img/0151410972a5c5ea0aab26416e44b1da_88_1.png)

Many templates now utilize rounded edges for the graphics, or faded effects, and some create a 3D type effect on our two-dimensional computer screens. This is achieved by using shading and image layering effects. Others use images to create interesting navigation effects which could not be achieved without using these.

Besides the design, navigation, and branding effects that images help provide, inside our content and modules, we use images to advertise and communicate to our site visitors. One important trick as web developers is to make sure our images are as optimized as they can be before asking our viewers to load them into their web browsers. This ensures a pleasant user experience because if a site is slow to load, or images are missing, these can be a big turn-off to your site visitors.

## Image formats and which ones to use

Images can often make up 50 percent of a Joomla! website; sometimes even more. These images that get loaded into the browser can be part of your template design, or site images we have loaded into our modules and articles. Choosing an appropriate format for this image content will help optimize the loading times of your web pages, which is one of the most important considerations when building a multimedia rich website:

![](img/0151410972a5c5ea0aab26416e44b1da_89_0.png)

There are a few simple rules we can use in order to choose an appropriate image format. The most important criterion for the final file is the size. The previous image is exported using Adobe Fireworks on a Mac computer. External image editors such as the GNU Image Manipulation Program (GIMP) are good open source solutions for manipulating and optimizing images for the web. GIMP can be downloaded by visiting http://www.gimp.org.

With each image requiring loading by the user's browser, page-loading times can be easily affected with the quality and quantity of images you use in your web pages. This in turn results in end user frustration (especially on dial-up Internet connections) and loss of site traffic.

## Digital images

Before we proceed further into file sizes and file types, it is important to mention a note about digital images. Because all of our web page images are stored or viewed on a computer device, they are in a digital format (an electronic file stored using zeros and ones). A digital image is built up of tiny elements called pixels. **Pixels** are the building blocks of all digital images and are small adjoining squares in a matrix across the length and width of your image.

Pixels are so small that you cannot easily see them when the image is viewed on your computer screen:

![](img/0151410972a5c5ea0aab26416e44b1da_90_0.png)

Each pixel is an element containing a single solid color, and when put together all of these tiny dots make up your final image. Usually, more pixels per area make up a higher quality image, but there is a point when viewed with the human eye on electronic devices that you cannot actually see the extra detail that the additional pixel's bring to the image:

![](img/0151410972a5c5ea0aab26416e44b1da_90_1.png)

The physical dimensions of a digital image are measured in pixels and all digital images have what is called an image resolution (the image height and width dimensions in pixels). It is possible to save your digital images in numerous formats and compressions in order to optimize the file quality and size. Image editing programs, such as Adobe Fireworks, are specifically designed with web optimization and export options.
