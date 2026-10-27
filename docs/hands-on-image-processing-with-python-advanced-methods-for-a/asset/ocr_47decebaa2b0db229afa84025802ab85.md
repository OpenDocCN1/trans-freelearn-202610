The diffusion model consists of the following processes (we shall explore more on this in the last chapter).

+   1. **Forward (noising) process**: A diffusion model defines a forward Markov chain that progressively corrupts a clean image $x_0$ into noise:
+   
+   $$q(x_t \mid x_{t-1}) = \mathcal{N}(x_t; \sqrt{1-\beta_t}\,x_{t-1},\,\beta_t I),$$
+   
+   for timesteps $t = 1, ..., T$.
+   
+   Closed form:
+   
+   $$x_t = \sqrt{\bar{\alpha}_t}\,x_0 + \sqrt{1-\bar{\alpha}_t}\,\epsilon, \quad \epsilon \sim \mathcal{N}(0,I),$$
+   
+   where $\alpha_t = 1 - \beta_t$ and $\bar{\alpha}_t = \prod_{s=1}^{t} \alpha_s$.

+   2. **Learned reverse (denoising) process**: A neural network $\epsilon_\theta(x_t, t, \text{cond})$ is trained to predict the added noise $\epsilon$. Sampling uses a learned reverse step to recover $x_{t-1}$ from $x_t$. One popular update (simplified) is:
+   
+   $$\hat{x}_{t-1} = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{1-\alpha_t}{\sqrt{1-\bar{\alpha}_t}}\,\epsilon_\theta(x_t, t, \text{cond})\right) + \sigma_t z,$$
+   
+   with $z \sim \mathcal{N}(0,I)$ and $\sigma_t$ a schedule-dependent noise term.

To steer sampling toward a condition (e.g., a text prompt $p$), **classifier-free guidance** forms an adjusted noise prediction:

$$\epsilon_{\text{guided}} = \epsilon_\theta(x_t, t \mid \text{cond}) + w\big(\epsilon_\theta(x_t, t \mid \text{cond}) - \epsilon_\theta(x_t, t \mid \emptyset)\big)$$

where $w$ is the guidance scale. Larger $w$ strengthens the condition (more faithful to the prompt) but may reduce

# How inpainting works conceptually with Stable Diffusion

*Stable Diffusion inpainting (which we shall revisit in chapter 13) typically operates in latent space:*

+   - The image is encoded to a latent $z_0$ by a **variational autoencoder (VAE)**:
+   
+   $$z_0 = \text{Enc}(x_0)$$

+   - To inpaint only masked regions, the pipeline constructs a noisy latent in which only the masked region is noised, while the known unmasked region is preserved (or used as conditioning). A common approach produces a fully noised latent $z_T$ consistent with masked region, or combine the original latent in unmasked areas and a noisy latent in masked areas:
+   
+   $$z_t = z_t^{\text{noisy}} \odot \text{mask} + z_0 \odot (1 - \text{mask})$$

so the model only needs to generate plausible content inside mask while keeping the rest unchanged.