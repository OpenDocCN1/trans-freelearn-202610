
```text
"The Taj Mahal floating in the sky...",
"A giraffe riding a bicycle on a busy Tokyo street...",
...
]
```

The prompt set intentionally combines famous landmarks, historical reimaginations, impossible scenarios, anthropomorphic animals, science-fiction concepts, and creative artistic compositions. Such prompts provide an effective benchmark for assessing semantic understanding and visual creativity.

**Automated Comparison Pipeline:** For each prompt, the program performs:

```python
dalle2_imgs = generate_dalle2(prompt)
dalle3_imgs = generate_dalle3(prompt)
```

The resulting images are arranged into a comparison grid, where the first row corresponds to DALL·E 2 outputs and the second row corresponds to DALL·E 3 outputs. This side-by-side visualization makes it possible to compare prompt fidelity, composition quality, scene complexity, object consistency, artistic realism, and output diversity for the same textual description.

**Downloading and Displaying Results:** The helper function

```python
def get_image(url):
    response = requests.get(url)
    return Image.open(io.BytesIO(response.content))
```

downloads generated images from the URLs returned by the API and converts them into PIL images for visualization. The figures are then saved automatically.

```python
plt.savefig(out_path, dpi=150)
```

allowing large-scale benchmarking across many prompts.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_721_0.png)

*Figure 13.36 – Image generation with DALL·E 2 vs. DALL·E 3 with same prompt*

DALL·E 3 generates more detailed and prompt-aligned images than DALL·E 2 (with the same set of prompts), reflecting improved semantic understanding and visual coherence, as shown in the last figure and the next figure, with two different prompts.

**Prompt 2:** The Eiffel Tower submerged underwater with scuba divers swimming around.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_722_0.png)

*Figure 13.37 – Image generation with DALL·E 2 vs. DALL·E 3 with the same prompt*

## DALL·E 2 versus DALL·E 3: The experiment typically reveals several qualitative differences:

DALL·E 2 remains useful for rapid image generation and experimentation, while DALL·E 3 generally produces more faithful and visually coherent results, particularly for long prompts involving multiple objects, detailed spatial arrangements, and narrative descriptions.

### Note

An LLM (Large Language Model) is a neural network trained on large-scale text data to understand and generate human language. Modern multimodal LLMs can also interact with images, audio, and video. Notably, DALL·E and GPT-Image are not LLMs; they are specialized proprietary OpenAI image-generation models (distinct from Stable Diffusion, but they are believed to combine transformer-based architectures with diffusion-based image generation) that work alongside LLMs such as ChatGPT to perform image generation, editing, inpainting, outpainting, and restoration-related tasks through natural-language prompts. Several companies provide multimodal AI systems with varying strengths, as summarized in the next table. In this chapter, we focus exclusively on OpenAI’s DALL·E and GPT-Image models because of their strong image-generation and editing capabilities, ease of use through APIs, and relevance to computer vision and image processing applications.

The next table summarizes a few popular LLMs and image generation models:

| Company | AI System | Strengths | Limitations |
|---|---|---|---|
| OpenAI | ChatGPT + GPT-Image / DALL·E | Excellent image generation, editing, inpainting, instruction following, API support | Less specialized for scientific image restoration |
| Google | Gemini | Strong multimodal reasoning and ecosystem integration | Image editing capabilities vary by version |
| Anthropic | Claude | Excellent long-context reasoning and document understanding | Limited native image generation features |
| xAI | Grok | Real-time information access and multimodal interaction | Smaller image creation ecosystem |
| Microsoft | Copilot | Productivity-tool integration and accessibility | Depends heavily on underlying foundation models |

*Table 13.2 – Comparison of major multimodal AI systems for image-related tasks*

Moving beyond model-specific generation with **DALL·E 2** and **DALL·E 3**, the next section explores broader image generation and editing capabilities provided through **GPT Image APIs** for more flexible and integrated multimodal workflows.

## GPT Image APIs: generation, editing, inpainting, and vision

The OpenAI ecosystem provides a unified multimodal framework for image understanding and image generation. Vision models such as GPT-4.1, GPT-4o, and GPT-5.1 can analyze images, answer visual questions, compare multiple images, and generate structured reports, while GPT Image models can synthesize, edit, restore, recolor, and transform images through natural-language instructions. Unlike traditional diffusion pipelines that require specialized architectures such as ControlNet, IP-Adapter, or segmentation networks, GPT Image APIs expose these capabilities through a single prompt-driven interface.

**Setup:** The OpenAI API requires an API key obtained from the OpenAI platform. After creating an account and adding billing information, an API key can be generated from the developer dashboard and supplied to the client:

```python
from openai import OpenAI
api_key = "<your_api_key>"
client = OpenAI(api_key=api_key)
```

For image understanding tasks, images are typically encoded as Base64 strings and passed to a vision model, while image generation and editing tasks use the GPT Image API.

### Image understanding with GPT vision

Modern GPT models can jointly process text and images. An image is encoded in `Base64` format and supplied alongside a textual query.

```python
image_path = "images/family.png"
base64_image = encode_image(image_path)
data_url = f"data:image/jpeg;base64,{base64_image}"
response = client.responses.create(model="gpt-4.1-mini",
    input=[{"role": "user",
    "content": [{"type": "input_text", "text": "what's in this image?"},
                {"type": "input_image", "image_url": data_url}] }])
```

For the following input image, the following description is generated:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_724_0.png)

*Figure 13.38 – Image understanding with GPT vision*

```python
print(response.output_text)
```

```text
The image shows a group of five people sitting indoors. Among them, there are three elderly women and one elderly man, all dressed in traditional Indian attire, including sarees for the women and a checked shirt for the man. There is also a younger man standing at the right side of the group wearing a light blue polo shirt. The scene appears to be a family or social gathering. The room has a window in the background, and the lighting suggests the picture was taken indoors during the evening or night.
```

The model performs visual scene understanding and generates a natural-language description of the image contents. Typical applications include image captioning, visual search, accessibility tools, document understanding, and image indexing.

### Image editing with GPT Image

GPT Image models support instruction-based image editing. Instead of generating an image from scratch, the model modifies an existing image according to a prompt.

```python
result = client.images.edit(model="gpt-image-1", image=open("cat.jpg","rb"),
                             prompt="Enhance lighting and make it cinematic")
```

The model preserves the image structure while adjusting illumination, colors, textures, and overall appearance according to the instruction, as shown below with the input cat image and edited image side-by-side.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_725_0.png)

*Figure 13.39 – Image editing with GPT-image*

Applications include photo enhancement, relighting, color correction, artistic transformation, and content modification.

### Edge-Guided editing

Traditional computer vision can be combined with generative AI. In this example, OpenCV extracts edges using the Canny detector. The edge map is converted into a mask and supplied to GPT Image using the `create_edge_mask()` function (refer to GitHub). The function `edge_inpaint()` uses the edge mask as an *editing region specification*; it sends the original image together with the mask to `client.images.edit()`, instructing the model to modify only the masked edge pixels, to replace the detected edges with glowing artistic lines, and returns the AI-edited image.

```python
def edge_inpaint(image):
    mask = create_edge_mask(image)  # find edges (Canny), convert to RGBA mask
```
