# An overview of slides

This provides the user with a visual overview of the slides, as shown in the following screenshot:

![](bbox=[0.256, 0.221, 0.743, 0.505])

The presentation slides will be recreated in a new full screen Div element. This is styled using the fullScreen class in the CSS stylesheet located in the SlideShowApp constructor:

```
overviewScreen = new DivElement();
overviewScreen.classes.toggle("fullScreen");
overviewScreen.onClick.listen((e) => overviewScreen.remove());
```

The HTML for the slides will be identical. To shrink the slides, the list of slides is iterated over, the HTML element object is obtained, and the CSS class for the slide is set:

```
currentSlideShow.slides.forEach((s) {
  aSlide = s.getSlideContents();
  aSlide.classes.toggle("slideOverview");
  aSlide.classes.toggle("shrink");
  ...
```