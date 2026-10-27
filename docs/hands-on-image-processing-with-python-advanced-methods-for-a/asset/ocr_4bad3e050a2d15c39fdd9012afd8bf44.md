# More image manipulation with PIL

Continuing from where we left off in the last chapter, let us explore some more applications of image manipulation with PIL.

# Changing the image quality/resolution of a JPEG image

The following code demonstrates how the quality parameter of the Image.save() method controls JPEG compression quality and file size. JPEG uses lossy compression to reduce storage by discarding image details that are less perceptible to the human eye. Internally, Pillow converts the image from RGB to YCbCr color space, applies chroma subsampling (for example, 4:2:0), divides the image into 8 × 8 blocks, and applies the Discrete Cosine Transform (DCT). Intuitively, the DCT converts image blocks into frequency components, concentrating most visual information into low-frequency coefficients while fine details correspond to high frequencies. Lower quality settings quantize these high-frequency components more aggressively, producing smaller files but introducing artifacts such as blocking, blurring, and ringing. We shall revisit the DCT and its mathematics in detail in Chapter 4.

The code saves the image with quality values 95, 20, 5 and 1, demonstrating the trade-off between visual fidelity and compression ratio. Higher quality preserves details, whereas lower quality increases compression and visible degradation. File sizes and compression ratios are computed using os.path.getsize() to quantitatively compare storage efficiency.

```
im = Image.open('images/parrot.jpg')
qualities = [95, 20, 5, 1]
for quality in qualities:
    im.save(f"images/parrot_{quality}.jpg", quality=quality)
plt.imshow(np.hstack([imread(f"images/parrot_{quality}.jpg") \
    for quality in qualities]))
```

If you run the preceding code snippet, you should obtain a figure like the following:

![](bbox=[0.11, 0.62, 0.84, 0.74])

Figure 3.1 – Changing image quality of a JPEG image with PIL

If you extract a small patch (e.g., using NumPy slicing with im[100:200, 200:300]) from each compressed image, im, and perform a zoomed-in visualization, you can observe compression artifacts such as blocking, blurring, and ringing, especially at lower-quality levels, as shown in the following figure.