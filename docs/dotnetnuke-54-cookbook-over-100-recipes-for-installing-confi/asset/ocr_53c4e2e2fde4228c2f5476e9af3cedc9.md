14. Select the **SampleASCXSkin** from the **Skins:** drop-down list and click on the Preview link.

![](bbox=[0.279, 0.134, 0.721, 0.251])

15. A small minus sign icon will appear next to the breadcrumb we added. If you click on the minus sign, the breadcrumb will disappear and a plus sign icon will remain:

![](bbox=[0.291, 0.329, 0.707, 0.433])

16. Clicking on the plus sign icon will restore the breadcrumb.

# How it works...

This is a handy trick to save "real estate" on your pages by hiding large elements that do not always need to be visible. As the widget operates on the client side it is a very quick and efficient way to show and hide DOM elements.

# There's more...

In this example we used some common DNN plus and minus icon images, but you are free to create your own images and add them to the skin. Just change the path in the CSS file to point to your images and add the images to the skin manifest.

# Using the Stylesheet widget

Just as the Rotator widget can dynamically cycle through a set of images on the client side, the Stylesheet skin widget lets you dynamically switch between different stylesheets for your skin. This can be a very powerful tool for changing the entire appearance of your DNN pages on the client side with very little coding.

In this recipe we will create three simple stylesheets specifying font sizes (small, medium, and large) then add the Stylesheet widget to switch between them.