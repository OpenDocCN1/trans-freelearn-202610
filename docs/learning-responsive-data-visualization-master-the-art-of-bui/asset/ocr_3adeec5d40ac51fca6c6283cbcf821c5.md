> More information about the SVG function getPointAtLength can be found on W3C at https://www.w3.org/TR/SVG/paths.html. An example using this function for implementing a path motion is available at http://bl.ocks.org/KoGor/8162640.

# # Using SVG filters

If you are planning to use filters in SVG, such as blending, blur, turbulence, and so on, you can go for it. Filters on SVG elements are supported in most of the browsers except Internet Explorer 9:

![](bbox=[0.11, 0.29, 0.95, 0.45])

SVG filters (source: http://caniuse.com/#feat=svg-filters)

Just as with many other SVG features, they are also introduced to HTML elements via CSS. However, this will not solve our problem using Internet Explorer 9 but will only result in more restrictions.

![](bbox=[0.11, 0.57, 0.95, 0.72])

CSS filters (source: http://caniuse.com/#feat=css-filters)

To support image filters in Internet Explorer 9, I recommend using feature detection and the old deprecated DX filters.

> More information about DX filters can be found on MSDN at https://msdn.microsoft.com/library/ms532853.aspx.