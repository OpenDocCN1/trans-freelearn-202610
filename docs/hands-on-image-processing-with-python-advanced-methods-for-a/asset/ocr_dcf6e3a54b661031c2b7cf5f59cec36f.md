```
pipe.to('cuda')
# torch_dtype=torch.float16
```

This pipeline contains a CLIP text encoder, a latent-space *VAE*, a *U-Net* denoiser, and a diffusion scheduler. Using half-precision (float16) reduces memory consumption and accelerates inference on modern GPUs.

**Text-to-Image Generation:** The core image-generation step is:

```
img = pipe(prompt, num_inference_steps=30).images[0]
```

Given a textual description, the pipeline starts from random latent noise and performs 30 reverse-diffusion steps to synthesize an image matching the prompt. Mathematically, the process can be summarized as:

$z_T \sim \mathcal{N}(0,I) \rightarrow z_{T-1} \rightarrow \cdots \rightarrow z_0 \rightarrow x,$

where each denoising step is guided by the prompt embedding.

**Generating Images from Multiple Prompts:** The demonstration generates images for four distinct prompts.

```
prompts = ["A futuristic city", "A medieval castle",
           "A cyberpunk street", "A photorealistic tiger"]
```

Each prompt is processed independently.

```
for prompt in prompts:
    img = pipe(prompt, num_inference_steps=30).images[0]
```

The resulting images illustrate the remarkable semantic understanding learned by Stable Diffusion. Despite starting from pure noise, the model synthesizes visually coherent scenes corresponding to futuristic architecture, historical structures, cyberpunk environments, and realistic wildlife.

![](bbox=[0.143, 0.641, 0.904, 0.783])

*Figure 13.22 — Text-to-image generation with Stable Diffusion*

The generated outputs clearly show that different textual prompts guide the model toward distinct semantic regions of the visual space, producing correspondingly different images (e.g., urban futuristic scenes, historical castles, cyberpunk streets, and realistic animals), thereby demonstrating strong text-conditioned control in diffusion-based image synthesis.