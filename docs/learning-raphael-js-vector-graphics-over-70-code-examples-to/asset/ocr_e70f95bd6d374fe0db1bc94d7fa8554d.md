The SVG specification refers to the drawing area itself as the viewport. Strictly speaking, a viewport is any rectangular viewing region. We will always refer to the browser window as the _viewport_ and our drawing region as the _canvas_ (not to be confused with the HTML5 Canvas).

We can create a canvas using the `Raphael` constructor as follows:

```
var paper = Raphael(50, 100, 500, 300);
```

# Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you. The demos for each of the code samples in this book can be found at http://raphaeljsvectorgraphics.com. Readers are strongly encouraged to open the demos alongside reading the book in order to best relate to the content covered.

This creates a canvas of width 500px and height 300px at 50px to the left of the viewport and 100px from the top of the viewport as shown in the following diagram. The `paper` variable holds a reference to a `Paper` object which is our canvas and we will use it for all future drawing:

![](bbox=[0.1855, 0.5703, 0.8145, 0.8234])