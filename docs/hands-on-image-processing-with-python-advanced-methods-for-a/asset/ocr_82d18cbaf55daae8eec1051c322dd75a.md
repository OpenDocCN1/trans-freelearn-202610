# Downsampling and aliasing

Improper downsampling introduces aliasing artifacts because high-frequency image components become misrepresented as lower frequencies after sampling. This often produces jagged edges, distorted textures, or *Moiré patterns*. In the frequency domain, aliasing occurs when high frequencies above the Nyquist frequency fold back into (are interpreted as) lower frequencies:

$$f_{\mathrm{alias}} = |f - kf_s|$$

where $k$ is an integer multiple of the sampling frequency.

# Anti-aliasing and Low-pass filtering

Anti-aliasing techniques reduce these artifacts by attenuating high-frequency components before downsampling. A common approach uses Gaussian low-pass filtering with kernel:

$$G(x,y)=\frac{1}{2\pi\sigma^2}e^{-\frac{x^2+y^2}{2\sigma^2}}$$

where $\sigma$ controls the amount of smoothing. Low-pass filtering suppresses fine textures and edges that would otherwise cause aliasing during resampling.

# Interpolation and Resampling with the Lanczos kernel

Interpolation methods such as nearest-neighbor, bilinear, and bicubic interpolation estimate pixel values during resampling. Among these, **Lanczos interpolation** provides sharper results with reduced aliasing by using a windowed *sinc* kernel, given by the following in 1D:

$$L(x)=\begin{cases}\operatorname{sinc}(x)\operatorname{sinc}\left(\frac{x}{a}\right), & |x|<a\\0, & \text{otherwise}\end{cases}$$

where $a$ determines the window size (commonly $a=2$ or $3$). Larger values preserve more high-frequency detail but increase computational cost. The interpolated pixel value is computed as a weighted sum of neighboring pixels using the Lanczos kernel. Next figure shows the kernel for different values of scaling parameter that determines width of window:

![](bbox=[0.393, 0.687, 0.676, 0.876])

*Figure 4.7 – Lanczos kernel for different values of **a***