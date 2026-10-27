The next figure shows the output of the preceding code block, the output image obtained with the BPF:

![](bbox=[0.15, 0.1, 0.5, 0.28])

![](bbox=[0.57, 0.1, 0.9, 0.28])

Figure 6.18 - Applying a band-bass filter to an image

While a band-pass filter allows a specific range of frequencies to pass through, the opposite approach—blocking a selected frequency band—can be equally important in many applications. This leads us to the band-stop filter, which we’ll explore next.

# Band-stop/band-reject (notch) filter (BSF/BRF)

This filter blocks/rejects a few chosen frequencies from the frequency domain representation of the image (obtained with DFT), hence the name. This filter is particularly useful for removing periodic noise from images. The ideal BRF is given by the following:

H_BSF(u, v) = { 0, if D1 ≤ D(u, v) ≤ D2
1, otherwise }

Similarly, we can define the Butterworth and Gaussian BRF (left as an exercise).

# Removing periodic noise

In this example, we will first add some periodic (sinusoidal) noise to the parrot image to create a noisy parrot image (this can happen because of interference with some electrical signal) and then observe the effect of the noise in the frequency domain of the image using the following code snippet:

```
im = np.mean(imread("images/parrot.jpg"), axis=2)
im = im / im.max()
F1 = fp.fft2((im).astype(float))
F2 = fp.fftshift(F1)
# plot original image im and spectrum F2 here
# add periodic noise to the image
for n in range(im.shape[1]):
    im[:, n] += np.cos(0.1*np.pi*n)
F1 = fp.fft2((im).astype(float)) # noisy spectrum
F2 = fp.fftshift(F1)
# plot noisy image im and spectrum F2 here
```