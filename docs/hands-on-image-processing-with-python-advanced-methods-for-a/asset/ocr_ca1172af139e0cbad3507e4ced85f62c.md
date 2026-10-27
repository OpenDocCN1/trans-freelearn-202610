![](bbox=[0.154, 0.067, 0.893, 0.212])

*Figure 9.14 – Image editing with Stable Diffusion models*

Let’s now understand the code in detail. Here are the steps:

+   - **Pipeline setup:** Loads Stable Diffusion v2.1 with a DiffEdit wrapper.
+   - **Base image:** The original image (a bowl of fruits).
+   - **Prompts:** The source prompt (“a bowl of fruits”) guides the mask generation; the target prompt (“a bowl of pears”) specifies the edit.
+   - **Mask generation:** The model automatically infers a mask of regions that differ between source and target prompts.
+   - **Latent inversion:** The original image is mapped into the latent space using DDIM inversion.
+   - **Editing step:** The pipeline replaces masked regions with content aligned to the target prompt while keeping the rest unchanged.
+   - **Visualization:** Shows the original, the mask, and the edited output side by side.

This allows powerful, prompt-driven edits with minimal user input.

> **Note**
> 
> DDIM inversion approximately reverses the DDIM generation process, mapping a real image into the diffusion model’s latent noise trajectory and thereby enabling structure-preserving, prompt-driven image editing. While **Denoising Diffusion Probabilistic Models (DDPM)** relies on a stochastic Markovian denoising process (to be covered in detail in *Chapter 13*), **Denoising Diffusion Implicit Models (DDIM)** employs a deterministic non-Markovian formulation that achieves comparable image quality with substantially fewer sampling steps.

# Image inpainting with Diffusion models

Inpainting aims to *restore missing or corrupted parts of an image*. Unlike editing, the mask is explicitly provided by the user, and the model must fill the hole with semantically consistent content. The probabilistic formulation is:

$$\hat{x} = \underset{x}{\arg\max}\ p(x \mid y, M, p),$$

where $M$ defines the missing region and $p$ is the guiding prompt describing what should fill the region.