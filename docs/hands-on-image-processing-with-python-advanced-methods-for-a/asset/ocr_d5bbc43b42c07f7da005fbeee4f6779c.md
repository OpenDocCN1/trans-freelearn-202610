# Periodicity and windowing

The DFT inherently assumes that the signal repeats periodically in both the spatial and frequency domains. For an 2D signal, the DFT and its inverse are periodic with period :

F(u, v) = F(u + N, v) = F(u, v + N) = F(u + N, v + N)

The Fourier transform also exhibits conjugate symmetry since or, more interestingly, .

Let’s understand the property with an example. Let’s use the following input of a set of books on a shelf (cropped), compute the DFT, and visualize the spectrum (both unshifted and shifted versions) using the following code snippet:

```
def plot_spectrum(F, cmap='coolwarm'):
    plt.imshow(np.minimum(np.abs(F).astype(int) + 100, 255), cmap=cmap)

f = imread('images/books.png', True)
F = np.fft.fft2(f)
# plot image f, spectrum F (with plot_spectrum()) and
# plot shifted spectrum F (with np.fft.fftshift(F))
```

What you may expect in the spectrum is the presence of the coefficients corresponding to a few frequencies in the direction perpendicular to those of the books. But if you run the preceding code snippet, you will end up with the following output, which shows multiple directions (shown in the next figure). Can you guess what’s happening here?

![](bbox=[0.09, 0.58, 0.88, 0.75])

Figure 4.41 – Magnitude of image spectrum with DFT

This is a typical example of implicit periodicity, which causes the edge effects. What the DFT sees (instead of the input image) is an image like the following: