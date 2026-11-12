> A short summary and explanation of all different CSS units can be found on MDN at https://developer.mozilla.org/de/docs/Web/CSS/length.

# # Designing for mobile devices first

Responsive applications should look and feel great (or work at all) on a huge number of devices with a huge number of different screen sizes and resolutions; they usually run on mobile devices as well as big TV screens. Thus, in the beginning of a project, every developer has to choose an approach to design the responsive application to master this transition; in general, we distinguish between the following methods:

+   - Mobile first
+   - Desktop first

In the desktop first method, we design for desktop devices and mouse input first, then we adjust and scale the design down for mobile devices and touch interactions. This often results in a web application with a reduced functionality for mobile devices due to a much smaller screen dimension and the lack of touch support.

In the mobile first method, we design for mobile devices and touch interactions first, and then we adjust and scale the design up for bigger devices and mouse interactions. This approach makes sure that mobile devices support a complete set of features, which can even be used on desktop clients.

Both methods are equally correct, accepted, and popular nowadays. The only exception is that mobile first became a buzzword due to an increase of mobile devices accessing the web during the previous years. Throughout the book, we will choose a mobile first approach and design for mobile devices and touch support. However, we will, of course, extend the visualizations to support mouse input and bigger screens. That's what the responsive design is all about.

# # Bootstrap – a popular mobile first CSS framework

If we want to create responsive mobile first web applications, we usually don't want to start from zero and re-implement common patterns and components; we would rather want to reuse some well-tested, existing components.