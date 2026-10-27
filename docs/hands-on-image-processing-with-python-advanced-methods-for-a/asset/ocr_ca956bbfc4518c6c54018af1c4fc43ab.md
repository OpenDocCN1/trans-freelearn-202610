![](bbox=[0.138, 0.071, 0.909, 0.219])

Here are the key points to note:

+   - The Wiener filter balances deblurring and denoising by adaptively weighting frequencies.
+   - Unlike the inverse filter, it does not excessively amplify noise at high frequencies.

In practice, Wiener filtering provides visually pleasing restoration when the blur kernel is approximately known, even with moderate noise.

# Tikhonov regularization in image restoration

The inverse filter naively attempts:

$$\hat{F}(u,v)=\frac{G(u,v)}{H(u,v)}$$

The problem is: If $H(u,v) \approx 0$ for some frequencies, division amplifies noise catastrophically. The Wiener filter addressed this by modeling noise and signal power spectra. But what if noise statistics are unknown? This is where regularization approaches, especially *Tikhonov regularization*, come in.

# Variational formulation

Instead of directly inverting, we treat restoration as an optimization problem: find an estimate $f$ that explains the observed data while also respecting some prior belief about natural images. The Tikhonov functional is:

$$\hat{f}=\arg\min_f\left(\|h*f-g\|_2^2+\lambda\|Lf\|_2^2\right)$$

*data fidelity* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *regularization / prior*

where $\|h*f-g\|_2^2$ ensures the restored image, when blurred, matches the observation (data fidelity), $L f$: a linear operator encoding prior knowledge and $\lambda > 0$: regularization parameter controlling the tradeoff. Common choices for $L$:

+   - $L=I$: penalizes energy of the image (smooths globally).
+   - $L=\nabla$: penalizes large gradients, favoring smooth images.

Thus, the prior is often called a fidelity + smoothness prior.