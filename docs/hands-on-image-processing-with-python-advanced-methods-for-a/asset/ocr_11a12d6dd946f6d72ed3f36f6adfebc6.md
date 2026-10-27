If you run above code and plot the images, you should obtain a figure like the following:

![](bbox=[0.102, 0.099, 0.854, 0.509])

*Figure 4.53 – Effect of translation in the spatial domain on the frequency spectrum of DFT*

# Conservation of energy: Parseval/Raleigh theorem

Parseval/Rayleigh’s theorem states the conservation of energy between the spatial and the frequency domains and is stated as follows for a 2D signal (image) of size :

The following code snippet demonstrates that the theorem holds true for the lena image:

```
```python
f = imread('images/lena.jpg', True)
F = np.fft.fft2(f)
f_energy, F_energy = np.sum(np.abs(F)**2) / np.prod(f.shape), np.sum(f**2)
f_energy, F_energy, np.isclose(f_energy, F_energy)
# (11918.093481563103, 11918.0934815631, True)
```
```

As we can see from the preceding output, the energy computed in the spatial and the frequency domain are very close, asserting the conservation of energy across domains.