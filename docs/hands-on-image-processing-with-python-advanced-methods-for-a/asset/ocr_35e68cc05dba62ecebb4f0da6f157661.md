# Mathematical framing

Let $x \in \mathbb{R}^{H\times W \times 3}$ be the unknown clean image and $m \in \{0,1\}^{H\times W}$ be a binary mask with $m(p)=1$, where the pixel $p$ is missing (to be inpainted). The observed (masked) image is:

$$ y = (1 - m) \odot x + m \odot v, $$

where $\odot$ is elementwise multiplication and $v$ is arbitrary filler (e.g., zeros).

The inpainting problem is to estimate $\hat{x}$ from $y$ and $m$, i.e.

$$ \hat{x} = f_\theta(y, m), $$

where $f_\theta$ is a learned generator (the LaMa model).

Training minimizes a combination of reconstruction and perceptual objectives over many instances $(x, m)$:

$$ \mathcal{L}(\theta) = \lambda_{\text{pix}} \| (1 - m) \odot f_\theta(y, m) - (1 - m) \odot x \|_1 + \lambda_{\text{perc}} \mathcal{L}_{\text{perc}}(f_\theta(y, m), x) + \lambda_{\text{adv}} \mathcal{L}_{\text{adv}}(\cdot), $$

optionally with style or frequency losses. The crucial architectural change in LaMa is replacing or augmenting standard convolutions with global/Fourier-based operations, so $f_\theta$ can mix information from the entire image (not just a local receptive field) in a parameter-efficient way.

# What makes LaMa work

The following are the reasons why LaMa works:

+   - **Global receptive field without huge parameter/depth cost:** Fourier-domain operations let the network model long-range correlations (e.g., horizon lines, repeating patterns) cheaply. In spatial terms, this is like having extremely large convolutional kernels but implemented via elementwise multiplications in frequency space.

+   - **Resolution robustness:** Because LaMa reasons in frequency space and is trained with multi-scale augmentations, the same model generalizes to different image sizes without retraining; in practice, it handles very large masks and high-res inputs better than local-only methods.

+   - **Practical loss mix:** Perceptual (VGG) losses + pixel losses + optional discriminator encourage both structure and visual realism.

+   - **Simplicity at inference:** Once trained, LaMa is a single forward pass (no iterative optimization) and is fast for reasonable image sizes.

The next code snippet uses the function SimpleLama from the library simple_lama_inpainting to inpaint an input image given an inpainting mask and plots the inpainted image along with the original image and the inpainted images side-by-side:

```
from simple_lama_inpainting import SimpleLama
simple_lama = SimpleLama()
img_path = "images/jump_orig.jpg"
mask_path = "images/jump_mask.jpg"
image = Image.open(img_path)
```