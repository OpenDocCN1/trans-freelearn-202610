7. Save the file and view it in a browser. The slides now act as links. The solution works, but it would be useful to have the mouse pointer indicate that there is a link. We will achieve this with a little bit of CSS, setting the appearance of the mouse cursor. We need to add the following text to each slide definition:

```
style="cursor: pointer;"
```

8. After modifications, we will have the following code:

```
<!-- The external container, containing the slideshow -->
<div class="art-slider art-slidecontainer79">
    <!-- Slideshow container -->
    <div class="art-slider-inner">
        <!-- First slide. Link to: www.artisteer.com -->
        <div class="art-slide-item art-slide790"
onclick="location.href='http://www.artisteer.com'"
style="cursor: pointer;"></div>
        <!-- Second slide. Link to: www.google.com -->
        <div class="art-slide-item art-slide791"
onclick="location.href='http://www.google.com'"
style="cursor: pointer;"></div>
        <!-- Third slide. Link to: www.example.com -->
        <div class="art-slide-item art-slide792"
onclick="location.href='http://www.example.com'"
style="cursor: pointer;"></div>
        <!-- Forth slide. Link to www.packtpub.com -->
        <div class="art-slide-item art-slide793"
onclick="location.href='http://www.packtpub.com'"
style="cursor: pointer;"></div>
    </div>
</div>
```

9. It's much better! The last thing we should do is add a hint to inform the visitor about which page he/she will be redirected to. To do this, we need to add a title parameter to each <div> representing a slide as follows:

```
title="description of the link"
```

10. Our final code looks as follows:

```
<!-- The external container, containing the slideshow -->
<div class="art-slider art-slidecontainer79">
    <!-- Slideshow container -->
    <div class="art-slider-inner">
        <!-- First slide. Link to: www.artisteer.com -->
        <div class="art-slide-item art-slide790"
onclick="location.href='http://www.artisteer.com'"
style="cursor: pointer;" title="Artisteer official
page"></div>
        <!-- Second slide. Link to: www.google.com-->
        <div class="art-slide-item art-slide791"
```