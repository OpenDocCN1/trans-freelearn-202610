If you run above code snippet with a couple of different values of the parameters scale and anti_aliasing, you should obtain a figure like the following:

![](bbox=[0.27, 0.11, 0.52, 0.26])

![](bbox=[0.52, 0.11, 0.78, 0.26])

![](bbox=[0.27, 0.26, 0.52, 0.41])

![](bbox=[0.52, 0.26, 0.78, 0.41])

Figure 4.11 – Rescaling with and without anti-aliasing using skimage

Here, scale=0.3 reduces the image size to 30% of original dimensions, channel_axis=-1 preserves color channels, and anti_aliasing=False disables pre-smoothing, producing stronger aliasing artifacts. Setting anti_aliasing=True applies low-pass filtering before rescaling, significantly improving visual quality and reducing distortion in the downsampled image. To learn more about interpolation and anti-aliasing, please visit my blog: https://sandipanweb.wordpress.com/2018/01/21/recursivegraphics-bilinear-interpolation-and-image-transformation-in-Python/.

Now that we have learned how to resize an image (scaling in the domain of the image function), let’s move on to the quantization of an image (which is scaling in the range of the function).

# Quantizing an image

Quantization reduces a large or continuous range of intensity or color values to a smaller discrete set and is therefore related to the number of bits used per pixel. For a digital image I(x, y), quantization can be represented as a mapping function:

Q: I(x, y) → I_q(x, y)

where I_q(x, y) denotes the quantized image.