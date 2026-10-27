# Image inpainting

Inpainting fills missing or undesired image regions using contextual information from surrounding pixels.

```
pipe = StableDiffusionInpaintPipeline.from_pretrained(
    "runwayml/stable-diffusion-inpainting").to("cuda")
image = Image.open('man.jpg').convert('RGB')
mask = Image.open('man_mask.png').convert('RGB')
result = pipe(prompt="natural background", image=image, mask_image=mask).images[0]
```

The mask identifies the region to be modified, while the diffusion model generates content that is consistent with the surrounding scene. Applications include object removal, image repair, and content-aware editing.

![](bbox=[0.085, 0.375, 0.32, 0.575])

![](bbox=[0.35, 0.375, 0.585, 0.575])

![](bbox=[0.625, 0.375, 0.86, 0.575])

Figure 13.33 – Image inpainting with Stable Diffusion

# Artistic style generation

Diffusion models can emulate a wide range of artistic styles through prompt engineering alone.

```
from diffusers import StableDiffusionPipeline
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5", torch_dtype=torch.float16).
to("cuda")
styles = ["oil painting", "anime illustration", "pixel art", "pencil sketch"]
for style in styles:
    prompt = (
        f"A majestic castle on a mountain, "
        f"{style}"
    )
    img = pipe(prompt, num_inference_steps=30, guidance_scale=8).images[0]
```