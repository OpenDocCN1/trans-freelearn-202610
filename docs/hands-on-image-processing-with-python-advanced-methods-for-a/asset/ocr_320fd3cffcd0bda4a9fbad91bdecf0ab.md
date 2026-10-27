Diffusion-based inpainting models (e.g., Kandinsky 2.2 inpainting) are trained with masks applied randomly during training, forcing the model to hallucinate realistic completions consistent with both context and prompts. The next code snippet implements image inpainting with diffusion:

```
from diffusers import AutoPipelineForInpainting
from diffusers.utils import load_image, make_image_grid

# Load Kandinsky inpainting pipeline
pipeline = AutoPipelineForInpainting.from_pretrained(
    "kandinsky-community/kandinsky-2-2-decoder-inpaint",
    torch_dtype=torch.float16)
pipeline.enable_model_cpu_offload()
pipeline.enable_xformers_memory_efficient_attention()  # optional optimization
init_image = load_image("https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/inpaint.png")
mask_image = load_image("https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/inpaint_mask.png")
prompt = "a black cat with glowing eyes, cute, adorable, disney, pixar, highly detailed, 8k"  # Prompt for inpainting
negative_prompt = "bad anatomy, deformed, ugly, disfigured"
image = pipeline(  # Run inpainting
    prompt=prompt, negative_prompt=negative_prompt,
    image=init_image, mask_image=mask_image).images[0]
# Show results
make_image_grid([init_image, mask_image, image], rows=1, cols=3)
```

If you run the above code, you should obtain a figure like the following:

![](bbox=[0.09, 0.61, 0.86, 0.79])

Figure 9.15 – Image inpainting with Stable Diffusion models