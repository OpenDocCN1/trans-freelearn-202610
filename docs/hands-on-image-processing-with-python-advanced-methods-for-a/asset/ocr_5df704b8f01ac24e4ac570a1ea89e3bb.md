However, it can hallucinate plausible but incorrect content (semantics may not match the original if promptless). May produce a slight color/lighting mismatch at the mask boundary, so feathering helps. Geometry-heavy structures (e.g., man-made straight lines, faces) may require semantic-aware constraints or additional conditioning (e.g., edge maps or reference patches).

**When not to use:** When you must strictly preserve original pixels in the hole (forensic or legal use), generative inpainting is not appropriate. When exact pixel recovery is needed (e.g., scientific images), classical restoration plus model-based approaches might be preferable.

# Inpainting and image editing

Image editing and inpainting represent two of the most exciting applications of generative deep learning models in computer vision. Unlike classical restoration tasks that aim to remove degradation (e.g., blur, noise, haze), editing and inpainting allow for semantic-level modifications to an image. Editing modifies parts of an image based on natural-language prompts, while inpainting restores missing or corrupted regions by synthesizing plausible content. Both tasks are typically modeled as conditional image generation problems, where the output image must be faithful both to the observed pixels (outside the mask) and to the semantic prompt provided by the user.

At their core, these tasks involve solving the following inverse problem:

$$y = M \odot x + (1 - M) \odot n,$$

where $y$ is the observed corrupted or incomplete image, $x$ is the true (restored) image we aim to recover, $M$ is a binary mask indicating known regions ($M = 1$) and missing/edited regions ($M = 0$), and $n$ is the unknown content to be hallucinated or replaced. The goal is to estimate $x$ such that:

$$x = \arg\max_x p(x \mid y, M, \text{prompt}),$$

where the prompt provides semantic or textual conditioning. Diffusion-based generative models (e.g., Stable Diffusion, Kandinsky) make this feasible by modeling the probability distribution $p(x)$ of natural images and sampling realistic completions. Let’s now explore both image editing and image inpainting using state-of-the-art diffusion models.

# Image editing with Diffusion models

Image editing refers to targeted modifications of existing content in an image, such as replacing an object or altering style. Diffusion models enable this by providing a way to encode an image into a latent space, modify specific regions with masks, and then decode a new version consistent with both the source image and target prompt. Mathematically, the problem can be expressed as:

$$\hat{x} = \arg\max_x p(x \mid y, M, p_t, p_s),$$

where $y$ is the input image, $M$ is the region to modify (mask), $p_t$ is the target text prompt, and $p_s$ is the source prompt (negative guidance: “don’t keep the old content”).