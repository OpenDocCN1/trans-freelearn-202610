The resulting images exhibit variations in pose, composition, lighting, and object appearance, demonstrating the stochastic nature of diffusion-based generation.

# Image-to-image refinement with SDXL-Turbo

A useful workflow is to first generate a rough concept and then refine it through *image-to-image diffusion*.

**Stage 1 – Concept Creation:** The text-to-image (pipe_t2i) pipeline is used to generate the

```
concept_prompt = """fantasy dragon city, flying dragons, ancient towers,
 fantasy landscape, concept art, cinematic lighting"""
pipe_t2i = AutoPipelineForText2Image.from_pretrained("stabilityai/sdxl-turbo",
    torch_dtype=torch.float16,
    variant="fp16")
concept = pipe_t2i(prompt=concept_prompt, num_inference_steps=4).images[0]
```

**Stage 2 – Refinement:** The image-to-image pipeline (pipe_i2i) starts from an existing image and modifies it according to a text prompt:

$$x_{out} = G(x_{in}, y)$$

where $x_{in}$ is the input image and $y$ is the textual instruction (*text prompt*).

```
detail_prompt = """highly detailed fantasy dragon city, ultra detailed
architecture,
    cinematic lighting, fantasy masterpiece, sharp focus, intricate textures"""
pipe_i2i = AutoPipelineForImage2Image.from_pretrained("stabilityai/sdxl-turbo",
    torch_dtype=torch.float16,
    variant="fp16")
refined = pipe_i2i(prompt=detail_prompt, image=concept, strength=0.35).
images[0]
```

The first stage establishes the overall design, while the second stage introduces architectural details, textures, lighting effects, and higher visual fidelity. This hierarchical workflow is widely used in concept art, game design, and architectural visualization.

![](bbox=[0.24, 0.75, 0.8, 0.92])

*Figure 13.29 – Image-to-image diffusion with SDXL*