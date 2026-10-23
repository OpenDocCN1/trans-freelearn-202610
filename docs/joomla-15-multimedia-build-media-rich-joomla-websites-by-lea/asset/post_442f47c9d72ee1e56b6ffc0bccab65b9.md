
Image placement using HTML and CSS offers you options to align and style your website content as you desire. Let's look at how we can add an image into our content using pure HTML.

Just like using a WYSIWYG editor to insert an image, when using HTML we are performing a similar process, but this time entering in the image code by hand. In HTML, all images are defined using the <img> tag. Additional properties are given to this tag to make it perform functions. When using the WYSIWYG editor, we need to make sure that our image code is placed in the position where we want it to be displayed on the page:

![](img/0151410972a5c5ea0aab26416e44b1da_103_0.png)

In the editing window, position your mouse cursor where you want to enter your image. For our example, we will insert an image into the second (middle) paragraph of text (note each block of text has a <p> (paragraph) tag around it). The browser will show your image where the image tag occurs in the document. If we put an image in between two paragraphs, the browser will show the first paragraph, then the image, and then the second paragraph. In our example, we want the image to show at the start of our second paragraph, in line with our text content.

We will break this down as an example, so let's start off by inserting the basic <img> tag into our document:

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img>This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

Next we need to provide a location for the image. This is done by using the src attribute (src stands for source). The value of this attribute is the source location of the image you wish to display:

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" />This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

Note how we have now added in a /> at the end of our image tag. This is required to end our <img> tag correctly and make our code XHTML compliant. Next we need to provide an alt (alternative text) description to our image. This is important to inform the viewer of what is loading into the page. Remember some web users have images turned off and use screen readers, so alt tags are very important for all images that you add into your site:

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page" />This is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

If we click on Apply on our content article now, this code will be saved and if the location to the image is correct, the image will show. The previous code will load the image in and then the text will load underneath:

![](img/0151410972a5c5ea0aab26416e44b1da_104_0.png)

If you want the text to wrap around your image, we need to set an align attribute (alignment) for the image. Options for the align attribute are either left, right, middle, top, and bottom, which will result in the image aligning to each of these values in relation to any text block around the image:

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page"
```

The image will now look like:

![](img/0151410972a5c5ea0aab26416e44b1da_106_0.png)

One final attribute that I would like to include is the title attribute (Title). This will provide a tooltip title to the image when the viewer hovers their mouse cursor over the image. Titles are useful for providing additional information and do have an affect on Search Engine Optimization (SEO). The code now becomes:

```
<p>This is an interesting paragraph about inserting an image around text</p>
<p><img src="http://www.mydomain.com/image.jpg" alt="My Page" align="left" height="60" width="100" hspace="5" title="A Nice Clock Image" />is an interesting paragraph about inserting an image around text </p>
<p>This is an interesting paragraph about inserting an image around text </p>
```

The image will now look like:

![](img/0151410972a5c5ea0aab26416e44b1da_106_1.png)

If you want to insert numerous images into your articles and modules, it would be necessary to perform these steps for every image that you want to load in. It is possible to cut and paste the previous code and just change the values each time, but it can become time consuming.

Some image attributes such as the image padding (vspace and hspace) can be set in your CSS, creating a site wide stylesheet class for all of your content images. This CSS code may look like:

```
img { border: 0 none; margin: 3px }
```

The previous example will set a border of 0 and a margin of 3 on all of your site images, so you may need to specify the CSS attribute in more detail to suit your template.

## Creating image galleries and slideshows

Joomla! is a Content Management System designed primarily for organizing and managing website content. It contains numerous multimedia features built into it, but its main focus is providing two roles: Powerful CMS features, and a well-designed framework which allows additional features to be added to the system, thus creating powerful functionality.

These additional features are called **Extensions** and are designed to plug in to the core Joomla! Framework and extend the functionality of it. With regards to the core Joomla! CMS, we have already looked at how images can be included into content items and modules. However, image galleries and slideshows are asking a bit more than just simple image management, and so require the power of extensions to provide these features:

![](img/0151410972a5c5ea0aab26416e44b1da_107_0.png)

The number of multimedia extensions now available for Joomla! is staggering. We have extensions which can create complex galleries, stream in videos, and compile jukebox type audio features.

Having considered at great length the best approach for this section of the book, it has resulted in one option. That is to highlight some of the most popular and useful image gallery and slideshow extensions, and hope that these will provide understanding as to the complex image management capability that can be achieved by using Joomla! Extensions.

## Image management extensions, and how to install them

Before proceeding with covering third-party extension functionality, let's quickly cover how image extensions are added to your Joomla! site.

As with most things in Joomla!, the process of installing extensions has had careful consideration from the development team, and is a very easy to perform. Most Joomla! Extensions come with their own installation instructions, and these general guidelines will help you get them installed on your site.

> Before installing anything, make sure you copy your file set and backup your site database.

For the purpose of this example, I am going to install one of my favorite Joomla! Multimedia Extensions — the RokBox extension by RocketTheme. RokBox is a MooTools powered slideshow plugin that enables you to display numerous media formats with a professional pop-up screen lightbox effect:

![](img/0151410972a5c5ea0aab26416e44b1da_108_0.png)

The installation steps are as follows:

- 1. Download the required extension and save this download on your computer. The file will be in a .zip format, or possibly a .gzip or .tgz format which may require unzipping before installation:

![](img/0151410972a5c5ea0aab26416e44b1da_109_0.png)

- 2. Log into your Joomla! Administrator's Control Panel, and navigate to the Extensions menu, then to Extension Manager. The page will default to the Install screen:

![](img/0151410972a5c5ea0aab26416e44b1da_109_1.png)
