
Even though the CSS is short, there's quite a bit going on here. You can see this if you view `snippets/04/ex3-responsive-header-images/index.html` from the code package in a browser and resize the width of the browser. The header will resize according to the size and the image will always fill the heading area. Likewise, you'll notice that the heading text will also change its size based upon the width of the browser window or the device, though this isn't shown in the preceding CSS.

## Note

If you want, you can also view the example using the interactive snippet playground. Select **4: Responsive Design**, **Example 3**.

There are a couple of interesting features of this CSS:

*   The value of `background-size` is set to `cover` (as shown after the slash in the `background` property shorthand). This instructs the browser to size the image such that it covers the entire area. The browser will also maintain the aspect ratio of the image.
*   The value of `background-position` is set to `right 33%`. We can specify any position we need; it just depends upon the image we're using and whether there's anything important we want to try to keep visible.
*   The `height` value is `25vw`. This ensures that the heading is always 25% of the width of the viewport. We also set `min-height` simply to keep the height from getting too small.
*   By default, the image we are using is `1024 x 512 px` wide. This is down-sampled from the original image, which is `2000 x 1000` wide. If the width of the browser is greater than `1024 logical pixels`, we may as well automatically switch to the larger image. Otherwise, the browser will have to up-sample the smaller image.
*   For pixel densities of `1.5:1` or higher, we display a larger image when the logical pixel width exceeds `512 px`. We could have specified a `2:1` density; but if the pixel density is more than `1:1`, it's better to down-sample than up-sample it. The ratio we've chosen here is fairly arbitrary.

The text that appears over this image is also responsive—the size changes based upon the width of the browser. However, there are cases when we might want to limit the font-size. In addition, since this example has text over an arbitrary image, it's helpful to add additional properties to make the text more readable. Here's the CSS:

```css
.fancy-heading h1 {
    box-sizing: border-box;
    font: normal 10vw Helvetica, sans-serif;
    text-align: right;
    color: white;
    text-shadow: 0 5px 5px black;
    background-image: linear-gradient(180deg, rgba(0, 0, 0, 0), 
        rgba(0, 0, 0, 0.75));
    position: absolute;
    bottom: 0px;
    left: 0; right: 0;
    padding: 10px; margin: 0;
}
@media only screen and (min-width: 1024px) {
    .fancy-heading h1 {
        font-size: 77pt;
    }
}
```

Let's go over the interesting portions of the preceding code:

*   The `text-shadow` property is used to separate the text from its background by rendering a shadow around the text. This is useful especially if the background image has near white areas.
*   The `background-image` property specifies a gradient that darkens the area behind the text. Again, this is intended to increase readability.
*   The `font-size` value is `10vw` or 10% of the viewport's width. If the size gets too big, however, we cap the size at `77pt`. Note that this is arbitrary—a matter of whatever looks best to your eye.

Note that the shadow and gradient are performance-intensive; the browser has to calculate how the shadow and gradient should look and this takes additional time. Don't overuse these effects or your app may start to feel sluggish.

Another typical pattern is a fullscreen image. With the use of viewport units, this can be accomplished using only a small variation of the previous code:

```css
.fancy-heading {
    ...
    width: 100vw;
    height: 100vh;
}
```

If you didn't want to use viewport units, the same can be accomplished by absolutely positioning the image and setting the `top`, `left`, `right`, and `bottom` properties to `0`.

## Note

This example is at `snippets/04/ex4-responsive-fullscreen-images` in the code package of this book. If you want, you can also view the example using the interactive snippet playground. Select **4: Responsive Design**, **Example 4**.

So far, we've been using `cover`, which is great unless you want to ensure that the entire image is always visible. If the latter is what you need, use `contain` instead. You'll need to have an appropriate `background-color` value though, since the image may not fill the element entirety. Instead, the browser fits the image within the element's area and finds the largest size it can use without cropping it.

Complex layout in CSS has often been a source of frustration. Sometimes, it is possible to use the `display: table-*` variants to get the desired effect; but these are far from ideal for nontabular data.

The **Flexbox layout module** provides us with mechanisms to control the layout of our elements, even when the dimensions of the parent element or viewport may not be known. This makes it ideal for responsive mobile applications. Flexbox enjoys reasonably good support in modern browsers (see `http://caniuse.com/#feat=flexbox`). Unfortunately, the specification has gone through several iterations, and older browsers implemented the specification at those stages. Anything lower than `iOS 6.1` and `Android 4.4` uses an older version of the syntax. So, if you plan on supporting either of these, you would have to support multiple versions of the specification. Generally, this isn't difficult; but older versions don't always support the same features. In most browsers, you'll also have to use an appropriate prefix, such as `-webkit-`.

In this section, we'll be using the modern syntax for Flexbox. There are lots of other tools, however, that you can use to supply prefixes automatically, including `Autoprefixer` (`https://github.com/postcss/autoprefixer`), or you can even develop `Sass` mixins to do the same thing.

First, we need to get some terminology out of the way. Flexbox's layout rules can affect two different kinds of elements: containers and items. This should be obvious; but when we're dealing with HTML, we often think of elements in terms of parent-children relationships. In the Flexbox layout, the container is essentially the parent and the items are the direct descendants. Visually, the relationship is like the following image:

![Using flex-box layout](graphics/9781OS_04_08.jpg)

Notice that the container element receives a `display: flex` style rule. This instructs the container to lay its items out in a flexible manner. Relative to its parent, the container itself will be laid out as if it were styled with `display: block`. There is also an `inline-flex` option if you need it (equivalent to `inline-block`).

The items within will automatically be laid out flexibly; but you'll almost certainly want to specify how these items can grow or shrink. There are three rules to accomplish this:

*   `flex-grow`: Determines whether the element can grow and how much it can in relation to other items. By default, growing is disabled, but setting this property to zero will also disable growth.
*   `flex-shrink`: Determines whether the element can shrink. By default, shrinking is allowed, but setting this property to zero will also disable shrinking.
*   `flex-basis`: Determines the default size of the element prior to growing or shrinking. If `auto` is specified, the browser adjusts the size based on `flex-grow`.

There is a shorthand property named `flex`, which is what you see in the prior image for each item. The order is `flex: <flex-grow> <flex-shrink> <flex-basis>`. The latter two numbers aren't required; if they aren't specified, the browser will intelligently figure them out.
