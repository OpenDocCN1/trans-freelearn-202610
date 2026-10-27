# Applying Gaussian blur

Let’s now proceed towards the demonstration of efficient frequency-domain filtering (the implementation is possible because of the convolution theorem). More specifically, you will see how the Gaussian blur filter can be applied to a few images in the frequency domain, using a few Python library functions.

# With numpy.fft

The following code block shows how to apply a Gaussian filter in the frequency domain using the convolution theorem and numpy.fft (since in the frequency domain it is simply multiplication):

```
```python
def plot_spectrum(F, method=None, cmap='coolwarm'):
    F_disp = (20*np.log10(0.01 + np.abs(F))).astype(int) \\
             if method == 'log' \\
             else np.minimum(np.abs(F).astype(int)+100, 255)
    plt.imshow(F_disp, cmap=cmap, aspect='auto')

im = np.mean(imread('images/lena.jpg'), axis=2)
gauss_kernel = np.outer(signal.windows.gaussian(im.shape[0], 5),
                        signal.windows.gaussian(im.shape[1], 5))
freq = fp.fft2(im)
assert(freq.shape == gauss_kernel.shape)
freq_kernel = fp.fft2(fp.ifftshift(gauss_kernel))
convolved = freq*freq_kernel # by the convolution theorem,
                             # simply multiply in the frequency domain
im_rec = fp.ifft2(convolved).real # imaginary part is artifact
# plot im, im_rec, gauss_kernel;
#      spectrums freq, freq_kernel, convolved
```
```

> **Note**
> 
> After applying the inverse FFT, we take only the real part of the result because the imaginary part typically arises from floating-point precision errors during FFT computations. In theory, if all operations were exact, the inverse FFT of a real-valued image would also be entirely real.

The following figure shows the output plot of the preceding code, the original *Lena* image, kernel, and output obtained after convolution, both in spatial and frequency domains: