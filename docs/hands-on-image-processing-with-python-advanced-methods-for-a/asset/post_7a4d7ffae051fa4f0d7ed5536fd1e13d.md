
```python
result = client.images.edit(model="gpt-image-1", image=open(image, "rb"),
    mask=open("images/mask.png", "rb"),
    prompt="Replace edges with artistic glowing lines")
return Image.open(io.BytesIO(base64.b64decode(result.data[0].b64_json)))
```

This demonstrates a hybrid workflow in which classical image processing provides structural guidance while generative AI synthesizes new visual content. The Gradio application combines OpenCV edge detection with GPT Image inpainting to automatically replace image edges with AI-generated glowing artistic contours through an interactive web interface.

```python
import gradio as gr

gr.Interface(fn=edge_inpaint, inputs=gr.Image(type="filepath"),
    outputs=gr.Image(),
    title="OpenCV + GenAI Hybrid").launch()
```

The next figure shows the Gradio interface for an edge-guided image generation:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_726_0.png)

Figure 13.40 – Gradio app for edge-guided image editing with GPT image

## Image inpainting

Inpainting fills missing or masked image regions while preserving surrounding content.

```python
result = client.images.edit(model="gpt-image-1", image=img_f, mask=mask_f,
    prompt="Fill the masked region realistically")
```

The mask identifies regions to be modified, and the model synthesizes visually consistent content, as demonstrated by the Gradio app shown below (refer to GitHub).

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_727_0.png)

Figure 13.41 – AI inpainting tool with Gradio with GPT-image model (OpenAI API)

Typical applications include object removal, scratch removal, damage restoration, background completion, and content replacement.

## AI style transfer

GPT Image can transform an image into a desired artistic style while preserving varying degrees of structural information. The function `build_prompt()` in the next Gradio demo dynamically adjusts the prompt according to a structure-preservation parameter. Low values allow creative reinterpretation, while high values preserve geometry, object layout, and perspective. The function `style_transfer()` applies the selected artistic style to an input image using GPT Image Editing while enforcing the desired level of structural preservation.

```python
client = OpenAI(api_key=api_key)

def build_prompt(style, strength):
    if strength < 0.3:
        return f"""Transform this image into {style} style.
        Feel free to creatively reinterpret the scene."""
    else:
        return (f"Apply {style} style. Preserve composition,"
                f"geometry, object positions and perspective."
                f"Only modify colors, textures and lighting.")

def style_transfer(image_path, style, strength):
    prompt = build_prompt(style, strength)
    result = client.images.edit(model="gpt-image-1",
                                image=open(image_path, "rb"), prompt=prompt)
    img_bytes = base64.b64decode(result.data[0].b64_json)
    return Image.open(io.BytesIO(img_bytes))
```

The Gradio app interface used to apply Ghibli style looks like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_728_0.png)

Figure 13.42 – Gradio app generating Ghibli-style image from an input image with GPT image

Supported styles include anime, watercolor, oil painting, cyberpunk, pencil sketch, Studio Ghibli, and Van Gogh. This approach provides a controllable form of style transfer without requiring a separate style-transfer network.

## Face restoration

Vision and image-generation models can be combined into a two-stage restoration pipeline.

### Stage 1: Image Assessment

GPT Vision first analyzes image quality. The model estimates blur, noise, scratches, and restoration requirements for the input old photo.

```python
response = client.responses.create(model="gpt-4.1",
    input=[{"role": "user", "content": [
        {"type": "input_text", "text": """Analyze this image. Estimate:
blur level,
    noise level, scratches, restoration needs"""},
        {"type": "input_image", "image_url": f"data:image/jpeg;base64,{base64.b64encode(img_bytes).decode()}"}
    ]}])
```

```python
analysis = response.output_text
print("\nGPT Vision Analysis:")
print(analysis)
```

> **Summary**:
> This is a heavily blurred vintage image with low noise and no obvious scratches. Restoration would mainly focus on improving sharpness, contrast, and reconstructing key facial features to bring back clarity and character to the photograph.

### Stage 2: Restoration

```python
result = client.images.edit(model="gpt-image-1", image=[("old_photo.jpg", img_bytes)], prompt="""Restore this photograph. Remove blur. Remove noise.

Improve facial detail. Preserve identity.""")
```

The model enhances facial details while preserving the original identity and overall appearance.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_729_0.png)

Figure 13.43 – Face restoration with GPT vision

Applications include historical photograph restoration, family photo enhancement, archive digitization, and face reconstruction.

## Multi-image composition

GPT Image can jointly reason over multiple input images, as shown in the next code:

```python
result = client.images.edit(model="gpt-image-1",
    image=[open("images/beach.png", "rb"), open("images/whale.jpg", "rb")],
    # image=[open("images/giraffe.jpg", "rb"), open("images/bird.jpg", "rb")]
    prompt="Combine these into a realistic product scene")
```

The model merges visual concepts from multiple sources (whale and beach, giraffe and bird) into a coherent composition, as shown in the next figure.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_730_0.png)

Figure 13.44 – Multi-image composition with GPT-image

Applications include advertising design, product visualization, concept art, and composite scene creation.

## E-Commerce product enhancements

Product images can be automatically transformed into professional marketing photographs.

```python
result = client.images.edit(model="gpt-image-1", image=f,
    prompt="""Place this product in a professional
    studio background with soft lighting""")
```

The model performs virtual photography by improving backgrounds, lighting, reflections, and presentation quality, as in the next Gradio app.

### E-commerce Product Generator

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_730_1.png)

Figure 13.45 – Gradio App for E-commerce product enhancement with GPT-image

## Visual Question Answering (VQA)

Vision models can answer arbitrary questions about an image. The function `analyze()` in the next code snippet sends an image and a natural-language question to a multimodal GPT model and returns the model’s visual understanding of the image.

```python
def analyze(image, question):
    with open(image, "rb") as f:
        b64 = base64.b64encode(f.read()).decode()
    response = client.responses.create(model="gpt-4.1-mini",
        input=[{"role": "user", "content": [
            {"type": "input_text", "text": question},
            {"type": "input_image", "image_url": f"data:image/jpeg;base64,{b64}"}
        ]}])
    return response.output_text
```

The Gradio app interface demonstrating VQA is shown in the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_731_0.png)

Figure 13.46 – Gradio App for Visual Question-Answering System with GPT-Image

> **Note**
>
> The three essential steps above: **encode image → send image and question to GPT → return the answer.** This capability is known as **Visual Question Answering (VQA)**, demonstrated by the next Gradio app.

## Automated industrial inspection (detecting manufacturing defects)

GPT Vision can function as a quality-control assistant. The next code uses a multimodal GPT model as an automated quality-control inspector to analyze an image of capsules for *manufacturing defects* and return a structured JSON inspection report containing detected defects, locations, severities, confidence scores, and an overall quality assessment.

```python
with open("images/capsules.png", "rb") as f:
    image_b64 = base64.b64encode(f.read()).decode()

prompt = """You are an industrial quality-control inspector. Analyze the image carefully. Look for:
Scratches, Cracks, Corrosion, Missing parts, Deformation, Surface
contamination, Paint defects,
Manufacturing anomalies. Return ONLY valid JSON:
{"defect_found": true,  "defects": [{"type": "",  "location": "", "severity": "",
"confidence": 0, "description": ""}],  "overall_quality": "",  "inspection_summary": ""}"""

response = client.responses.create(model="gpt-4o", input=[{"role": "user",
    "content": [{"type": "input_text", "text": prompt},
                {"type": "input_image",
                 "image_url": f"data:image/jpeg;base64,{image_b64}"}]}])
result = response.output_text
print(result)
```

The function `base64.b64encode(...)` converts the image into a text format suitable for API transmission.

`prompt` defines the inspection task and specifies the required JSON output format. The function `client.responses.create(...)` sends the image and inspection instructions to the GPT vision model. Here, `"role": "user"` again indicates that the prompt and image are user inputs to the model, `"type": "input_text"` provides the defect-inspection instructions, and `"type": "input_image"` supplies the capsule image to be analyzed, where `response.output_text` retrieves the model's inspection report in JSON format and `print(result)` displays the generated quality-control report.

Results are returned as structured JSON.

```json
{
    "defect_found": true,
    "defects": [{"type": "crack", "location": "first capsule", "severity": "high", "confidence": 0.95,
        "description": "Visible crack running down the length of the capsule."}, ...]
}
```

The subsequent visualization code (refer to GitHub) converts the JSON output into a dashboard showing defect locations, severity levels, confidence scores, and inspection summaries.
