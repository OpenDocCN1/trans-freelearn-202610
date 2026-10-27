# Uniform quantization

In uniform quantization, the intensity range [$I_{\min}, I_{\max}$] is divided into $K$ equal intervals. The quantization step size is $\Delta = \frac{I_{\max}-I_{\min}}{K}$ and the quantized intensity value is computed as:

$$I_q(x,y)=\left\lfloor\frac{I(x,y)-I_{\min}}{\Delta}\right\rfloor\Delta+\frac{\Delta}{2}$$

where $\lfloor\cdot\rfloor$ denotes the floor operator. Each pixel is mapped to the representative value (typically the midpoint) of its quantization interval. Next code snippet demonstrates uniform quantization of a grayscale image with function `quantize_image()`:

```
```python
def quantize_image(img, levels):
    img_min, img_max = img.min(), img.max()
    step = (img_max - img_min) / levels
    quantized = np.floor((img - img_min) / step)*step + step/2
    return np.clip(quantized, 0, 1)

image = img_as_float(rgb2gray(data.astronaut())) # grayscale float in [0,1]
quant_8bit = quantize_image(image, 16) # Quantize to 4-bit (16 levels)
quant_4bit = quantize_image(image, 4) # Quantize to 2-bit (4 levels)
```
```

If you run the preceding code snippet and visualize the quantized images along with the original image, you should obtain a figure like the following one:

![](bbox=[0.101, 0.521, 0.852, 0.73])

*Figure 4.12 – Original astronaut image and the 4-bit and 2-bit quantized images*

Here, `step` represents the quantization interval width, `levels` specifies the number of quantization levels, `np.floor()` assigns pixels to discrete intervals, and `np.clip(...,0,1)` constrains intensities to the valid normalized range. The astronaut image is converted to grayscale and normalized to [0,1] using `img_as_float()`.