MAXIM works well for:

+   - **Long-range dependencies:** Traditional CNNs are limited by local receptive fields. MAXIM’s MLP layers can model global context.
+   - **Multi-scale supervision:** Helps refine details across various spatial resolutions.
+   - **Cross-gating blocks:** Improve the interaction between encoder and decoder features.

# Haze removal using dark channel prior

Outdoor photography often suffers from reduced visibility due to haze, fog, or atmospheric scattering. Haze affects both color fidelity and contrast, making object detection and scene understanding more difficult. In this section, we will explore a powerful and widely used image dehazing method called **Dark Channel Prior (DCP)**.

Originally proposed by He, Sun, and Tang in their influential 2009 paper, this technique makes use of a simple yet profound observation about haze-free images: most local patches in haze-free outdoor images contain some pixels with very low intensity in at least one color channel. This statistical prior, the dark channel, forms the basis of an effective haze removal algorithm.

The formation of a hazy image is commonly modeled by the atmospheric scattering model:

```
$$I(x) = J(x) \cdot t(x) + A \cdot (1 - t(x))I(x)$$
```

where:

+   - $I(x)$: Observed hazy image at pixel xxx
+   - $J(x)$: Scene radiance (true image to be recovered)
+   - $A$: Global atmospheric light (typically a 3-channel RGB vector)
+   - $t(x)$: Medium transmission describes the portion of light not scattered and reaching the camera

The goal of dehazing is to recover $J(x)$ given $I(x)$.

# Dark channel prior

Consider the dark channel of an image $I$, where

+   - $\Omega(x)$: A local patch centered at pixel $x$
+   - $I^c(y)$: Intensity of color channel ccc at pixel $y$