```
plt.subplot(N,N,i+1), plt.imshow(images[i].reshape(N,N))
plt.xticks([]), plt.yticks([])
plt.suptitle(f'{N}x{N} {tname} basis functions'); plt.show()
# g, tname, N = dct, 'DCT', 4
# plot_basis_images(get_all_basis_images(g, tname, N), tname, N)
# g, tname = walsh, 'Walsh'
# plot_basis_images(get_all_basis_images(g, tname, N), tname, N)
```

If you uncomment and run the preceding code snippet, you should obtain the figure below:

![](bbox=[0.22, 0.26, 0.78, 0.48])

Figure 4.38 – Discrete cosine and Walsh basis functions

Let's move on and explore a few properties of DFT that can turn out to be very useful.

# Properties of DFT

Understanding the properties of the DFT allows for more efficient computation and insight into how frequency information is represented in both one-dimensional signals and two-dimensional images.

# Separability

2D DFT is a separable and symmetric unitary transform. The following figure shows how 2D DFT computed by applying two successive 1D DFTs along the rows and columns, where the input f is a 2D signal of dimension M × N , and the output F is the DFT output:

![](bbox=[0.15, 0.68, 0.85, 0.82])

Figure 4.39 – Separability of 2D DFT