
## Adding and Managing Image Content

It is easy to see a number of images that help make up this template. We have images for the logo and rounded images for the boxes. Here is the same template loaded in the browser with the images disabled:

![](img/0151410972a5c5ea0aab26416e44b1da_122_0.png)

The previous examples are of a relatively simplistic template, but you can see how the template images come together in greatly enhancing the site design. The advancement of Joomla! Templates are increasing every month, with many companies making extremely professional templates available for purchase each month.

These commercial templates often include preset color themes and styles, offering the ability to change the site color scheme, and layout in seconds. This flexibility offers the administrator powerful features to create a new look for their site, but also can mean that some templates contain an abundance of scripts and images that never get used once the administrator had made a decision on the color choice, among others. What used to be a simple path to your template image folder can now often be multiple directories in your template containing the image files:

![](img/0151410972a5c5ea0aab26416e44b1da_122_1.png)

## How to adjust your template images

The images for a Joomla! Template are generally located in the <Your Joomla Home Folder>/templates/<yourtemplate>/images directory. If you would like to adjust certain images in your template, the easiest way to do this is by viewing the template in your site browser and then making the necessary adjustments to the images.

It is worth mentioning that many web developers develop their sites locally. A local development process is easily created by setting up a localhost server environment on your computer. Utilities such as MAMP for MAC, and XAMPP for Windows provide easy software installs of all the necessary components for running a local server environment. If you don't already work locally, take two minutes out to look into these programs, as they not only save you time (as there are no connections to the live web servers), but also provide confidence when developing as you are never working on your live site:

![](img/0151410972a5c5ea0aab26416e44b1da_123_0.png)

> There is more information about localhost development in Chapter 9, Joomla! Multimedia Project.

One of the easiest ways to find and manipulate your site images is to either use a browser element inspector tool (such as Firebug for Firefox or Web Inspector for Safari), or to simply right-click on the images in the web browser, and choose **View Image**. This option may differ depending on the browser you are using, but most browsers offer the ability to find the location (URL) of the image or to open it in a new browser tab:

![](img/0151410972a5c5ea0aab26416e44b1da_124_0.png)

Your image URL may look something like: `<Your Joomla Home Folder>/templates/<yourtemplate>/images/logo.png`. This image URL method lets you know the path, so you can find the necessary image.

There are hundreds of available image manipulation programs, and we cannot go into the details of image manipulation in this book. However, if you stick to the following rules when adjusting your images, you will be on the right track:

- Copy the original file. For example, if your file is called `logo.png`, then name the original file as `logo.png.orig` (or something similar), so you always have a copy of that original file. Or if you will be editing your website regularly, consider a version control system such as SVN or Git.
- Make a note of the file name, size, and format before editing the file.
- Unless you are a pro, always keep the same image format as the original image. If this image is referenced in the CSS or original code, your new image name will be different and the image will no longer show.
- When saving an image, try to make sure the file size is as low as the original or as close to it as you can achieve through image optimization.
- If you do change the image dimensions, make sure you check if there are any CSS variables or other code that might specify the physical file dimensions.

With most templates, one of the first files you will adjust will be the Template Logo. The process for changing the logo is exactly the same as the previous steps.

I briefly mentioned the use of Web Inspector Tools. Probably one of the most common of these is a tool available for the Firefox web browser. This is called Firebug. Visit http://getfirebug.com/ for more information. Firebug allows you to edit, debug, and inspect CSS, HTML, and JavaScript on any web page:

![](img/0151410972a5c5ea0aab26416e44b1da_125_0.png)

Firebug allows you to inspect web elements and edit these on the fly, in your browser. For example, if you wish to move your logo sideways or up and down a bit, simply inspect the image logo element, and then adjust the corresponding CSS which positions this element. This is just a brief example of what the Firebug tool can do, but its power comes in handy when you need to problem solve errors, and so on. If you are keen on enhancing and developing for your Joomla! Template, I suggest a tool like Firebug which will really help you out!

It is worth noting that Internet Explorer and Safari also have their own versions of an "Element Inspector".

## Browser support and image accessibility

Almost all web browsers now offer image support. There are some considerations, however, when using images in your website content. The W3C Accessibility Guidelines emphasize the importance of offering text support (or text equivalents) to non-text content.

The power of text-equivalents lie in their capacity to provide "information and support" to people from disability groups and those using varied technologies to view your web page content.

Using text alternatives when using images can be a foolproof way to make sure that the widest possible audience has access to your web content. Text-alternatives can be visually read by screen readers, the size of the text adjusted for viewing, and content printed off to read in Braille, and so on, which makes a powerful argument for making alternative text content available.

Secondly when we do use images, it is important to specify the alt and title tag information, so users who have images turned off in their browsers can see the "alternative text" which explains what should be loading in. Title tags provide clear tooltips to the user and both of these tags combined can enhance accessibility and search engine optimization:

![](img/0151410972a5c5ea0aab26416e44b1da_126_0.png)

Up until recently, browsers such as Internet Explorer did not offer image transparency support, or PNG file extension support. Web developers had to use workarounds in order to show PNG files to users using these browsers to view their websites. Now Internet Explorer 7 and 8 both provide PNG support as a standard. SVG is a relatively new format, which required a browser plugin, for again the Internet Explorer browser. Web browsers, however, are improving every day, as is the support in them for various image formats.

## Summary

Images are an important communication element. They are used to tell stories, show facts, advertise and sell, as well as visually entice the user with emotive feelings. Our Joomla! Templates usually contain many images, which create eye-pleasing and complex designs.

The use of images in websites has evolved quickly, from flat single color images to realistic high quality formats that cater to transparency and lossless compressions. The Web just wouldn't be the same without them! An increase in broadband and connection speeds has allowed the evolution of beautiful image-gallery scripts; a number of these being incorporated into powerful Joomla! Image Extensions. There has never been a better time to display images and galleries in your Joomla! site.

Take careful consideration when adjusting your existing template images, or when creating new ones for your site. Image file size and optimization is an important consideration to your site users. The use of good optimization tools and experience will help in providing quality images for your site while decreasing page loading time.

With a diverse range of user abilities and disabilities, varied connection speeds, and a vast range of hardware accessing the World Wide Web, it is very important to consider the correct use of images and make alternatives available for those who cannot or choose not to view them. The use of alt and `title` tags, as well as height and width properties are all best practice when loading images into your site pages.
