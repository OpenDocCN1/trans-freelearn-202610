# Image resolution

The most common issue is that an image has too high a resolution for the area that displays it. This forces the browser to resize the image to fit the screen or area. If the size of the file is unnecessarily large, then it will take longer to be transferred over an internet connection. The browser will then throw away most of the information. You should resize the image ahead of time before adding it to the site.

There are many image manipulation tools available to resize pictures. If you run Windows, then **Paint.NET** (`https://www.getpaint.net/`) is an excellent free piece of software. This is much better than the Paint program that comes with Windows (although, this will work if you have no other option).

For other platforms, GIMP (`https://www.gimp.org/`) is very good. If you prefer using the command line, then you may like ImageMagick (`http://imagemagick.org/script/index.php`), which can perform lots of image manipulation tasks programmatically or in batches. Also, there are cloud-hosted image management services, such as **Cloudinary** (`http://cloudinary.com/`).

You should shrink images to the actual size so that they will be displayed on the user's screen. There can be complications when dealing with responsive images, which scale with the size of the user's screen or browser window. Also, keep in mind **high DPI** or Retina displays, which may have more than one physical pixel to every logical pixel. Your images may have to be bigger to not look blurry, but the upper bound is still likely to be lower than the raw size. It is rare to need an image at more than twice the size of the displayed resolution. We will discuss responsive images in more detail later in this book, but it is worth keeping them in mind.

The following image displays the resizing dialog from Paint.NET:

![](bbox=[0.365, 0.655, 0.635, 0.89])