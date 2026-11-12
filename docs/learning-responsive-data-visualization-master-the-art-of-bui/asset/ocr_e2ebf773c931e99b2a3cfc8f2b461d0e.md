# # Hardware-accelerated Pixel Graphics with WebGL

WebGL is a JavaScript API based on OpenGL ES 2 to create hardware-accelerated 3D Pixel Graphics in the browser. This technology is very powerful and allows access to the client's GPUs via so-called Shaders; the final visualization is exposed in the browser via the Canvas element. However, browser support is not very good, as we can see in the following figure; mobile devices support the API only partially and Internet Explorer only from version 11.

> > You can find more detailed information about the WebGL API on MDN at https://developer.mozilla.org/en/docs/Web/API/WebGL_API.

![](bbox=[0.11, 0.335, 0.92, 0.495])

Cross-browser compatibility WebGL from http://caniuse.com/#feat=webgl

Due to WebGL's use of the Canvas element, it doesn't solve any of the limitations of Pixel Graphics that we discussed in the Canvas section. Thus, we will look at a different type of image representation next.

# # Vector graphics with SVG

SVG is an XML specification used to create and compose Vector Graphics. Thanks to HTML5, these graphics can simply be embedded into an HTML page, rendered and displayed perfectly sharp by the browser in every screen resolution. As we can see in the following figure, cross-browser compatibility is very good across all the major browsers and Internet Explorer starting from version 9: