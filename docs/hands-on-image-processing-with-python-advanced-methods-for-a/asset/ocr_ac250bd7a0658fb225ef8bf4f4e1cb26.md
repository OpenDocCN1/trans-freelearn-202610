# Using max and min filters

The following code shows how to use the MaxFilter() to remove the pepper noise, followed by a MinFilter() to remove the salt noise from an image. The next figure shows the output of the previous code block. It can be seen that the maximum and minimum filters are somewhat effective in removing the salt-and-pepper noise from the noisy image, respectively.

```
im = Image.open('images/mandrill_spnoise_0.1.jpg')
sz = 3
im_max_out = im.filter(ImageFilter.MaxFilter(size=sz))
im_min_out = im.filter(ImageFilter.MinFilter(size=sz))
```

If you run the above code snippet and plot the filtered images along with the input image, you should obtain the following figure:

![](bbox=[0.12, 0.39, 0.86, 0.57])

Figure 7.25 - Applying PIL Max and Min filter

# Smoothing (denoising) with scikit-image

The scikit-image library also provides a set of non-linear filters in the restoration module. In the following sections, we will discuss a couple of such very useful filters, namely bilateral and non-local means filters.

# Using the bilateral filter

The bilateral filter performs edge-preserving smoothing by combining spatial proximity and intensity similarity in its weighting scheme. Let f(x, y) denote the input image. The filtered output g(x, y) at pixel (x, y) is given by:

g(x,y) = 1/W(x,y) Σ_{(s,t)∈N_{x,y}} exp(-((x-s)²+(y-t)²)/(2σ_s²)) exp(-((f(x,y)-f(s,t))²)/(2σ_r²)) f(s,t)

+   - (s, t) ∈ N_{x,y} are neighboring pixels